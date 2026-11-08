# Reward-model results on the common day split

Both reward models are fitted on the same days, early-stopped on the same 20 days, and evaluated on the same 20 test days; three seeds per system and preference (18 configurations). Step-level Pearson correlation, mean ± SD over the 18 configurations; the last column counts configurations in which the semantic model is higher.

| Fitting days (steps) | Subset | Score-only | Semantic | Semantic higher |
|---|---|---|---|---|
| 10 (240) | fitting | 0.910 ± 0.135 | 0.994 ± 0.012 | 14/18 |
| 10 (240) | early-stopping | 0.621 ± 0.099 | 0.855 ± 0.038 | 18/18 |
| 10 (240) | test | 0.628 ± 0.075 | 0.856 ± 0.027 | 18/18 |
| 20 (480) | fitting | 0.971 ± 0.025 | 0.992 ± 0.009 | 16/18 |
| 20 (480) | early-stopping | 0.722 ± 0.057 | 0.905 ± 0.015 | 18/18 |
| 20 (480) | test | 0.710 ± 0.056 | 0.895 ± 0.017 | 18/18 |
| 60 (1440) | fitting | 0.937 ± 0.043 | 0.989 ± 0.010 | 17/18 |
| 60 (1440) | early-stopping | 0.827 ± 0.048 | 0.936 ± 0.010 | 18/18 |
| 60 (1440) | test | 0.809 ± 0.049 | 0.929 ± 0.012 | 18/18 |

Clear-pair day-ranking accuracy on the 20 test days (fraction of test-day pairs with clearly different labeled scores ordered correctly), mean over the 18 configurations:

| Fitting days | Score-only | Semantic |
|---|---|---|
| 10 | 0.661 | 0.813 |
| 20 | 0.679 | 0.831 |
| 60 | 0.788 | 0.878 |
