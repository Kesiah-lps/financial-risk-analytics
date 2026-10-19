# financial-risk-analytics
# Market Risk Analytics — S&P 500 Stocks (2014–2017)

## Overview

This project explores historical market risk using daily stock price data from 2014 to 2017.

The objective is to analyze stock returns, measure downside risk, identify major drawdowns, and evaluate the performance of a historical Value at Risk (VaR) model through backtesting.

The project was developed using Python and focuses on exploratory data analysis, risk measurement, and the interpretation of historical financial data.

## Objectives

- Analyze daily stock returns and price movements.
- Measure historical volatility and annualized volatility.
- Identify the largest peak-to-trough losses using maximum drawdown.
- Estimate Value at Risk (VaR) and Conditional Value at Risk (CVaR).
- Evaluate a historical VaR model using a rolling-window backtesting approach.
- Identify data quality issues and discuss their impact on risk estimates.

## Dataset

**Source:** [Stock Prices — Kaggle](https://www.kaggle.com/datasets/mysarahmadbhat/stock-prices)

The dataset contains daily stock market observations with the following variables:

| Column | Description |
|---|---|
| `symbol` | Stock ticker |
| `date` | Trading date |
| `open` | Opening price |
| `high` | Highest price during the session |
| `low` | Lowest price during the session |
| `close` | Closing price |
| `volume` | Trading volume |

### Dataset overview

- **497,472 rows**
- **505 stock symbols**
- **Date range:** January 2014 – December 2017

The dataset is not fully balanced across stocks, as some symbols have fewer observations than others.

## Methodology

### 1. Data Cleaning and Exploration

The initial analysis includes:

- Checking data types and missing values.
- Detecting duplicate observations.
- Checking price consistency.
- Investigating missing or incomplete observations.
- Sorting records chronologically for each stock.

Basic data quality checks identified a small number of observations with price inconsistencies, which require careful interpretation.

### 2. Returns and Volatility

Daily returns are calculated using closing prices:

\[
r_t=\frac{P_t}{P_{t-1}}-1
\]

Daily volatility is estimated using the standard deviation of daily returns.

Annualized volatility is estimated using the conventional approximation:

\[
\sigma_{\text{annual}}=\sigma_{\text{daily}}\sqrt{252}
\]

This assumes approximately 252 trading sessions per year.

### 3. Maximum Drawdown

Maximum drawdown measures the largest decline from a historical peak to a subsequent trough.

A wealth index is constructed by compounding daily returns, and each observation is compared with the highest wealth index reached up to that date.

The resulting drawdown series helps identify stocks that experienced substantial cumulative losses during the study period.

### 4. Value at Risk (VaR) and Conditional Value at Risk (CVaR)

Historical VaR at 95% is estimated using the 5th percentile of daily returns.

VaR represents a loss threshold associated with the lower tail of the historical return distribution.

CVaR at 95% estimates the average loss among observations whose returns fall at or below that 5th-percentile threshold.

These measures provide complementary views of downside risk: VaR estimates a threshold, while CVaR describes the severity of losses beyond it.

### 5. Historical VaR Backtesting

A rolling-window approach is used to evaluate the historical VaR model.

For each stock:

1. The previous 250 daily returns are used to estimate the 5th-percentile threshold.
2. The estimated threshold is compared with the return observed on the following day.
3. An exception is recorded whenever the actual return falls below the estimated threshold.
4. The process is repeated through time.

For a VaR model at 95%, the expected long-run exception frequency is approximately 5%.

The observed exception rates are compared with this target to investigate whether the historical VaR method adequately captures downside risk.

## Key Findings

- The historical return distributions reveal differences in volatility and downside risk across stocks.
- Maximum drawdown highlights stocks that experienced substantial peak-to-trough declines during the observation period.
- VaR and CVaR provide different perspectives on downside risk, distinguishing a loss threshold from the average severity of losses in the lower tail.
- In the rolling-window backtest, **341 out of 499 tested stocks exhibited exception rates above 5%**.
- This result suggests that the historical VaR model may underestimate downside risk for a substantial proportion of the tested stocks.

The observed exception rate alone does not establish statistical model failure. Further analysis would be required to assess the statistical significance of these deviations and whether exceptions cluster during particular market conditions.

## Limitations

- The analysis covers a historical period from 2014 to 2017 and does not establish how the same methods would perform under current market conditions.
- The dataset documentation does not confirm whether closing prices are adjusted for dividends, stock splits, and other corporate actions.
- Returns calculated from unadjusted closing prices may therefore contain misleading movements around corporate actions.
- Historical risk estimates do not guarantee future outcomes.
- The backtesting analysis evaluates exception frequency but does not yet include formal statistical coverage tests or tests of exception independence.

These limitations should be considered when interpreting the results.

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Jupyter Notebook

## How to Run

1. Clone or download this repository.
2. Install the required Python libraries.
3. Download the dataset from Kaggle.
4. Place the dataset in the appropriate project directory.
5. Open the Jupyter Notebook and run the cells sequentially.

Example installation:

```bash
pip install pandas numpy matplotlib jupyter
```

## Future Improvements

- Use adjusted stock prices to obtain more consistent return estimates.
- Compare historical VaR with alternative risk estimation methods.
- Investigate the influence of extreme observations and corporate actions.
- Evaluate VaR performance across different rolling-window lengths.
- Extend the analysis to portfolio-level risk and diversification.
- Add statistical backtesting methods when appropriate.

## Author

Developed as a personal project to explore financial data analysis and historical market risk measurement using Python.