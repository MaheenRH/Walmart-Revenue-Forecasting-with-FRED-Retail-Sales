# Walmart-Revenue-Forecasting-with-FRED-Retail-Sales
This project evaluates whether U.S. retail sales growth (FRED RSXFS) can serve as a leading indicator for Walmart quarterly revenue growth.

Using walk-forward out-of-sample forecasting, naive baselines, OLS regression models, rolling correlation analysis, and Granger causality testing, the project examines whether retail-sales information improves forecasting accuracy beyond Walmart’s own historical trend and seasonality.
 ## Key Findings

- Retail-sales models modestly outperformed a simple seasonal-naive benchmark.
- A drift baseline using Walmart’s own historical growth trend achieved the best forecasting accuracy overall.
- Granger causality tests found no statistically significant incremental predictive power from retail sales beyond Walmart’s own historical behavior.
- Forecast stability weakened materially during the COVID/post-COVID period.
## Out-of-Sample Forecast Performance

| Model | MAPE |
|---|---|
| Seasonal Naive | 4.92% |
| Drift Baseline | 2.70% |
| OLS: Contemporaneous Retail | 3.53% |
| OLS: Lagged Retail | 3.51% |

## Methodology

- Monthly FRED retail sales aggregated into Walmart fiscal quarters
- Year-over-year growth transformation
- Expanding-window walk-forward forecasting
- Seasonal-naive and drift baselines
- OLS regression forecasting
- Rolling correlation stability analysis
- Granger causality testing

## Repository Structure

- `analysis.ipynb` — full notebook analysis
- `analysis.py` — script version
- `memo.md` — final written conclusions
- `output/` — generated figures
