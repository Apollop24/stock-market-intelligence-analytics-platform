# Stock Market Intelligence and Portfolio Analytics Platform

**End-to-End Business Analytics | CRISP-DM Methodology | Python**

Philip Kibet — Statistician and Data Analyst

---

## Overview

This platform delivers a complete, production-grade financial analytics pipeline built on three real-world public datasets. It covers every stage of the CRISP-DM methodology: from raw data acquisition and quality auditing through exploratory analysis, statistical modelling, machine learning forecasting, and a fully interactive executive dashboard rendered as a self-contained HTML file.

The platform requires no API keys. All data is sourced from publicly accessible raw GitHub URLs and fetched at runtime.

---

## Repository Structure

```
stock-market-intelligence-analytics-platform/
|
|-- README.md
|
|-- code/
|   |-- stock_intelligence_platform.py        Main executable script (7 analytical phases)
|   |-- stock_intelligence_platform.ipynb     Jupyter Notebook version (cell-by-cell walkthrough)
|
|-- dashboard/
|   |-- stock_intelligence_dashboard.html     Interactive Plotly executive dashboard (self-contained)
|
|-- data/
|   |-- DATA_SOURCES.md                       Data citation, lineage, and access instructions
|
|-- docs/
    |-- methodology.md                        Statistical methodology and analytical decisions
    |-- results_summary.md                    Key findings and executive summary
```

---

## Data Sources

All datasets are publicly available and fetched directly from raw GitHub URLs. No downloads or API keys are required.

| Dataset | Source Repository | Coverage | Records |
|---|---|---|---|
| S&P 500 Fundamentals | github.com/datasets/s-and-p-500-companies-financials | 503 companies, 14 financial fields | 503 rows |
| Apple OHLCV | github.com/plotly/datasets (finance-charts-apple.csv) | 2015-02-17 to 2017-02-16 | 506 trading days |
| Multi-Stock Close Prices | github.com/plotly/datasets (stockdata.csv) | AAPL, MSFT, IBM, SBUX, S&P500 | 2,306 trading days (2007-2016) |

Full citation details are in `data/DATA_SOURCES.md`.

---

## Analytical Pipeline

The platform executes seven sequential phases in a single run.

### Phase 1 — Data Acquisition and Feature Engineering

Downloads all three datasets and engineers 28 features per trading day for the Apple OHLCV dataset, including price momentum, trend indicators, volatility measures, momentum oscillators, and calendar features.

Features engineered: daily return, log return, cumulative return, MA20/MA50/MA200, 20-day annualised volatility, RSI(14), MACD(12,26,9) with signal line and histogram, drawdown from rolling peak, high-low spread, open-close change, volume MA20, and calendar decomposition (month, quarter, day-of-week, year).

### Phase 2 — Data Quality Assessment

Follows Tabachnick and Fidell (2019) standards throughout.

- Missing value analysis with percentage flags and status classification
- Normality testing using Shapiro-Wilk W statistic with skewness and kurtosis
- Outlier detection using the Z-score method with the |Z| > 3.29 threshold
- Stationarity testing using the Augmented Dickey-Fuller test on price levels and returns
- All results presented in APA-formatted console tables

### Phase 3 — Statistical Analysis

- Pearson and Spearman correlation matrices across five tickers
- OLS regression of Apple daily return on volume, volatility, and MACD features
- Variance Inflation Factor (VIF) for multicollinearity assessment
- Breusch-Pagan test for heteroskedasticity
- Durbin-Watson statistic for autocorrelation
- Rolling 30-day beta of each stock against the S&P 500

### Phase 4 — Business Analytics

- Sector treemap of S&P 500 market capitalisation by GICS sector
- Valuation scatter plot: P/E ratio against P/B ratio, sized by market capitalisation
- Graham score classification: Deep Value, Value-Leaning, Neutral, Speculative
- 52-week price momentum analysis across sectors
- Seasonality analysis of Apple returns by month and day-of-week

### Phase 5 — Predictive Analytics

- Linear regression and Ridge regression with time-series cross-validation (TimeSeriesSplit, 5 folds)
- Feature importance ranking from standardised coefficients
- Rolling 30-day forecast with confidence bands
- Model evaluation: R-squared, MAE, RMSE across folds
- MACD and RSI technical indicator charting

### Phase 6 — Executive Dashboard

Produces a fully self-contained interactive HTML dashboard with Bloomberg dark-terminal styling. All charts are Plotly-powered and interactive (hover, zoom, pan, range selection). The dashboard includes:

