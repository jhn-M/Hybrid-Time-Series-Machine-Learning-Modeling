## Results and Evaluation

### Test Set Performance

The hybrid model was evaluated against the standalone ARIMA baseline 
on an 18-month test set (July 2024 – December 2025).

| Metric | Standalone ARIMA | Hybrid ARIMA-RF | Improvement |
|--------|-----------------|-----------------|-------------|
| MSE    | 3.1305          | 2.9073          | 7.13%       |
| RMSE   | 1.7693          | 1.7051          | 3.63%       |
| MAE    | 1.6458          | 1.5856          | 3.66%       |

The hybrid model consistently outperformed the standalone ARIMA across 
all metrics, confirming that the Random Forest residual correction layer 
captures non-linear patterns that ARIMA alone cannot model.

---

## 36-Month Forecast (2026–2028)

The model was refitted on the full dataset (January 2021 – December 2025) 
and used to generate a 36-month forward forecast with a 95% growing 
confidence interval.
<img width="4169" height="1468" alt="forecast_plot" src="https://github.com/user-attachments/assets/23fe9527-e6c3-4fc0-97c2-ff3d1320f8ae" />


### What the Forecast Shows

The ARIMA component forecasts inflation converging toward ~2.0% — 
consistent with the BSP's current inflation trajectory following the 
sharp decline from the 2023 peak of 8.7%. The Random Forest correction 
layer adds dynamic oscillations around this baseline, producing a hybrid 
forecast ranging from 1.78% to 2.55% over the forecast horizon.

---

## Model Limitations

### Wall 1 — ARIMA Mean Reversion

ARIMA(0,1,1) is inherently a short-memory model. Beyond the first 
forecast step it collapses to a flat line at the long-run mean (~2.0%), 
losing all dynamic information. It has no mechanism to anticipate 
future shocks — it can only extrapolate from where it has been.

### Wall 2 — RF Corrections Are Marginal Without Exogenous Variables

The Random Forest was trained solely on lagged ARIMA residuals. 
Diagnostic analysis of the residual correction output revealed:

- Mean correction: 0.019
- Std of corrections: 0.166  
- Range: -0.22 to +0.55

These corrections are near-negligible over a 36-month horizon because 
the ARIMA residuals approximate white noise — there is minimal 
exploitable pattern remaining after ARIMA captures the linear structure. 
Without external economic signals, the RF has no meaningful forward-looking 
information to act on.

### Wall 3 — Recursive Prediction Degrades Over Time

The 36-month forecast uses recursive multi-step prediction — each 
step feeds the previous prediction as input for the next. Small errors 
compound with each iteration. By month 12 the RF signal has largely 
dissipated, and by month 36 corrections converge toward zero, leaving 
the hybrid forecast indistinguishable from the flat ARIMA baseline.

---

## Why This Model Is a Baseline, Not a Final Answer

The limitations above are not failures — they are the honest boundaries 
of a model that relies purely on historical inflation data. Philippine 
inflation is driven by external shocks (energy prices, currency movements, 
global supply chains) that no univariate time series model can anticipate.

This project establishes a rigorous statistical baseline. The next phase 
— documented in 
[macro-nexus-forecasting](https://github.com/jhn-M/macro-nexus-forecasting) 
— directly addresses these walls by integrating:

- **Brent Crude Oil prices** — contemporaneous inflation driver
- **USD-PHP Exchange Rate** — 4-month leading indicator (r = 0.44 at lag 4)
- **NLP Sentiment Scores** — early warning signal from BSP policy statements 
  and financial news
- **RF Classification Layer** — directional validation of the regression forecast

The expectation is that exogenous variables will provide the RF correction 
layer with genuine forward-looking signal, meaningfully increasing correction 
magnitude and forecast stability beyond 12 months.
