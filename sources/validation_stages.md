# Label-validation checks of the annotation pipeline

Source: `analysis/qwen_batch_calibrated_annotation.py`. The `--validation-mode` switch (`full` = default,
`format_only`, `none`) is the ablation of Fig. 10(e) in the manuscript.

## Format check (stage 1): format / completeness (per batch)

The response must parse as a JSON object with a `scalar_labels` array covering every key step of the batch;
otherwise the batch is retried (`full`, `format_only`).

```python
def parse_json_object(text: str) -> Dict[str, Any]:
    s = strip_json_fences(text)
    try:
        obj = json.loads(s)
        if isinstance(obj, dict):
            return obj
    except json.JSONDecodeError:
        pass

    start = s.find("{")
    end = s.rfind("}")
    if start >= 0 and end > start:
        obj = json.loads(s[start : end + 1])
        if isinstance(obj, dict):
            return obj
    raise ValueError("Could not parse a JSON object from LLM response")
```

```python
def scalar_labels_from_answer(answer: Dict[str, Any]) -> List[Dict[str, Any]]:
    labels = answer.get("scalar_labels")
    if labels is None and "labels" in answer:
        labels = answer["labels"]
    if not isinstance(labels, list):
        raise ValueError("Missing scalar_labels list")
    return labels
```


## Step-consistency check (stage 2): per-step consistency with the operator cue (`full` only triggers retries)

```python
def score_hint_violated(score: float, hint: Any) -> bool:
    hint_s = str(hint or "").strip().lower()
    if hint_s == "positive_ge_3":
        return score < 3
    if hint_s == "non_negative_0_to_6":
        return score < 0
    if hint_s == "neutral_minus1_to_plus2":
        return score < -1 or score > 2
    if hint_s == "mixed_tradeoff_minus3_to_plus3":
        return score < -3 or score > 3
    if hint_s == "mixed_tradeoff_minus4_to_plus3":
        return score < -4 or score > 3
    if hint_s == "mitigated_near_departure_tradeoff_minus5_to_plus3":
        return score < -5 or score > 3
    if hint_s == "moderate_negative_minus3_to_minus5":
        return score < -5 or score > 2
    if hint_s == "severe_shortfall_le_minus6":
        return score > -6
    if hint_s == "severe_voltage_le_minus5":
        return score > -5
    if hint_s == "severe_negative_minus6_to_minus8":
        return score > -6 or score < -8
    if hint_s == "extreme_negative_le_minus8":
        return score > -8
    return False
```

```python
def high_confidence_red_line_violated(score: float, step: Dict[str, Any]) -> bool:
    confidence = str(step.get("expert_confidence", "")).strip().lower()
    if confidence != "high":
        return False
    red_lines = str(step.get("expert_red_lines", "")).lower()
    if "protected_good_anchor must be non negative" in red_lines and score < 0:
        return True
    if "severe near departure shortfall" in red_lines and not step_has_clear_recovery(step) and score > -3:
        return True
    if "severe voltage violation" in red_lines and int(step.get("voltage_mitigation", 0)) != 1 and score > 2:
        return True
    if "neutral baseline should not receive excellent score" in red_lines and not step_has_clear_recovery(step) and score >= 7:
        return True
    return False
```

```python
def expert_direction_mismatched(score: float, step: Dict[str, Any]) -> bool:
    direction = str(step.get("expert_direction", "")).strip().lower()
    if direction == "positive":
        return score < 0
    if direction == "negative":
        return score > 2
    if direction == "neutral":
        return score >= 6 or score < -3
    if direction == "mixed":
        return False
    return False
```


## Dataset-level check (stage 3): batch-level self-check and dataset-level audit

`compute_self_check` counts pairwise-order violations, negative scores on protected good anchors,
missing expert-alignment fields and high-confidence red-line violations; any positive count triggers a retry in
`full` mode. After annotation, `audit_labels` checks the label distribution (positive ≥ 5 %, neutral ≥ 3 %,
extreme-negative ≤ 15 %), parsed-label coverage (≥ 90 % for partial finalize) and the soft violation rate (≤ 10 %).

