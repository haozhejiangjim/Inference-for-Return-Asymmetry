# Monthly test statistics for the real-world application

This repository contains the window-level test statistics used in the
real-world application (Section 5) of the paper *Inference for Realized
Quadratic Covariation of R and R² as a Measure of Return Asymmetry*.

## File

`Real-World_Statistics.csv`: 307 rows × 166 columns.

| Column | Description |
|---|---|
| `date` | End of the monthly window (last trading day of a block of 22 trading days, 16:00 ET) |
| `AA`, `ABT`, …, `ZION` | One column per stock (165 stocks, ticker symbols). Each entry is the bridge-corrected conditional-Gaussian statistic $t^0_n$ for that stock in that window |

There are no missing values.

## How the statistics were computed

For each stock and each non-overlapping window of 22 trading days
(78 five-minute returns per day, so n = 1,716 returns per window):

1. Five-minute log returns are computed from NYSE TAQ data. Overnight
   returns and early-closing days are excluded.
2. Jumps are detected with the Lee and Mykland (2008) test and the
   flagged returns are set to zero.
3. Returns are demeaned within the window.
4. The demeaned realized quadratic covariation is studentized by the
   bridge-corrected variance estimator, giving $t^0_n$ (see the paper
   and its Supplementary Material for the definitions).

The sample covers April 1998 to April 2025 (K = 307 windows) for the
165 S&P 500 constituents with a complete five-minute record over the
whole period.

## Reproducing the empirical result

The aggregate mean test for each stock is $\sqrt{K}\,\bar t^{\,0}_{K,n}$,
the scaled column mean. A two-sided test at the 5% level rejects when
|statistic| > 1.96.

```python
import numpy as np, pandas as pd

df = pd.read_csv("Real-World_Statistics.csv", index_col="date")
agg = np.sqrt(len(df)) * df.mean()
print(agg[agg.abs() > 1.959964].sort_values())   # 15 stocks, all negative
```

This reproduces Figure 4 of the paper.

## Data availability

The raw TAQ data are subject to a subscription license and are **not**
included. Only derived window-level test statistics are provided, from
which prices or returns cannot be reconstructed.

## License

Released under CC BY 4.0.
