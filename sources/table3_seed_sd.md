# Table III with standard deviations across seeds

Policies frozen after 10,000 episodes, evaluated deterministically on 24 held-out days; each entry is the mean ± sample standard deviation (denominator n − 1, n = 3) across the three seeds (503, 509, 521); PSA denotes the price–service adaptive reward. Columns 33-V to 69-C denote the 33-bus and 69-bus systems under the voltage-first (V), service-first (S), and cost-first (C) preferences.

| Metric | Method | 33-V | 33-S | 33-C | 69-V | 69-S | 69-C |
|---|---|---|---|---|---|---|---|
| Return per day | Semantic Reward | -1.42 ± 0.03 | -0.64 ± 0.04 | 0.49 ± 0.01 | -1.24 ± 0.13 | -0.76 ± 0.02 | 0.06 ± 0.04 |
|  | Expert Reference | -1.96 ± 0.02 | -0.83 ± 0.11 | 0.48 ± 0.00 | -1.60 ± 0.07 | -0.98 ± 0.00 | 0.01 ± 0.00 |
|  | Score-only Reward | -2.81 ± 0.12 | -1.05 ± 0.18 | -2.40 ± 1.45 | -4.46 ± 2.93 | -1.27 ± 0.21 | -3.98 ± 1.04 |
|  | PSA | -3.27 ± 0.04 | -0.96 ± 0.00 | 0.48 ± 0.00 | -3.54 ± 0.07 | -1.06 ± 0.03 | 0.01 ± 0.00 |
| Purchase cost (kCNY/day) | Semantic Reward | 46.2 ± 0.1 | 45.8 ± 0.2 | 44.8 ± 0.0 | 49.7 ± 0.2 | 48.4 ± 0.3 | 47.0 ± 0.1 |
|  | Expert Reference | 47.7 ± 0.3 | 45.5 ± 0.6 | 44.8 ± 0.0 | 55.4 ± 0.5 | 49.7 ± 0.8 | 47.1 ± 0.0 |
|  | Score-only Reward | 48.5 ± 1.5 | 48.0 ± 2.6 | 47.4 ± 1.1 | 53.8 ± 4.3 | 54.3 ± 3.4 | 51.7 ± 1.6 |
|  | PSA | 44.8 ± 0.0 | 44.8 ± 0.0 | 44.8 ± 0.0 | 47.1 ± 0.0 | 47.1 ± 0.0 | 47.1 ± 0.0 |
| Waiting deficit (SOC-h/day) | Semantic Reward | 2.78 ± 0.04 | 2.63 ± 0.03 | 2.70 ± 0.12 | 2.95 ± 0.01 | 2.80 ± 0.08 | 2.87 ± 0.05 |
|  | Expert Reference | 2.71 ± 0.12 | 2.18 ± 0.03 | 2.27 ± 0.05 | 2.75 ± 0.04 | 2.19 ± 0.01 | 2.41 ± 0.01 |
|  | Score-only Reward | 2.63 ± 0.23 | 2.39 ± 0.15 | 4.08 ± 2.06 | 2.47 ± 0.08 | 2.86 ± 0.27 | 2.91 ± 0.17 |
|  | PSA | 2.36 ± 0.11 | 2.34 ± 0.11 | 2.25 ± 0.01 | 2.47 ± 0.03 | 2.48 ± 0.04 | 2.42 ± 0.01 |
| Voltage violation rate | Semantic Reward | 0.008 ± 0.004 | 0.019 ± 0.011 | 0.057 ± 0.009 | 0.007 ± 0.003 | 0.010 ± 0.001 | 0.096 ± 0.001 |
|  | Expert Reference | 0.033 ± 0.005 | 0.049 ± 0.023 | 0.076 ± 0.001 | 0.010 ± 0.004 | 0.070 ± 0.008 | 0.111 ± 0.003 |
|  | Score-only Reward | 0.031 ± 0.003 | 0.043 ± 0.014 | 0.413 ± 0.290 | 0.119 ± 0.100 | 0.072 ± 0.029 | 0.414 ± 0.010 |
|  | PSA | 0.071 ± 0.005 | 0.069 ± 0.005 | 0.075 ± 0.002 | 0.107 ± 0.003 | 0.103 ± 0.008 | 0.112 ± 0.002 |
| Network loss (MWh/day) | Semantic Reward | 6.6 ± 0.3 | 6.0 ± 0.3 | 4.3 ± 0.0 | 10.1 ± 0.4 | 7.7 ± 0.6 | 4.9 ± 0.2 |
|  | Expert Reference | 9.7 ± 0.7 | 5.6 ± 1.1 | 4.3 ± 0.0 | 20.4 ± 0.9 | 10.4 ± 1.6 | 5.1 ± 0.0 |
|  | Score-only Reward | 10.4 ± 2.3 | 9.2 ± 4.2 | 9.2 ± 2.5 | 17.3 ± 7.4 | 18.0 ± 5.5 | 13.6 ± 2.6 |
|  | PSA | 4.3 ± 0.0 | 4.3 ± 0.0 | 4.3 ± 0.0 | 5.1 ± 0.0 | 5.1 ± 0.1 | 5.1 ± 0.0 |
| Service limits met (share of days) | Semantic Reward | 0.94 ± 0.02 | 0.97 ± 0.02 | 0.90 ± 0.10 | 1.00 ± 0.00 | 1.00 ± 0.00 | 1.00 ± 0.00 |
|  | Expert Reference | 0.96 ± 0.04 | 1.00 ± 0.00 | 1.00 ± 0.00 | 1.00 ± 0.00 | 1.00 ± 0.00 | 1.00 ± 0.00 |
|  | Score-only Reward | 0.90 ± 0.13 | 1.00 ± 0.00 | 0.67 ± 0.58 | 0.94 ± 0.10 | 0.81 ± 0.17 | 0.96 ± 0.04 |
|  | PSA | 1.00 ± 0.00 | 1.00 ± 0.00 | 1.00 ± 0.00 | 1.00 ± 0.00 | 1.00 ± 0.00 | 1.00 ± 0.00 |