```python
def compute_self_check(example: Dict[str, Any], labels: List[Dict[str, Any]]) -> Dict[str, Any]:
    label_items = {
        (int(item["episode"]), int(item["step"])): item
        for item in labels
    }
    label_lookup = {key: float(item["overall_score"]) for key, item in label_items.items()}
    key_steps = example["batch_input"]["key_steps"]
    step_lookup = {
        (int(item["episode"]), int(item["step"])): item
        for item in key_steps
    }
    good_anchor_negative_count = 0
    for key, step in step_lookup.items():
        score = label_lookup.get(key)
        if score is None:
            continue
        if "protected_good_anchor" in step:
            is_good_anchor = int(step.get("protected_good_anchor", 0)) == 1
        else:
            is_good_anchor = any(
                int(step.get(name, 0)) == 1
                for name in ("target_reached", "soc_gap_shrink", "unmet_decrease", "voltage_mitigation")
            )
        if is_good_anchor and score < 0:
            good_anchor_negative_count += 1

    pairwise_violation_count = 0
    for pair in example["batch_input"].get("contrastive_pairs", []):
        better = (int(pair["better_episode"]), int(pair["better_step"]))
        worse = (int(pair["worse_episode"]), int(pair["worse_step"]))
        better_score = label_lookup.get(better)
        worse_score = label_lookup.get(worse)
        if better_score is None or worse_score is None or better_score <= worse_score:
            pairwise_violation_count += 1

    scores = [float(item["overall_score"]) for item in labels]
    computed = {
        "expected_scalar_labels": len(key_steps),
        "actual_scalar_labels": len(labels),
        "good_anchor_negative_count": int(good_anchor_negative_count),
        "pairwise_violation_count": int(pairwise_violation_count),
        "extreme_negative_count": int(sum(score <= -8 for score in scores)),
        "positive_count": int(sum(score > 0 for score in scores)),
        "neutral_count": int(sum(-1 <= score <= 2 for score in scores)),
        "negative_count": int(sum(score < 0 for score in scores)),
    }
    if example_has_expert_fields(example):
        missing_expert_alignment_count = 0
        expert_contradiction_count = 0
        expert_score_hint_violation_count = 0
        high_confidence_red_line_violation_count = 0
        expert_direction_mismatch_count = 0
        for step in key_steps:
            key = (int(step["episode"]), int(step["step"]))
            item = label_items.get(key)
            if item is None:
                missing_expert_alignment_count += 1
                expert_contradiction_count += 1
                high_confidence_red_line_violation_count += 1
                continue
            score = float(item["overall_score"])
            alignment = str(item.get("expert_alignment", "")).strip().lower()
            alignment_reason = str(item.get("expert_alignment_reason", ""))
            combined_reason = f"{item.get('label_reason', '')} {alignment_reason}"
            if not alignment:
                missing_expert_alignment_count += 1
            if high_confidence_red_line_violated(score, step):
                high_confidence_red_line_violation_count += 1
                expert_contradiction_count += 1
            if expert_direction_mismatched(score, step):
                expert_direction_mismatch_count += 1
            violated = score_hint_violated(score, step.get("expert_score_hint"))
            exception_named = severe_exception_named(combined_reason)
            if violated and not exception_named:
                expert_score_hint_violation_count += 1
            if alignment in {"red_line_exception"}:
                high_confidence_red_line_violation_count += 1
                expert_contradiction_count += 1
            if alignment in {"overridden", "exception", "partially_followed", "contradicted", "not_followed"} and not exception_named:
                expert_contradiction_count += 1
        computed.update(
            {
                "missing_expert_alignment_count": int(missing_expert_alignment_count),
                "high_confidence_red_line_violation_count": int(high_confidence_red_line_violation_count),
                "expert_direction_mismatch_count": int(expert_direction_mismatch_count),
                "expert_contradiction_count": int(expert_contradiction_count),
                "expert_score_hint_violation_count": int(expert_score_hint_violation_count),
            }
        )
    return computed
```

