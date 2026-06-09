# Key Findings and Executive Summary

## Platform Summary

This platform analysed three real-world financial datasets covering 503 S&P 500
companies, 506 Apple trading days (2015-2017), and 2,306 multi-stock trading
days (2007-2016). All findings below are derived from publicly available data
and are presented for analytical demonstration purposes only.

---

## Data Quality Findings

The Apple OHLCV dataset contained no missing values in core price and volume
fields. Structural missingness in engineered features (RSI, MACD, moving averages)
arises from the required lookback windows and is expected and documented.

Normality testing confirmed that Apple daily returns and log returns deviate
significantly from a normal distribution (Shapiro-Wilk p < 0.05), consistent
with the well-established empirical finding of fat tails in financial returns.
Price levels are non-stationary (Augmented Dickey-Fuller, p > 0.05), while
daily returns are stationary (p < 0.01), which is the expected and required
property for regression modelling.

Outlier analysis using the Tabachnick and Fidell (2019) threshold of |Z| > 3.29
identified a small number of extreme daily returns. These are retained in the
dataset because extreme returns are a structural feature of equity markets,
not measurement artefacts.

---

## Apple Price Analysis (2015-2017)

Apple traded between approximately USD 90 and USD 133 during the analysis period.
The Bollinger Band structure and moving average crossovers captured the two
dominant price regimes: a declining phase through mid-2016 and a recovery
phase from late 2016 through early 2017.

Annualised 20-day realised volatility ranged from approximately 12% to 45%,
with elevated volatility coinciding with the earnings-driven decline in 2016.
Maximum drawdown from the January 2015 peak reached approximately 32%.

RSI oscillations between 30 and 70 were broadly consistent with a range-bound
to mildly trending market, with no sustained extreme readings indicating a
prolonged overbought or oversold condition across the full period.

---

## Multi-Stock Comparative Analysis (2007-2016)

On a base-100 normalised basis from January 2007, performance diverged
substantially across the five tickers by 2016. Apple and Starbucks were the
strongest performers over the decade. IBM underperformed the S&P 500 index
from approximately 2013 onward. Microsoft re-rated materially following its
strategic pivot from 2014.

The correlation structure across tickers was moderately positive, consistent
with market beta dominating individual stock returns during periods of broad
market stress (2008-2009 financial crisis visible in all series).

Rolling 30-day beta estimates confirmed that all four stocks tracked the S&P
500 with varying sensitivity, with Apple and Starbucks showing periods of
both high and low beta depending on company-specific news cycles.

---

## S&P 500 Fundamental Analysis

Information Technology was the dominant sector by market capitalisation,
followed by Health Care and Financials. The sector treemap illustrates
the concentration of index weight in a relatively small number of large-cap
technology companies.

The Graham score classification identified a subset of companies meeting
all four value criteria (Deep Value, score = 100) concentrated primarily
in the Financials, Energy, and Materials sectors, which traded at below-median
P/E and P/B ratios relative to the full index at the time of the dataset.

Valuation scatter analysis confirmed the expected negative relationship
between market capitalisation and P/B ratio at the large-cap end, and wide
dispersion in P/E ratios across the mid-cap universe.

---

## Regression and Predictive Modelling

OLS regression of Apple daily return on lagged volume ratio, lagged volatility,
and lagged MACD histogram produced modest but statistically meaningful results.
Regression diagnostics flagged heteroskedastic residuals, consistent with
volatility clustering in equity return series. VIF values confirmed low
multicollinearity among the selected predictors.

Ridge regression with time-series cross-validation (5 folds) produced
consistent out-of-sample R-squared values across folds, with RMSE values
in line with the unconditional standard deviation of daily returns. This
confirms that the features carry modest predictive signal but that short-term
return prediction remains inherently limited, as expected for a liquid large-cap
equity.

---

## Risk Metrics Summary

| Metric | Interpretation |
|---|---|
| Sharpe Ratio | Annualised risk-adjusted return relative to 2% risk-free rate |
| Sortino Ratio | Risk-adjusted return penalising only downside volatility |
| Beta (30-day rolling) | Sensitivity of each stock return to S&P 500 movement |
| VaR (95%) | Maximum expected daily loss in 95% of trading days |
| CVaR (95%) | Mean loss on the worst 5% of trading days |
| Maximum Drawdown | Largest peak-to-trough cumulative loss in the analysis period |

All metric values are computed and displayed interactively in the executive
dashboard (`dashboard/stock_intelligence_dashboard.html`).

---

## Disclaimer

All findings are derived from historical data for analytical demonstration
purposes. Nothing in this platform constitutes investment advice or a
recommendation to buy or sell any security. Past market performance does
not guarantee future results.
