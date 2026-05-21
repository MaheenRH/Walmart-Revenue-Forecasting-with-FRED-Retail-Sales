# Prompt Log — YipitData AI Engineer Take-Home

## Prompts Sent to Claude (claude-sonnet-4-6)

---

### Prompt 1 — Project setup and analysis plan

> I'm doing a data science take-home. The question: does the FRED monthly retail-sales index (RSXFS) predict Walmart's quarterly revenue better than a naive baseline? Build a Python analysis: walk-forward OOS evaluation, two naive baselines, no shuffled k-fold for time series. Then a one-page memo for a portfolio manager and a prompt log.

**Claude generated:** Data generation code (synthetic), project structure, and the full walk-forward loop with seasonal-naive and drift baselines plus two OLS models.

---

### Prompt 2 — Fix fiscal calendar alignment bug

> The merge is returning 0 rows. Walmart's quarters end January/April/July/October, not the standard March/June/September/December. Fix the resampling.

**Claude identified and fixed:** Using `resample('QE-JAN')` instead of `resample('QE')` to match Walmart's fiscal calendar. Also switched from sum to mean of monthly values to handle unequal quarter lengths (Walmart's January quarter has only 1 FRED observation at the series start).

---

### Prompt 3 — Granger causality without statsmodels

> statsmodels isn't available. Implement Granger causality as a manual F-test using sklearn and scipy.

**Claude generated:** Correct manual F-test: restricted model (wmt_yoy ~ wmt_lag1) vs. unrestricted (wmt_yoy ~ wmt_lag1 + retail_lag1). Result on real data: F=0.53, p=0.47 — **not significant**. This was the key finding that changed the memo's conclusion.

---

### Prompt 4 — Subperiod analysis and scatter plot

> Add COVID vs. non-COVID MAPE split and a scatter of retail YoY vs. Walmart YoY with COVID quarters highlighted.

**Claude generated:** Subperiod loop and Figure 5 (scatter with regression line and R² annotation).

---

### Prompt 5 — Memo drafting with real numbers

> Write the one-page memo. The drift baseline (2.61% MAPE) beats the FRED signal (3.40%). The Granger test is not significant. Be honest — don't spin the result. End with what evidence would change our minds.

**Claude generated:** First draft. The "what to worry about" section initially focused only on COVID, missing the fiscal calendar misalignment issue. I pushed back.

---

### Prompt 6 — Add the fiscal calendar misalignment as a key caveat

> The fiscal calendar misalignment is important — most practitioners would miss it. Add it explicitly to the memo's risks section.

**Claude added:** Caveat #3 on Walmart's fiscal quarter dates and the `QE-JAN` fix.

---

## Reflection on LLM Use

**What Claude got right.** The walk-forward OOS structure was solid on the first pass — expanding windows, no future leakage, MAPE as the primary metric. Claude also correctly framed the lagged OLS as the "true" leading indicator test and distinguished it from the contemporaneous model. The cross-correlation analysis across multiple lags was well-motivated and revealed the symmetric correlation pattern that undermined the leading-indicator story.

**Where I pushed back — and caught a real error.** The biggest issue was the date alignment bug: Claude's initial code used `resample('QE')`, which resamples to standard calendar quarters (ending March/June/September/December), while Walmart's fiscal quarters end in January/April/July/October. The merge returned zero rows. I diagnosed this by printing the unique quarter-end months in both series and asked Claude to fix the offset. This is exactly the kind of domain-specific detail that LLMs miss — the code was syntactically correct and ran without errors, but was silently wrong. The fix (`QE-JAN`) is a one-liner, but finding it required understanding Walmart's fiscal calendar.

The second push-back was on the memo. Claude's first draft framed the result optimistically ("the signal beats the seasonal-naive baseline"). I asked it to lead with the honest conclusion: the drift baseline wins, and the Granger test is not significant. The revised memo is more useful to a portfolio manager.

**How I checked the output.** Ran the full script and verified: (1) OOS evaluation only used held-out quarters, (2) the COVID subperiod dates matched Walmart's fiscal calendar, (3) the Granger F-statistic formula matched the textbook definition, (4) spot-checked two specific OOS predictions manually against raw data.
