**Preference-specific priority sentence.** For each operator preference, the annotation pipeline places a priority sentence at the start of the system prompt, ahead of the generic instructions. The line "overall_score is service-dominant" in the template above is the service-first setting; for the voltage-first and cost-first preferences, the priority sentence states which score dominates and sets the corresponding caps. The cost-first sentence uses `cost_ratio_vs_reference` (rules 12-17 of the template) as its main evidence. For the voltage-first, service-first, and cost-first runs, the priority sentence sets the voltage-severity bands (severe at 0.06 or above, extreme at 0.10 or above) in place of the default bands of template rules 8-9, and the 0.95-1.05 p.u. range in the voltage-first sentence is the annotation reference band, which is separate from the 0.90-1.10 p.u. band used for the reported voltage metrics. The three sentences are reproduced verbatim below.

Voltage-first (V):

```text
Priority order for this batch: voltage security first, EV service second, cost third. overall_score is voltage-dominant: an unmitigated severe voltage violation (severity >= 0.06) must score <= -5 (-5 or lower; -4 violates it) even when service improves; an unmitigated extreme violation (>= 0.10) must score <= -8 (-8 or lower). A moderate service shortfall (service-deficit increase <= 0.25 per step, no near-departure shortfall) is an acceptable price for keeping every bus inside 0.95-1.05 p.u.
```

Service-first (S):

```text
Priority order for this batch: EV service first (charge every vehicle to its target before it departs), voltage security second, real purchase cost third. operating_cost is money (price x upstream purchase + 3 x price x shed load). overall_score is service-dominant: departure-timed completion and SOC-gap recovery are the main positive evidence and a near-departure shortfall the main negative evidence; a severe voltage violation (severity >= 0.06) that is not mitigated should score <= -5 and never above +2.
```

Cost-first (C):

```text
Priority order for this batch: real purchase cost first, EV service second, voltage security third. operating_cost is money (price x upstream purchase + 3 x price x shed load), not a dispatch-LP penalty. overall_score is cost-dominant: paying clearly less than the scenario reference (cost_ratio_vs_reference) is the main positive evidence and paying clearly more without a service or voltage reason the main negative evidence. Cheap-by-not-serving is never good: a severe near-departure shortfall without recovery must score <= -3, and a severe voltage violation (severity >= 0.06) that is not mitigated never scores above +2.
```
