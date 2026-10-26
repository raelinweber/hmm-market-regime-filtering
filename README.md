# Market Regime Filtering with Gaussian Hidden Markov Models

A Data Science capstone project investigating whether regime-based filtering improves risk-adjusted performance of two technical trading strategies on the S&P 500.

## Research question

Does applying Gaussian HMM market-regime filters improve the risk-adjusted performance of a 20/50-day simple moving average crossover and a 20-day breakout strategy compared with the same rules used without filtering?

## Approach

- **Market:** Daily S&P 500 index prices, with source data covering 2000–2024.
- **Features:** Volatility, SMA50, SMA20/SMA50 ratio, RSI14 and ROC14, with a correlation screen within each rolling training window.
- **Model:** Gaussian HMM, candidate 2–4 states chosen using BIC.
- **Validation:** Rolling 31-observation training windows, with out-of-sample prediction after each window and next-open trading execution. Development period 2000–2005 excluded from final walk-forward training and testing.
- **Strategy comparison:** Both rules tested with and without a favourable-regime filter, allowing 5 bps transaction fees plus 2 bps slippage per position change.

## Final results

| Strategy | Cumulative net return | Annualised Sharpe | Maximum drawdown |
|---|---:|---:|---:|
| SMA unfiltered | 128.992% | 0.446 | -29.432% |
| SMA HMM filtered | 21.577% | 0.166 | -18.950% |
| Breakout unfiltered | 98.954% | 0.396 | -40.990% |
| Breakout HMM filtered | 40.293% | 0.261 | -29.462% |

The final experiment produced **4,749 regime predictions** and **4,747 holding-period returns** (execution dates 2006-02-17 through 2024-12-27). Filtering reduced maximum drawdowns but did not improve annualised Sharpe ratios. Filtered strategies also incurred higher trading costs because they changed positions more frequently.

## Repository layout

```text
HMM_Market_Regimes.ipynb                Main project notebook
requirements.txt                        Python dependencies
experiment_config.json                  Recorded experiment configuration
final_performance.csv                   Verified final strategy summary
hmm_dataset.csv                         Primary market data (add locally if redistribution is permitted)
final_report_outputs/
  methodology_aligned_predictions_strict.csv   4,749 predictions (copy from local project)
```

## Running the project

1. Use a compatible Python environment and install `pip install -r requirements.txt`. The recorded run used Python 3.14.6 and dependency versions in `requirements.txt`.
2. Place your original `hmm_dataset.csv` alongside the notebook, respecting the original data provider's redistribution terms.
3. Copy the **final verified** `methodology_aligned_predictions_strict.csv` into `final_report_outputs/`.
4. Open `HMM_Market_Regimes.ipynb` in Jupyter or VS Code and run its cells in order. With `RUN_HMM_TRAINING = False` (the default), the notebook reads the completed checkpoint and reproduces the evaluation without retraining.
5. If the checkpoint is unavailable, the notebook documents an optional batch training/resume mode, but it requires substantial computation.

**Important:** This starter package does not yet contain the local prediction checkpoint or original Yahoo Finance data. Add these before advertising the repository as completely rerunnable, and check licensing before making the dataset public. Do not add unrelated drafts, passwords, tokens or personal information.

## Limitations

The underlying S&P 500 index is not directly tradable. The short training window may produce unstable state estimates, and historical backtests do not establish future profitability. A numerical Deflated Sharpe Ratio was not reported because an effective number of independent performance-selection trials could not be established; PSR statistics were reported separately as supplementary diagnostics.

## Contributors

Raelin Weber · Angelique Gotyana · Cole Campher · Paseka Nkabinde
