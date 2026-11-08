---
annotator: qwen-turbo
temperature: 0.3
response_format: json_object
request_timeout_s: 180
max_retries: 10
batch: 2 episodes x 24 steps (all steps of each labeled day)
contrastive_pairs: 4
---

You are annotating EV charging / distribution-grid scheduling behavior for reward-model training.

# Batch Calibrated Preference Annotation Rubric

Use one shared scoring scale across the whole batch before assigning scalar labels.

- +7 to +10: clearly excellent. Target SOC is reached, service quality is good, voltage is safe, or the step clearly repairs an earlier severe service problem.
- +3 to +6: good. SOC gap shrinks materially, unmet energy decreases, near-departure EVs receive timely charging, or voltage violation is mitigated.
- -1 to +2: neutral or mild tradeoff. The state is stable, the issue is mild, or cost/service/voltage tradeoffs are acceptable.
- -3 to -5: moderate problem. SOC is not yet attained, voltage is biased, or cost-service tradeoff is poor, but there is still recovery room.
- -6 to -8: severe problem. Service is clearly insufficient, unmet SOC is high, time to departure is short, or voltage violation is clear.
- -9 to -10: extreme failure. Reserve for the worst small minority, such as near-departure severe undercharge combined with severe voltage violation or persistent service failure.

Important constraints:
1. Do not put every risky state into -8 or -9.
2. -9 to -10 can only be assigned to the most severe small minority inside a batch.
3. If a step improves SOC gap, unmet energy, or voltage violation relative to nearby states, it must score higher than a comparable worsening step, even if it has not fully recovered.
4. A non-catastrophic batch should contain positive, neutral, moderate-negative, and severe-negative labels when such examples exist.
5. If every step in a batch is negative, explain why there is no service recovery or acceptable tradeoff.
6. Neutral baseline states are not excellent labels. If target_reached=0, protected_good_anchor=0, soc_gap_shrink=0, unmet_decrease=0, voltage_mitigation=0, and expert_direction=neutral, then overall_score should not be excellent.
7. Low operating cost or safe voltage can move a neutral baseline slightly positive, but cannot move it to an excellent score without service-recovery, target-reached, voltage-mitigation, or protected-anchor evidence.

Hard anchor rules:
1. If target_reached=1 and there is no severe voltage problem, overall_score must be >= +3.
2. If soc_gap_shrink=1 and voltage did not worsen, overall_score must be >= 0.
3. If unmet_decrease=1 or a fast-departure vehicle receives timely charging, overall_score should be >= 0.
4. Severe near-departure shortfall should be <= -6.
5. Severe voltage violation should be <= -5 unless voltage_mitigation=1 and the step is clearly repairing voltage.
6. The batch input includes protected_good_anchor. If protected_good_anchor=1, overall_score must be >= 0.
7. If a recovery flag is present but protected_good_anchor=0, it is an exception case such as severe near-departure shortfall, severe voltage, or extreme cost. It may be negative only if label_reason names the exception.
8. Treat voltage_violation_severity < 0.09 as not severe. It may still reduce voltage_score, but by itself it is not enough to make a protected_good_anchor negative.
9. Treat voltage_violation_severity >= 0.10 as severe and >= 0.13 as extreme.
10. If voltage_mitigation=1 and voltage_violation_severity < 0.09, overall_score must be >= 0 unless operating_cost is extreme.
11. Do not rely on the input rank_group alone. The explicit anchor and exception flags override the rank_group when deciding whether a step may be negative.

Score relationship:
- service_score measures EV charging service quality.
- voltage_score measures grid safety.
- cost_score measures operating-cost reasonableness.
- overall_score is service-dominant, but it must still account for voltage and cost.
- A high service_score does not automatically imply a high overall_score if voltage is severely unsafe or cost is extremely unreasonable.

Cost discipline (v6 addendum, used only when key_steps carry
cost_ratio_vs_reference, cost_p50_reference, cost_p90_reference,
delta_operating_cost, and cost_spike_unjustified):

12. cost_ratio_vs_reference = operating_cost / cost_p50_reference (the
    scenario-period median cost from a reference policy). 1.0 is the
    reference. > 2.0 is high. > 3.0 is extreme. Use this absolute ratio
    as the cost trade-off signal, NOT just the intra-batch cost_q90.
13. If cost_ratio_vs_reference > 2.0 and the step has no voltage_mitigation,
    no target_reached, and no protected_good_anchor, then overall_score
    must not be greater than +1, even if soc_gap_shrink=1 or
    unmet_decrease=1. Service progress paid for by expensive grid imports
    is not a positive label.
14. If cost_ratio_vs_reference > 3.0 and there is no recovery flag
    (target_reached / voltage_mitigation / protected_good_anchor), then
    overall_score must be <= -3.
15. cost_spike_unjustified = 1 marks a step whose cost is at least 2x the
    scenario reference AND has no voltage_mitigation AND no
    terminal_shortfall_risk AND no target_reached. Such a step must not
    score above 0.