- Candlestick chart with Bollinger Bands
- Volume profile with 20-day moving average
- Normalised multi-stock performance comparison (base 100)
- Correlation heatmap
- Sharpe ratio, Sortino ratio, Beta, VaR, CVaR, and maximum drawdown summary
- Sector treemap and valuation bubble chart
- OLS regression diagnostics
- Ridge regression forecast with confidence interval
- Executive recommendation table

### Phase 7 — Console Summary

Prints a structured analytical summary to the console covering data quality findings, statistical test outcomes, risk metrics, and model performance.

---

## Key Results

| Metric | Value |
|---|---|
| Apple OHLCV trading days analysed | 506 |
| Features engineered | 28 |
| S&P 500 companies in fundamental analysis | 503 |
| Multi-stock period | 2007-2016 (2,306 days) |
| OLS model R-squared (AAPL daily return) | Reported in dashboard |
| Ridge regression cross-validated R-squared | Reported in dashboard |
| Apple annualised volatility (2015-2017) | Reported in dashboard |
| Deep Value stocks (Graham score = 100) | Reported in dashboard |

---

## Methodology

### Statistical Framework

Tabachnick, B. G., and Fidell, L. S. (2019). *Using Multivariate Statistics* (7th ed.). Pearson.

This framework governs all data quality decisions, outlier thresholds, normality assessments, and regression diagnostics throughout the platform.

### CRISP-DM Process Model

Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C., and Wirth, R. (2000). *CRISP-DM 1.0: Step-by-step data mining guide*. SPSS Inc.

The six CRISP-DM phases (Business Understanding, Data Understanding, Data Preparation, Modelling, Evaluation, Deployment) map directly to the seven analytical phases of this platform.

### Risk Metrics

- Sharpe Ratio: annualised excess return divided by annualised standard deviation, assuming a 2% risk-free rate
- Sortino Ratio: annualised excess return divided by downside deviation (negative returns only)
- Beta: rolling 30-day covariance of stock return with S&P 500 return, divided by S&P 500 variance
- Value at Risk (VaR): 95th percentile of the daily return distribution (historical simulation)
- Conditional VaR (CVaR): mean of returns below the VaR threshold
- Maximum Drawdown: largest peak-to-trough decline in cumulative returns

---

## Requirements

```
python >= 3.9
numpy
pandas
plotly >= 5.14
scipy
scikit-learn
statsmodels
```

Install all dependencies in one command:

```bash
pip install numpy pandas plotly>=5.14 scipy scikit-learn statsmodels
```

---

## How to Run

### Option 1 — Python script (recommended for production)

```bash
git clone https://github.com/Apollop24/stock-market-intelligence-analytics-platform.git
cd stock-market-intelligence-analytics-platform
pip install numpy pandas plotly scipy scikit-learn statsmodels
python code/stock_intelligence_platform.py
```

The script downloads all data at runtime, runs all seven phases, prints analytical tables to the console, and writes the interactive dashboard to `dashboard/stock_intelligence_dashboard.html`.

### Option 2 — Jupyter Notebook (recommended for exploration)

```bash
jupyter notebook code/stock_intelligence_platform.ipynb
```

Run cells sequentially. Each cell corresponds to one analytical phase with explanatory markdown above the code.

### Option 3 — View the dashboard directly

Open `dashboard/stock_intelligence_dashboard.html` in any modern browser. No Python required. The file is fully self-contained.

---

## Data Citation

### S&P 500 Fundamentals

Rufus Pollock and contributors. (2020). *S&P 500 Companies with Financial Information* [Dataset]. Open Knowledge Foundation, datasets project. Retrieved from https://github.com/datasets/s-and-p-500-companies-financials. Licensed under the Open Data Commons Open Database License (ODbL).

### Apple OHLCV and Multi-Stock Close Prices

Plotly Technologies Inc. (2023). *Plotly datasets* [Dataset repository]. GitHub. Retrieved from https://github.com/plotly/datasets. Files: `finance-charts-apple.csv` and `stockdata.csv`. Original price data sourced from Yahoo Finance.

---

## Author

Philip Kibet
Statistician and Data Analyst
Nairobi, Kenya

Available for freelance statistical programming, financial analytics, and data science projects.

---

## Disclaimer

This repository is developed for analytical and portfolio demonstration purposes. Nothing in this platform constitutes financial advice. All analyses are retrospective and based on historical data. Past market behaviour does not guarantee future results.
