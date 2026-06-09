# Data Sources, Citations, and Lineage

## Overview

All datasets used in this platform are publicly accessible and fetched directly
from raw GitHub URLs at runtime. No API keys, registration, or local downloads
are required. The data lineage below documents every source used, the licence
under which it is distributed, and the exact URL used for retrieval.

---

## Dataset 1 — S&P 500 Fundamentals

| Field         | Detail |
|---|---|
| Name          | S&P 500 Companies with Financial Information |
| Maintainer    | Rufus Pollock and Open Knowledge Foundation contributors |
| Repository    | https://github.com/datasets/s-and-p-500-companies-financials |
| Raw URL       | https://raw.githubusercontent.com/datasets/s-and-p-500-companies-financials/master/data/constituents-financials.csv |
| Format        | CSV |
| Records       | 503 companies |
| Fields        | Symbol, Name, Sector, Price, Price/Earnings, Dividend Yield, Earnings/Share, 52 Week Low, 52 Week High, Market Cap, EBITDA, Price/Sales, Price/Book, SEC Filings |
| Licence       | Open Data Commons Open Database License (ODbL) v1.0 |
| Licence URL   | https://opendatacommons.org/licenses/odbl/1-0/ |
| Last verified | 2025 |

### Citation (APA 7th)

Pollock, R., and Open Knowledge Foundation contributors. (2020).
*S&P 500 companies with financial information* [Dataset].
Open Knowledge Foundation, datasets project.
https://github.com/datasets/s-and-p-500-companies-financials

### Features engineered from this dataset

- MarketCap_B: market capitalisation in billions USD
- EBITDA_B: EBITDA in billions USD
- Week52Range: 52-week high minus 52-week low
- PriceMomentum: position of current price within 52-week range (0 to 1)
- GrahamScore: composite value score (0, 25, 50, 75, 100) based on P/E, P/B, EPS positivity, and dividend yield
- ValueClass: categorical classification (Deep Value, Value-Leaning, Neutral, Speculative)

---

## Dataset 2 — Apple Inc. OHLCV Price Data

| Field         | Detail |
|---|---|
| Name          | Finance Charts Apple (AAPL OHLCV with Bollinger Bands) |
| Maintainer    | Plotly Technologies Inc. |
| Repository    | https://github.com/plotly/datasets |
| Raw URL       | https://raw.githubusercontent.com/plotly/datasets/master/finance-charts-apple.csv |
| Format        | CSV |
| Records       | 506 trading days |
| Coverage      | 2015-02-17 to 2017-02-16 |
| Fields        | Date, AAPL.Open, AAPL.High, AAPL.Low, AAPL.Close, AAPL.Volume, AAPL.Adjusted, BB Lower (dn), BB Mid (mavg), BB Upper (up), direction |
| Underlying source | Yahoo Finance historical data |
| Licence       | MIT License |
| Licence URL   | https://github.com/plotly/datasets/blob/master/LICENSE |
| Last verified | 2025 |

### Citation (APA 7th)

Plotly Technologies Inc. (2023). *Finance charts Apple* [Dataset].
Plotly datasets repository, GitHub.
https://github.com/plotly/datasets

Original price data: Yahoo Finance (https://finance.yahoo.com).

### Features engineered from this dataset

28 features per trading day:

- Returns: DailyReturn, LogReturn, CumReturn
- Trend: MA20, MA50, MA200
- Volatility: Volatility20 (20-day rolling annualised)
- Momentum: RSI(14), MACD(12,26,9), MACD_Signal, MACD_Hist
- Risk: Drawdown from rolling peak
- Spread: HL_Spread (High minus Low), OC_Change (Close minus Open)
- Volume: Volume_MA20
- Calendar: Month, Quarter, DayOfWeek, Year

---

## Dataset 3 — Multi-Stock Close Prices

| Field         | Detail |
|---|---|
| Name          | Stock Data — Multi-Ticker Close Prices |
| Maintainer    | Plotly Technologies Inc. |
| Repository    | https://github.com/plotly/datasets |
| Raw URL       | https://raw.githubusercontent.com/plotly/datasets/master/stockdata.csv |
| Format        | CSV |
| Records       | 2,306 trading days |
| Coverage      | 2007-01-03 to 2016-12-29 |
| Tickers       | AAPL (Apple Inc.), MSFT (Microsoft Corp.), IBM (IBM Corp.), SBUX (Starbucks Corp.), GSPC (S&P 500 Index) |
| Underlying source | Yahoo Finance historical data |
| Licence       | MIT License |
| Licence URL   | https://github.com/plotly/datasets/blob/master/LICENSE |
| Last verified | 2025 |

### Citation (APA 7th)

Plotly Technologies Inc. (2023). *Stock data — multi-ticker close prices* [Dataset].
Plotly datasets repository, GitHub.
https://github.com/plotly/datasets

Original price data: Yahoo Finance (https://finance.yahoo.com).

### Features engineered from this dataset

- Normalised price series (base 100 at 2007-01-03) for each ticker
- Daily returns for each ticker
- Rolling 30-day beta of AAPL, MSFT, IBM, and SBUX against the S&P 500

---

## Data Lineage Summary

| Phase | Dataset Used | Purpose |
|---|---|---|
| Feature Engineering | AAPL OHLCV | 28 engineered features per trading day |
| Data Quality | AAPL OHLCV | Missing values, normality, outliers, stationarity |
| EDA — Descriptive | AAPL OHLCV | Price distributions, return profile, Bollinger Bands |
| EDA — Multi-Stock | Multi-Stock Close | Normalised performance, correlation |
| Statistical Analysis | AAPL OHLCV + Multi-Stock | OLS regression, VIF, beta, heteroskedasticity |
| Business Analytics | S&P 500 Fundamentals | Sector treemap, valuation, Graham score |
| Predictive Analytics | AAPL OHLCV | Ridge regression rolling forecast |
| Executive Dashboard | All three datasets | Integrated interactive visualisation |

---

## Reproducibility Note

All data is fetched at runtime from the raw URLs listed above. No local data
files are committed to this repository. To reproduce results exactly, run
the platform script or notebook with an active internet connection. Results
may differ marginally if the upstream repositories update their data files.

To pin a specific version of the data, fork the upstream repositories and
update the URL constants in `code/stock_intelligence_platform.py`:

```python
DATA_SOURCES = {
    "sp500_fundamentals": "https://raw.githubusercontent.com/YOUR_FORK/...",
    "aapl_ohlcv":         "https://raw.githubusercontent.com/YOUR_FORK/...",
    "multi_stock_close":  "https://raw.githubusercontent.com/YOUR_FORK/...",
}
```
