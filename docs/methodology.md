# Statistical Methodology and Analytical Decisions

## Framework

This platform follows two guiding frameworks applied consistently across all phases.

**CRISP-DM** (Chapman et al., 2000) provides the process structure: Business Understanding,
Data Understanding, Data Preparation, Modelling, Evaluation, and Deployment map
directly to the seven analytical phases of the platform.

**Tabachnick and Fidell (2019)** governs all statistical decisions: sample size
requirements, outlier thresholds, normality criteria, and regression diagnostic
interpretation are applied as specified in *Using Multivariate Statistics* (7th ed.).

---

## Phase 1 — Feature Engineering Decisions

### Returns

Daily return is computed as the percentage change in closing price. Log return
is computed as the natural logarithm of the price ratio. Log returns are used
for statistical testing because they are more likely to satisfy normality
assumptions and are time-additive.

Cumulative return uses the compounding formula: product of (1 + daily return) minus 1.

### Volatility

Annualised volatility is computed as the 20-day rolling standard deviation of
daily returns multiplied by the square root of 252 (trading days per year).
This is the standard realised volatility estimator used in institutional practice.

### RSI

The Relative Strength Index is computed over a 14-period window using Wilder's
smoothed moving average method. Values above 70 indicate overbought conditions;
values below 30 indicate oversold conditions.

### MACD

The Moving Average Convergence Divergence indicator uses the standard (12, 26, 9)
parametrisation: the MACD line is the 12-period EMA minus the 26-period EMA;
the signal line is the 9-period EMA of the MACD line; the histogram is MACD
minus signal.

### Drawdown

Drawdown is computed as the percentage decline from the rolling cumulative maximum.
It measures the loss an investor would have experienced from the peak to any given point.

---

## Phase 2 — Data Quality Decisions

### Missing Values

Missing values arising from rolling window calculations (the first 19 observations
for a 20-period window, for example) are treated as structural missingness rather
than data errors. They are retained in the dataset with NaN values and excluded
from calculations as needed by each analytical function.

### Normality Testing

Shapiro-Wilk is used as the primary normality test because it has the highest
power for samples up to 5,000 observations (Razali and Wah, 2011). The test
is applied to the first 500 observations to remain within the optimal range.
The significance threshold is p > 0.05 for approximate normality.

Daily returns and log returns are expected to deviate from normality due to
fat tails and volatility clustering, which is a well-documented property of
financial return series. Deviation from normality does not disqualify these
series from regression analysis given the large sample size (n = 506), per
the central limit theorem.

### Outlier Detection

The Z-score threshold of |Z| > 3.29 follows Tabachnick and Fidell (2019),
who specify this value as the 0.001 two-tailed critical value. Outliers
identified in daily return series are retained because extreme returns
(fat tails) are a structural feature of financial data, not measurement error.

### Stationarity

The Augmented Dickey-Fuller test is applied to both price levels and daily
returns. Price levels are expected to be non-stationary (unit root present).
Daily returns are expected to be stationary. Regression models use stationary
return series to avoid spurious correlation.

---

## Phase 3 — Statistical Analysis Decisions

### Correlation Analysis

Both Pearson and Spearman correlations are computed. Pearson correlation
measures linear association and assumes approximate bivariate normality.
Spearman correlation is non-parametric and robust to non-normality and outliers.
Both are reported to allow comparison.

### OLS Regression

The dependent variable is Apple daily return. Independent variables are
lagged by one period to avoid look-ahead bias: volume relative to its
20-day average, 20-day volatility, and the MACD histogram.

### Multicollinearity

Variance Inflation Factor is computed for each predictor. VIF values above
10 indicate problematic multicollinearity per Tabachnick and Fidell (2019).
Values between 5 and 10 warrant caution.

### Heteroskedasticity

The Breusch-Pagan test is used to test the null hypothesis of homoskedastic
residuals. Rejection (p < 0.05) indicates that variance of residuals depends
on fitted values. Financial return regressions commonly fail this test due to
volatility clustering.

### Autocorrelation

The Durbin-Watson statistic tests for first-order autocorrelation in residuals.
Values near 2.0 indicate no autocorrelation. Values below 1.5 indicate positive
autocorrelation, which inflates t-statistics and requires correction.

---

## Phase 5 — Predictive Analytics Decisions

### Cross-Validation

TimeSeriesSplit with 5 folds is used in place of standard k-fold cross-validation.
Standard k-fold randomly shuffles observations, which introduces look-ahead bias
in time series data. TimeSeriesSplit preserves temporal order: each fold trains
on all data up to a point and tests on the immediately following period.

### Ridge Regression

Ridge regression is used instead of OLS for the predictive model because it
applies L2 regularisation, which shrinks coefficient estimates toward zero and
reduces variance in out-of-sample predictions. This is particularly important
in financial forecasting where predictors are often weakly correlated with
the target and overfitting is a primary concern.

### Feature Standardisation

All features are standardised (zero mean, unit variance) before Ridge regression
using StandardScaler. This ensures that the regularisation penalty is applied
uniformly across features regardless of their original scale.

### Model Evaluation

R-squared measures the proportion of variance explained. MAE (Mean Absolute Error)
is reported in the original units of the target variable (daily return as a decimal).
RMSE (Root Mean Squared Error) penalises large errors more heavily than MAE and is
the primary metric for model comparison.

---

## Graham Score Methodology

The Graham score assigns 25 points for each of four conditions:

1. P/E ratio is positive and below the 33rd percentile of the S&P 500 distribution
2. P/B ratio is positive and below the 33rd percentile of the S&P 500 distribution
3. Earnings per share is positive
4. Dividend yield is positive

This scoring approach is inspired by Benjamin Graham's criteria in *The Intelligent
Investor* (Graham, 1949) and adapted for cross-sectional screening of the S&P 500.
It is a screening heuristic, not a complete valuation model.

---

## References

Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C.,
and Wirth, R. (2000). *CRISP-DM 1.0: Step-by-step data mining guide*. SPSS Inc.

Graham, B. (1949). *The intelligent investor*. Harper and Brothers.

Razali, N. M., and Wah, Y. B. (2011). Power comparisons of Shapiro-Wilk,
Kolmogorov-Smirnov, Lilliefors and Anderson-Darling tests. *Journal of
Statistical Modeling and Analytics*, 2(1), 21-33.

Tabachnick, B. G., and Fidell, L. S. (2019). *Using multivariate statistics*
(7th ed.). Pearson.