16. A cost ramp (delta_operating_cost > 0.5 * cost_p90_reference) is
    evidence to reduce cost_score even when overall_score remains positive
    due to service. cost_score in this case should be at most 1.
17. Apply cost-discipline clauses 13-16 AFTER applying clauses 1-7. They
    can move a "+5 service-recovery" step down to +1, but they cannot
    cancel target_reached=1 below the +3 floor unless voltage is also
    severely unsafe.


Expert-anchored v5 fields:
Each key step may include expert_direction, expert_reasons, expert_confidence, expert_red_lines, expert_soft_guidance, and the compatibility fields expert_evaluation, expert_score_hint, and expert_priority.
These fields are expert calibration guidance derived from the step metrics and hard anchors, but they are not exact answer keys. Treat them as direction and red-line guidance. The LLM still decides the final scalar label using step metrics, pairwise context, and the expert guidance together.

When expert fields are present:
- Every key step must have an expert evaluation.
- Use expert_direction as the main directional prior: positive, negative, neutral, or mixed.
- Use expert_reasons to identify the major service, voltage, cost, and urgency evidence.
- Use expert_confidence to decide how strict the expert hint is. High-confidence red lines must be followed. Medium/low confidence mixed tradeoffs can be weighed by the LLM.
- Use expert_red_lines as non-negotiable hard constraints unless the step metrics show that the expert generator is clearly wrong.
- Use expert_soft_guidance as interpretation help, not as a precise score.
- The compatibility field expert_score_hint is soft guidance unless expert_confidence=high and a red line applies.
- If the final score differs from expert_direction, explain why in expert_alignment_reason.
- Add expert_alignment and expert_alignment_reason to every scalar_labels item.
- expert_alignment must be one of: followed, soft_exception, red_line_exception.
- Use followed when the label follows the expert direction or red line.
- Use soft_exception when the label departs from soft expert guidance with a reasonable metric or pairwise explanation.
- Use red_line_exception only when the final label intentionally violates a high-confidence red line because the step metrics prove the expert generator is wrong. This will usually fail hard audit.
- If a high-confidence red line forces you to cap or lower a score and you obey that red line, use expert_alignment=followed, not red_line_exception.
- self_check must also include missing_expert_alignment_count, high_confidence_red_line_violation_count, expert_direction_mismatch_count, expert_contradiction_count, and expert_score_hint_violation_count.


Follow this order. Do not jump directly to scalar scores.

Step 1: Episode-level ranking
Rank the episodes from best to worst overall scheduling quality using SOC attainment, unmet SOC, time-to-departure risk, voltage violation, cost tradeoff, and service recovery.

Step 2: Step-group ranking
Within each episode, rank the provided key steps from best to worst service quality and grid safety. Mark each step as one of:
positive_anchor, neutral_tradeoff, moderate_failure, severe_failure.

Step 3: Pairwise comparison
For each contrastive pair, decide which step is better. Prefer the step with smaller SOC gap, longer time to departure, lower unmet SOC, lower voltage severity, lower service deficit, or clearer service recovery.

Step 4: Scalar labels
Only after ranking and pairwise comparison, output scores. If A is judged clearly better than B, A's overall_score must be higher than B's.

Output only JSON with keys:
- episode_level_ranking
- step_group_ranking
- pairwise_comparisons
- scalar_labels
- self_check

Each scalar_labels item must contain:
episode_id, step, service_score, voltage_score, cost_score, overall_score, label_reason, rank_group

If expert fields are present in key_steps, each scalar_labels item must additionally contain:
expert_alignment, expert_alignment_reason

The self_check object must contain:
expected_scalar_labels, actual_scalar_labels, good_anchor_negative_count, pairwise_violation_count, extreme_negative_count, positive_count, neutral_count, negative_count

If expert fields are present in key_steps, self_check must additionally contain:
missing_expert_alignment_count, high_confidence_red_line_violation_count, expert_direction_mismatch_count, expert_contradiction_count, expert_score_hint_violation_count

Before returning JSON, verify:
- actual_scalar_labels equals expected_scalar_labels.
- For every contrastive pair, better step overall_score is greater than worse step overall_score.
- Ties are pairwise violations. If a better step and worse step have equal scores, raise the better step or lower the worse step before returning.
- good_anchor_negative_count counts protected_good_anchor steps with negative overall_score, and it must be 0.
- Recovery steps with protected_good_anchor=0 may be negative only when label_reason names severe shortfall, severe voltage, or extreme cost.
- If expert_direction is neutral and there is no target_reached, soc_gap_shrink, unmet_decrease, voltage_mitigation, or protected_good_anchor evidence, do not assign an excellent score.
- If expert_confidence is high and expert_red_lines contains a rule, obey it and mark expert_alignment=followed. If you intentionally violate the red line because the metrics prove the expert generator is wrong, use expert_alignment=red_line_exception and explain the exact metric conflict.

Batch input:
{batch_json}
