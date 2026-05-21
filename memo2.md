# Does FRED Retail Sales Predict Walmart Revenue Better Than a Naive Baseline?

**Bottom line: The FRED retail signal beats the seasonal-naive baseline by 1.41 percentage points out-of-sample — but the drift baseline wins outright. The signal is a modest improvement over the simplest possible guess, not a strong leading indicator.**

---

## What We Tested

We used the FRED monthly retail-sales index (RSXFS), summed to Walmart's fiscal quarters (ending January/April/July/October), to forecast Walmart's year-over-year quarterly revenue growth. We compared four approaches in a **walk-forward out-of-sample test** — 24 quarters of predictions from 2020Q1 through 2025Q4, trained on an expanding window of prior data. No future data was ever used to make a prediction.

**Baselines:**
- *Seasonal naive:* Next quarter's revenue = same quarter one year ago — MAPE: **4.92%** / MAE: **$7.7B**
- *Drift:* Seasonal naive × (1 + historical mean YoY growth rate) — MAPE: **2.70%** / MAE: **$4.2B**

**Signal models:**
- *OLS, contemporaneous:* This quarter's retail YoY predicts this quarter's Walmart YoY — MAPE: **3.53%** / MAE: **$5.4B**
- *OLS, lagged (true leading indicator):* Last quarter's retail YoY predicts this quarter's Walmart YoY — MAPE: **3.52%** / MAE: **$5.4B**

---

## The Answer

**Yes, the signal beats the seasonal-naive baseline — by 1.41 percentage points** (4.92% → 3.51% MAPE). At Walmart's ~$150B average quarterly revenue over this period, each percentage point of MAPE is roughly $1.5B in forecast error. The improvement is real and meaningful in dollar terms.

**However, the drift baseline wins at 2.70% MAPE** — 0.81 points better than the lagged retail model. Knowing Walmart's own historical growth rate is still more useful than the FRED signal. Granger causality confirms why: retail sales YoY does **not** statistically Granger-cause Walmart revenue YoY at either lag 1 or lag 2 (F=1.72, p=0.195 at lag 1). The retail series adds no incremental predictive power beyond Walmart's own lagged history.

The near-zero contemporaneous and lagged correlations (rolling mean: 0.30, dropping to -0.70 at its worst during COVID) tell the same story: the relationship is weak, unstable, and regime-dependent.

---

## What to Worry About

**1. The signal inverts in disruptions.** The rolling 8-quarter correlation between retail YoY and Walmart revenue YoY hit **-0.70** during COVID. This is the worst-case scenario for a leading indicator: it doesn't just weaken in the periods that matter most, it actively points the wrong direction. A portfolio manager who relied on it in 2020 would have been misled.

**2. Walmart outperforms its sector in downturns.** RSXFS covers all U.S. retail — apparel, electronics, luxury, discretionary. Walmart's revenue is ~60% grocery and consumables. When consumers trade down during recessions, Walmart gains share while the broader index falls. This structural divergence means the signal is most likely to break exactly when economic conditions are most interesting to track.

**3. The drift baseline is hard to beat.** Walmart's revenue growth is remarkably persistent — once you know its trend, the FRED signal adds noise rather than information. Any signal claiming to predict Walmart revenue should first clear the drift baseline, not just the seasonal-naive floor.

**4. Reporting lags and real-time data.** RSXFS is revised after initial release. This analysis uses final revised figures, not what was available at forecast time. True real-time testing via ALFRED vintage data might show worse performance.

**5. Fiscal calendar alignment.** Walmart's quarters end January/April/July/October — not standard calendar quarters. Any analysis that ignores this misaligns every data point, inflating apparent predictive power. This notebook accounts for it by shifting Walmart dates to align with FRED quarters before merging.

---

## What Would Change Our Minds

- **Grocery/food subindex:** Test the FRED food services and grocery subindex instead of headline retail. If it Granger-causes Walmart revenue where the headline index does not, the signal earns a second look — and the causal story (grocery volumes drive Walmart revenue) becomes credible.

- **Exclude disruption periods:** Run the OOS test on 2014–2019 only. If the lagged model beats drift in the pre-COVID stable period, the relationship exists but breaks under stress — a defensible caveat rather than a disqualifier.

- **Walmart-specific proxies:** SNAP redemption volumes, food-at-home CPI, or dollar-store same-store sales track Walmart's actual customer base far more tightly. Any of these would be a stronger candidate for a genuine leading indicator.

---

*Analysis: FRED RSXFS (Jan 2010 – Mar 2026) and Walmart quarterly revenue (FY2010–FY2026). Walk-forward OOS: 2020Q1–2025Q4 (24 quarters, expanding window). Full methodology in `analysis2.ipynb`.*
