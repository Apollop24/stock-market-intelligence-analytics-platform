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
|   |-- stock_intelligence_platform.py         Main executable script (7 analytical phases)
|   |-- stock_intelligence_platform.ipynb      Jupyter Notebook version (cell-by-cell)
|
|-- dashboard/
|   |-- stock_intelligence_dashboard.html      Interactive Plotly executive dashboard
|
|-- data/
|   |-- DATA_SOURCES.md                        Data citations, lineage, and access instructions
|
|-- docs/
    |-- methodology.md                         Statistical methodology and analytical decisions
    |-- results_summary.md                     Key findings and executive summary
```

---

## Data Sources

All datasets are publicly available and fetched directly from raw GitHub URLs at runtime.

| Dataset | Source | Coverage | Records |
|---|---|---|---|
| S&P 500 Fundamentals | github.com/datasets/s-and-p-500-companies-financials | 503 companies, 14 fields | 503 rows |
| Apple OHLCV | github.com/plotly/datasets | 2015-02-17 to 2017-02-16 | 506 trading days |
| Multi-Stock Close Prices | github.com/plotly/datasets | AAPL, MSFT, IBM, SBUX, S&P 500 | 2,306 trading days (2007-2016) |

Full citations and data lineage are in `data/DATA_SOURCES.md`.

---

## Analytical Pipeline

The platform executes seven sequential phases in a single run.

| Phase | Description |
|---|---|
| 1 — Data Acquisition | Download all datasets, engineer 28 features per trading day |
| 2 — Data Quality | Missing values, normality (Shapiro-Wilk), outliers (Z-score), stationarity (ADF) |
| 3 — Statistical Analysis | OLS regression, VIF, Breusch-Pagan, Durbin-Watson, correlation matrices |
| 4 — Business Analytics | Sector treemap, valuation scatter, Graham score, seasonality |
| 5 — Predictive Analytics | Linear regression, Ridge regression, TimeSeriesSplit CV, rolling forecast |
| 6 — Executive Dashboard | Interactive Plotly HTML with Bloomberg dark-terminal styling |
| 7 — Console Summary | Structured print of all findings, tables, and model metrics |

---

## Executive KPI Dashboard

The dashboard opens with nine live KPI cards computed directly from the data.

| KPI | Value | Description |
|---|---|---|
| AAPL Total Return | 5.9% | 2015-02-17 to 2017-02-16 |
| Sharpe Ratio | 0.239 | Risk-adjusted performance |
| Max Drawdown | -32.08% | Peak-to-trough loss |
| Beta vs S&P 500 | 0.961 | Systematic risk |
| Annualised Volatility | 24.28% | Annual standard deviation |
| VaR (95%, 1-day) | -2.49% | Maximum daily loss (95% CI) |
| Win Rate | 49.9% | Positive return days / total days |
| LR Model R² | 0.965 | Next-day price prediction accuracy |

---

## Dashboard Charts

The interactive dashboard contains fifteen sections rendered with Plotly in a Bloomberg dark-terminal theme. Each chart is fully interactive with hover tooltips, zoom, pan, and range selection.

### 1. Price Intelligence — AAPL (2015-2017)
Candlestick chart with Bollinger Bands (20-day, 2 standard deviations), MA20, MA50, and MA200 overlays. Volume profile with 20-day moving average shown below. Captures the full price cycle from USD 90 to USD 133, including the mid-2016 drawdown and the Q4 2016 recovery.

### 2. Multi-Stock Normalised Performance (2007-2016)
Base-100 normalised close price series for all five tickers from 2007-01-03. Ten-year cumulative returns shown directly in the chart. Apple diverged from the group from 2009 onward, reaching 9x the starting value by 2016.

### 3. Return Correlation Matrix
Pearson correlation heatmap across AAPL, MSFT, IBM, SBUX, and the S&P 500 daily returns. Annotated with coefficient values. Strong positive correlations between large-cap technology names visible during the 2007-2016 period.

### 4. Return Distribution Analysis
Histogram of AAPL daily returns with kernel density overlay. Fat tails visible in both the positive and negative return extremes, confirming the non-normality of financial returns and validating the Shapiro-Wilk test findings.

### 5. Drawdown Analysis
Continuous drawdown curve from peak equity across the full 2015-2017 period. Maximum drawdown of -32.08% reached during the 2016 correction. Recovery timeline annotated on the chart.

### 6. Sector Treemap — S&P 500 Market Capitalisation
Proportional area treemap of all 11 GICS sectors by aggregate market capitalisation. Information Technology is the dominant sector. Each tile sized by total sector market cap in USD billions, colour-coded by sector.

### 7. Valuation Landscape — PE vs PB
Scatter plot of all 503 S&P 500 companies with P/E ratio on the x-axis, P/B ratio on the y-axis, marker size proportional to market capitalisation, and colour coded by sector. Identifies undervalued and overvalued clusters across sectors.

### 8. Sector PE vs Dividend Yield
Sector-level scatter comparing median P/E ratio against median dividend yield. Sectors with low P/E and high yield (Utilities, Financials, Consumer Staples) sit in the value quadrant. Technology and Consumer Discretionary sit in the growth quadrant.

### 9. Monthly Seasonality
Average AAPL daily return by calendar month with standard deviation bands. Reveals historical seasonal patterns in return distribution. Positive months and negative months visible over the 2015-2017 sample.

### 10. Rolling 30-Day Beta
Time-series of AAPL 30-day rolling beta against the S&P 500. Beta oscillated between 0.7 and 1.4 across the analysis period, with elevated systematic risk during earnings announcements and market-wide events.

### 11. MACD Momentum Indicator
MACD line (12-26 EMA difference), signal line (9-period EMA of MACD), and histogram bars. Bullish crossovers and bearish crossovers annotated. RSI(14) shown in a lower panel with overbought (70) and oversold (30) reference lines.

### 12. Predictive Model — Actual vs Forecast
Out-of-sample test set comparison of actual AAPL closing price against Linear Regression and Ridge Regression forecasts with confidence bands. 80/20 train-test split with temporal ordering preserved.

---

## Results Tables

### Table 1. AAPL Risk Metrics (2015-2017)

| Metric | Value |
|---|---|
| Annualised Return | 5.80% |
| Annualised Volatility | 24.28% |
| Sharpe Ratio | 0.239 |
| Sortino Ratio | 0.332 |
| Maximum Drawdown | -32.08% |
| Beta vs S&P 500 | 0.961 |
| Jensen's Alpha | 24.09% |
| VaR (95%, 1-day) | -2.49% |
| CVaR (95%, 1-day) | -3.45% |
| Win Rate | 49.9% |
| Best Single Day | +6.50% |
| Worst Single Day | -6.57% |

### Table 2. 10-Year Total Returns (2007-2016)

| Ticker | Company | 10-Year Total Return |
|---|---|---|
| AAPL | Apple Inc. | +806.8% |
| SBUX | Starbucks Corp. | +271.8% |
| MSFT | Microsoft Corp. | +119.5% |
| IBM | IBM Corp. | +66.9% |
| GSPC | S&P 500 Index | +39.6% |

Apple delivered 806.8% over the decade, outperforming the S&P 500 index by more than 20 times on a total return basis. Starbucks was the second-best performer at 271.8%.

### Table 3. Predictive Model Comparison

| Model | RMSE | R² | MAPE | CV R² |
|---|---|---|---|---|
| Linear Regression | 1.2897 | 0.965 | 0.719% | 0.642 |
| Ridge (alpha = 1.0) | 1.2928 | 0.9648 | 0.739% | 0.2128 |

Linear Regression achieved R² = 0.965 on the out-of-sample test set. Ridge regression produced nearly identical RMSE (1.2928 vs 1.2897) with a lower cross-validated R² (0.213 vs 0.642), indicating that the linear model generalises better for this feature set with time-series cross-validation.

---

## Strategic Recommendations from the Dashboard

The dashboard synthesises all analytical findings into six actionable recommendations derived directly from the data.

**Risk-Adjusted Returns.** AAPL delivered a Sharpe ratio of 0.24. A ratio below 1.0 suggests that the return per unit of risk is modest and that sector rotation or hedging strategies should be considered for portfolios with AAPL as a core holding.

**Volatility and Beta.** Beta of 0.961 classifies AAPL as a near-market-beta stock. Portfolio managers should size positions in line with benchmark tracking targets to manage systematic exposure during broad market drawdowns.

**Value Screening.** 64 companies in the S&P 500 score 100 out of 100 on the Graham value screen (low P/E, low P/B, positive EPS, positive dividend yield). These present the highest margin-of-safety opportunities in the index at the time of the dataset.

**Sector Rotation Signal.** Sectors with P/E below 15 and dividend yield above 2% historically outperform during monetary tightening cycles. Utilities, Financials, and Consumer Staples screen favourably on these combined criteria.

**Predictive Model Deployment.** Linear Regression achieves R² = 0.965 on out-of-sample test data. Combining the model signal with RSI divergence and MACD crossover produces a multi-signal entry and exit framework with improved precision over any single indicator alone.

**Seasonal Strategy.** Monthly return analysis reveals consistent historical patterns. Deploying capital in historically strong months while reducing exposure in historically weak months can improve portfolio CAGR by an estimated 1 to 3 percentage points annually.

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

Install all dependencies:

```bash
pip install numpy pandas "plotly>=5.14" scipy scikit-learn statsmodels
```

---

## How to Run

**Option 1 — Python script**

```bash
git clone https://github.com/Apollop24/stock-market-intelligence-analytics-platform.git
cd stock-market-intelligence-analytics-platform
pip install numpy pandas "plotly>=5.14" scipy scikit-learn statsmodels
python code/stock_intelligence_platform.py
```

**Option 2 — Jupyter Notebook**

```bash
jupyter notebook code/stock_intelligence_platform.ipynb
```

**Option 3 — View dashboard directly**

Open `dashboard/stock_intelligence_dashboard.html` in any modern browser. No Python required.

---

## Statistical Framework

Tabachnick, B. G., and Fidell, L. S. (2019). *Using Multivariate Statistics* (7th ed.). Pearson.

Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C., and Wirth, R. (2000). *CRISP-DM 1.0: Step-by-step data mining guide*. SPSS Inc.

---

## Data Citations

Pollock, R., and Open Knowledge Foundation contributors. (2020). *S&P 500 companies with financial information* [Dataset]. Open Knowledge Foundation. https://github.com/datasets/s-and-p-500-companies-financials. Licensed under ODbL v1.0.

Plotly Technologies Inc. (2023). *Plotly datasets* [Dataset repository]. GitHub. https://github.com/plotly/datasets. Original price data sourced from Yahoo Finance.

---

## Author

Philip Kibet
Statistician and Data Analyst
Nairobi, Kenya

---

## Disclaimer

This repository is developed for analytical and portfolio demonstration purposes only. Nothing in this platform constitutes financial advice. All analyses are retrospective and based on historical data. Past market behaviour does not guarantee future results.
