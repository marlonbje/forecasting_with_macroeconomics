
# Exploring Whether Macroeconomic Indicators Can Predict Asset Prices

This project is **not** a production-ready model.  
It is a **simple empirical test** to answer a conceptual question:

> *Can broad macroeconomic variables (e.g. CPI, unemployment, sentiment, payrolls) be used to predict the price of a financial asset like SPY?  
And if so — how strong is that relationship?*

The script loads price history from Yahoo Finance and combines it with several macroeconomic time series from the Federal Reserve (FRED). A very basic regression model is then fitted to check **correlation / explanatory power**, not to provide tradeable forecasts.

The goal is **exploration**, not **prediction**.

---

## Methodology

1. **Data Sources**
   - Asset prices via `yfinance` (adjusted close)
   - Macroeconomic indicators from FRED:
     - CPI (`CPIAUCSL`)
     - Producer Prices (`PPIACO`)
     - Personal Consumption Expenditures (`PCEPI`)
     - Unemployment Rate (`UNRATE`)
     - Consumer Sentiment (`UMCSENT`)
     - Payroll Employment (`PAYEMS`)

2. **Objective**

   Instead of trying to forecast future prices, we ask:

   - *If macro conditions at time `t` are known, how much of the price level of an asset at time `t` can be explained by them?*
   - *Is the relationship stable or noisy?*
   - *Does it behave linearly or require higher-order transformation?*

3. **Modeling Approach**

   - Use a regression model (`Ridge Regression`) with polynomial expansion.
   - Apply `TimeSeriesSplit` cross-validation to avoid leakage.
   - Evaluate fit on a holdout period.
   - Visualize **Prediction vs Actual** and **Residual Structure**.

---

## What This Is *Not*

- ❌ It is *not* a trading strategy  
- ❌ It is *not* a causal model  
- ❌ It does *not* claim macro → price directionality  
- ❌ It is *not* optimized for performance or scalability

This is purely a **statistical correlation test** packaged in code.

---

## How to Run

```bash
python your_script.py
```

Include a `configs.json` file with your FRED key:

```json
{ "fred_api_key": "YOUR_FRED_KEY_HERE" }
```

---

## Interpretation of Results

If the model produces **high R² scores and structured residuals**, it suggests macro variables contain meaningful pricing information.

If the residuals are **random and R² is low**, the conclusion is straightforward:

> *Macro data alone is insufficient for price-level estimation at this granularity.*

Either outcome is valuable.

---

## Future Extensions

- Lagged macro variables to test delayed relationships
- Asset returns instead of price levels
- Rolling window regression to test **stability through regimes**
- Statistical comparison across asset classes (equities vs commodities vs FX)
