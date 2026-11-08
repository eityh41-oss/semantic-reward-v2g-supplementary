# Table S1: daily network-loss energy

**Table S1.** Daily network-loss energy (MWh/day) over the 24 held-out days for the policies frozen after 10,000 episodes; means and standard deviations across three seeds. The policies charge almost the same daily EV energy (32.9 to 33.2 MWh on the 33-bus and 33.8 to 34.1 MWh on the 69-bus system), so a daily loss ratio normalized by native load plus EV charging ranks the policies in the same order as the loss energy. For these three methods, the seed means of this table are the network-loss row of Table III of the manuscript.

| System, preference | Score-only Reward | Semantic Reward | Expert Reference |
|---|---|---|---|
| 33-bus, voltage first | 10.44 ± 2.31 | 6.64 ± 0.29 | 9.75 ± 0.70 |
| 33-bus, service first | 9.22 ± 4.23 | 6.00 ± 0.26 | 5.55 ± 1.08 |
| 33-bus, cost first | 9.22 ± 2.55 | 4.26 ± 0.01 | 4.32 ± 0.00 |
| 69-bus, voltage first | 17.32 ± 7.41 | 10.08 ± 0.38 | 20.44 ± 0.86 |
| 69-bus, service first | 18.02 ± 5.50 | 7.75 ± 0.63 | 10.36 ± 1.58 |
| 69-bus, cost first | 13.57 ± 2.64 | 4.94 ± 0.18 | 5.13 ± 0.01 |
