# sector-etf-fedfunds-2010-2024-or-shorter-etf-fedrate-sensitivity
Time-series analysis of sector ETF returns (XLK, XLF, XLU) vs the Federal Funds Rate, 2010–2024. Includes data cleaning, ADF, cointegration, Granger causality, and regression.
# Sector ETF Interest Rate Sensitivity (2010–2024)

This repository contains the data and code for my honors research project on how
sector ETF returns respond to changes in the Federal Funds Rate.

I study three sector ETFs:

- **XLK** – Technology
- **XLF** – Financials
- **XLU** – Utilities

over the period **2010–2024**, and combine them with the effective Federal
Funds Rate from FRED.

## What this project does

- Cleans and merges daily ETF price data with Fed Funds Rate data
- Constructs daily return and rate-change series
- Runs:
  - Augmented Dickey–Fuller (ADF) tests for stationarity
  - Engle–Granger cointegration tests
  - Granger causality tests (multiple lags)
  - OLS regressions of ΔETF returns on ΔFedRate
- Produces charts:
  - ETF & FedRate time-series plots
  - Impulse responses from VAR
  - Regression diagnostics (residual histograms, Q–Q plots, residuals vs fitted)

## Key findings (very short)

- ETF prices and the Fed Funds Rate are **non-stationary** in levels and **not cointegrated**.
- In first differences, **changes in the Fed Funds Rate Granger-cause sector ETF
  returns**, especially XLK.
- A 1 percentage point move in the Fed Funds Rate is associated with roughly a
  **–5.6% change in XLK’s daily return**, but the model explains **<1% of daily
  variance**, so rates are a real but weak driver of day-to-day moves.

Full write-up: see [`docs/Research_Note_ETF_FedFunds_2010_2024.pdf`](docs/Research_Note_ETF_FedFunds_2010_2024.pdf).

## Repo structure

```text
data/        # raw input data (ETF prices + FRED FedFunds series)
notebooks/   # Jupyter notebook with full analysis
docs/        # PDF research note used on my website
