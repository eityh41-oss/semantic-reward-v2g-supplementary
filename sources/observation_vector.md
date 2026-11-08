# PPO observation and action vectors

Source: `environments.py` (`state_dim = 33 + 6 + EV_type*2`, `action_dim = EV_type*2`, EV_type = 10),
`DSO_environment/dso_flow.py` (network metrics block). The observation contains **no explicit time-of-day index**.

## Policy observation $s_t$ (59 dims)

| index | dims | content | scaling |
|---|---|---|---|
| 0–32 | 33 | bus voltage magnitudes (p.u.), IEEE-33 | none |
| 33–34 | 2 | upstream active / reactive power (kW, kvar) | / $10^5$ kW, / $10^5$ kvar |
| 35 | 1 | available PV power (MW) | / 10 MW |
| 36 | 1 | PV curtailment (MW) | / 10 MW |
| 37 | 1 | active load curtailment (MW) | / 10 MW |
| 38 | 1 | reactive load curtailment (Mvar) | / 10 Mvar |
| 39–48 | 10 | current SOC of the 10 EV aggregations | [0, 1] |
| 49–58 | 10 | target SOC of the 10 EV aggregations | [0, 1] |

In the manuscript, $y_t$ denotes only the six non-voltage network entries (indices 33–38), not the full observation. Current and target SOCs are stored energy divided by the corresponding aggregate battery capacity. The two substation quantities are divided by $10^5$ kW and $10^5$ kvar; the other four power quantities are divided by 10 MW or 10 Mvar.

The hour $t$, operating period $p_t$, operator profile $z_t$, and reward-model context $o_t$ are not explicit policy inputs. The policy is $\pi_\theta(a_t\mid s_t)$; the reward model is $\hat r_\phi(s_t,a_t,o_t)$. For fixed policy parameters, identical observations produce the same action distribution. Preference changes affect the policy through reward-driven parameter updates. Operating quantities can still correlate with time under the fixed scenario schedule.

## Action $a_t$ (20 dims, each in $[-1, 1]$)

| index | dims | content |
|------|-----|-----------------------------------|
| 0–9 | 10 | active charging (+) / discharging (−) fraction per EVCS, scaled by $P^{\max}_{C/D} = 22\,\text{kW} \times N_{EV}$ |
| 10–19 | 10 | reactive power fraction per EVCS, bounded by $\sqrt{S_{\max}^2 - P^2}$ |

Base fleet: $N_{EV} = 100$ per EVCS, $B_{EV} = 50\,\text{kWh} \times N_{EV}$, $S_{\max} = 40\,\text{kVA} \times N_{EV}$;
EV-penetration experiments rescale $N_{EV}$ (see `environments.py::set_ev_penetration`).

## 69-bus system

The 69-bus observation has the same structure: 69 bus-voltage magnitudes (p.u.), the six normalized network features above, and the current and target SOCs of the 10 EVCSs, giving 69 + 6 + 10 + 10 = 95 dimensions.
