# Sector ETF Interest Rate Sensitivity (2010–2024)

Time-series analysis of sector ETF returns (XLK, XLF, XLU) vs the Federal Funds Rate, 2010–2024. Includes data cleaning, ADF, cointegration, Granger causality, and regression.

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

## Repo structure

```text
data/        # raw input data (ETF prices + FRED FedFunds series)
notebooks/   # Jupyter notebook with full analysis
docs/        # PDF research note used on my website

Graphs import pandas as pd

# Load data
file_path = "Sheet 1-1-etf_prices_with_fed_trends.csv"  # or full path if needed
df = pd.read_csv(file_path, parse_dates=["Date"])
df.set_index("Date", inplace=True)

# Convert columns to numeric and drop missing
df[["XLK", "FedRate"]] = df[["XLK", "FedRate"]].apply(pd.to_numeric, errors="coerce")
df.dropna(subset=["XLK", "FedRate"], inplace=True)

# Preview
print(df.head())
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
from statsmodels.graphics.gofplots import qqplot
from sklearn.linear_model import LinearRegression
import numpy as np

# Load and prepare data
file_path = "Sheet 1-1-etf_prices_with_fed_trends.csv"
df = pd.read_csv(file_path, parse_dates=["Date"])
df.set_index("Date", inplace=True)
df[["XLK", "FedRate"]] = df[["XLK", "FedRate"]].apply(pd.to_numeric, errors="coerce")
df.dropna(subset=["XLK", "FedRate"], inplace=True)

# Define target (ETF) and predictor (FedRate)
X = df[["FedRate"]]
y = df["XLK"]

# Fit model
model = LinearRegression()
model.fit(X, y)
df["Predicted"] = model.predict(X)
df["Residuals"] = y - df["Predicted"]

# Print summary stats
print("R-squared:", model.score(X, y))
print("Intercept:", model.intercept_)
print("Slope:", model.coef_[0])

# === Plot 1: Regression Line ===
plt.figure()
sns.regplot(x="FedRate", y="XLK", data=df, line_kws={"color": "red"})
plt.title("XLK vs Fed Funds Rate")
plt.xlabel("Fed Funds Rate")
plt.ylabel("XLK Price")
plt.grid(True)
plt.show()

# === Plot 2: Residuals ===
plt.figure()
sns.histplot(df["Residuals"], kde=True)
plt.title("Distribution of Residuals")
plt.xlabel("Residuals")
plt.grid(True)
plt.show()

# === Plot 3: Q-Q Plot ===
plt.figure()
qqplot(df["Residuals"], line='s')
plt.title("Q-Q Plot of Residuals")
plt.grid(True)
plt.show()

# === Plot 4: Residuals vs Fitted ===
plt.figure()
plt.scatter(df["Predicted"], df["Residuals"], alpha=0.5)
plt.axhline(y=0, color='r', linestyle='--')
plt.title("Residuals vs Fitted Values")
plt.xlabel("Fitted Values")
plt.ylabel("Residuals")
plt.grid(True)
plt.show()

