
# Economic Feature Regression – README

This script downloads historical price data for a ticker (e.g., `SPY`) and combines it with macroeconomic time series (e.g., CPI, unemployment rate, etc.). It then trains and evaluates a regression model (a pipeline of scaling, polynomial features, and Ridge regression) using `GridSearchCV`. The prediction performance is visualized and saved as an image.

---

## Requirements

### 1. Install Dependencies

```bash
pip install pandas fredapi yfinance numpy matplotlib seaborn scikit-learn
```

### 2. Provide FRED API Key

Create a file named `configs.json` in the project root:

```json
{
    "fred_api_key": "YOUR_FRED_KEY_HERE"
}
```

---

## How It Works

| Component | Description |
|------------|--------------|
| `GetData.get_ticker()` | Downloads OHLC data via `yfinance` and caches it in `/data`. |
| `GetData.get_macroeconomics(features)` | Downloads macroeconomic series from FRED and caches them in `/data`. |
| `model(xdata, ydata)` | Performs Train/Test split and tunes Ridge regression via `GridSearchCV` + `TimeSeriesSplit`. |
| `plot_eval(estimate, actual, ticker)` | Visualizes prediction vs. actual and residuals. |

---

## Run

```bash
python your_script.py
```

Default settings:

- **Ticker:** `SPY`
- **Macro features:** `['CPIAUCSL','PPIACO','PCEPI','UNRATE','UMCSENT','PAYEMS']`

Resulting plot will be saved to:

```
/plots/econs_SPY.png
```

---

## Customization

Change ticker:

```python
ticker = 'AAPL'
```

Change macro indicators:

```python
feature_df = obj.get_macroeconomics(['FEDFUNDS','M2SL','INDPRO'])
```

---

## Limitations

- Feature and ticker data must be equal in length, otherwise execution stops.
- This is **not a forecasting model** — pure in-sample regression.
- Hyperparameter search may take time on large datasets.

---

## Future Improvements

- Auto-align time indexes instead of hard-stopping on length mismatch.
- Add multi-step forecasting (e.g., with RNNs or lag features).
- Export predictions to CSV.

---
