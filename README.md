# India Factor Investing Backtest Engine
### Systematic factor analysis on NSE large-cap equities | 2020–2026

---

## Overview

This project implements a bottom-up factor investing framework applied to India's large-cap equity universe (Nifty 20 constituents). Three factors are independently tested and then combined into a composite multi-factor model, with portfolio construction and performance attribution done entirely from scratch using Python.

The goal: determine whether classic equity factors — momentum, volatility, and short-term reversal — generate meaningful alpha in the Indian market, and whether combining them produces a more robust signal.

---

## Factors Tested

| Factor | Definition | Signal Direction |
|---|---|---|
| **Momentum (12-1)** | 12-month return, skipping last month (Jegadeesh-Titman) | Long winners |
| **Rolling Volatility** | 12-month rolling std of monthly returns | Long low-vol stocks |
| **Short-Term Reversal** | Prior month return, inverted | Long last month's losers |
| **Combined** | Equal-weight average rank across all three | Long composite top quintile |

---

## Key Results

> Backtest period: March 2020 – March 2026 | Universe: 18–20 NSE large-cap stocks | Rebalancing: Monthly

### Q3 Portfolio Performance (Top Quintile, Long Only)

| Factor | Ann. Return | Sharpe Ratio | Max Drawdown |
|---|---|---|---|
| Momentum | ~22.4% | 0.92 | — |
| Volatility | ~21.7% | 0.89 | — |
| Short-Term Reversal | ~23.7% | 0.89 | — |
| **Combined (Multi-Factor)** | **~21.8%** | **1.08** | **-13.7%** |

**Key finding:** The combined multi-factor model achieves the highest Sharpe ratio (1.08) across all tested strategies, demonstrating the diversification benefit of factor combination even within a concentrated large-cap universe.

**Notable observation:** High-volatility stocks outperformed low-volatility stocks in this universe over the backtest period — contrary to the classic low-volatility anomaly documented in global markets. This likely reflects India's structural growth premium and retail-driven risk appetite.

### Portfolio Construction Methodology

- Stocks ranked cross-sectionally within each month (rank 1 = worst, 18 = best)
- Universe split into tertiles (Q1 = bottom, Q3 = top)
- Equal-weighted portfolios rebalanced monthly
- Long-short (LS) = Q3 minus Q1 return
- Risk-free rate: 6.5% (approximate Indian 10Y G-Sec yield)

---

## Project Structure

```
india-factor-backtest/
│
├── data/
│   ├── nifty20_prices.csv          # Monthly price data, NSE large caps
│   ├── momentum_portfolios.csv     # Momentum Q1/Q2/Q3/LS returns
│   ├── vol_portfolios.csv          # Volatility portfolio returns
│   ├── reversal_portfolios.csv     # Reversal portfolio returns
│   ├── combined_portfolios.csv     # Multi-factor combined returns
│   └── multi_factor_results.png   # Cumulative return + Sharpe bar chart
│
└── combined_factors.ipynb          # Main analysis notebook (all factors)
```

---

## How to Run

**Requirements:**
```
pandas
numpy
matplotlib
```

**Steps:**
1. Clone the repository
2. Place your price data CSV in the `data/` folder (monthly OHLC or close prices, stocks as columns, dates as index)
3. Run `combined_factors.ipynb` end to end

The notebook is self-contained — factor construction, portfolio building, performance attribution, and charting are all in one file.

---

## Methodology Notes

- **Momentum** is computed as the 11-month return starting 13 months ago to 2 months ago (standard Jegadeesh-Titman 12-1 construction), lagged by one month to avoid look-ahead bias
- **Volatility** factor inverts the raw volatility score so that low-vol stocks receive higher ranks — this produced a counterintuitive result in the Indian large-cap universe (see findings above)
- **Combined score** is an arithmetic average of the three factor ranks, recomputed cross-sectionally each month
- No transaction costs or slippage modeled — results represent gross returns

---

## Background

Built as part of a personal research initiative to understand factor return dynamics in Indian equity markets. India's market microstructure, retail participation, and growth cycle differ meaningfully from developed markets — which makes replicating global factor findings a non-trivial and interesting empirical question.

---

## Author

**Atulya** | Quantitative Investment Strategies, Goldman Sachs Asset Management  
[GitHub](https://github.com/shekhar-atulya)
