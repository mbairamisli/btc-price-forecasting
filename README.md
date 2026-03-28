## Bitcoin Price Forecasting Using Machine Learning: Comparing Linear Regression, LSTM, Random Forest and XGBoost

A machine learning project that evaluates four models for predicting the daily closing price of Bitcoin (BTC-USD), using a combination of on-chain blockchain metrics, macroeconomic indicators, and market sentiment proxies.

---

## Overview

| | |
|---|---|
| **Period** | April 2020 – April 2025 |
| **Frequency** | Daily |
| **Target** | BTC closing price (USD) |
| **Models** | Linear Regression, LSTM, Random Forest, XGBoost |

---

## Results

| Model | RMSE | MAE | R² |
|---|---|---|---|
| Linear Regression (baseline) | $2,161 | $1,591 | 0.981 |
| LSTM | $2,266 | $1,775 | 0.979 |
| Random Forest | $15,532 | $10,410 | 0.029 |
| XGBoost | $17,004 | $10,897 | -0.164 |

*Test set: April 2024 – April 2025*

**Key finding:** Linear Regression and LSTM achieve comparable performance (R² ~0.98), while tree-based models fail on the test set due to extrapolation failure, a limitation that becomes visible when the test period contains price levels not seen during training.

---

## Features

| Category | Features | Source |
|---|---|---|
| Crypto market | BTC Volume, USDT Volume | CoinGecko |
| On-chain | Hash Rate, MVRV Ratio | Blockchain.com |
| Macro & liquidity | S&P 500, Fed Balance Sheet (WALCL), US Dollar Index, Gold | Investing.com / FRED |
| Sentiment & risk | Fear & Greed Index, VIX | Alternative.me / Investing.com |

Features were selected based on their established relationship with Bitcoin price dynamics in the academic literature.

**Consistent finding across all models:** SP500, MVRV, and Fear & Greed Index are the top predictors.

---

## Notebook Structure

```
Section 1 · Setup & Configuration
Section 2 · Data Loading & Cleaning
Section 3 · Dataset Assembly & Validation
Section 4 · Exploratory Data Analysis
Section 5 · Modelling
    - Model 1 · Linear Regression (baseline)
    - Model 2 · LSTM
    - Model 3 · Random Forest
    - Model 4 · XGBoost
Section 6 · Model Comparison & Conclusions
```

---

## How to Run

**Option A - Google Colab (recommended)**
1. Upload `BTC_Price_Forecasting.ipynb` to [Google Colab](https://colab.research.google.com)
2. Upload all data files to the Colab session
3. Run all cells

**Option B - Local**

Clone the repository:
```
git clone https://github.com/username/btc-price-forecasting.git
```

Install dependencies:
```
pip install -r requirements.txt
```

Open the notebook:
```
jupyter notebook BTC_Price_Forecasting.ipynb
```

---

## Key Technical Decisions

**Data leakage prevention:** In order to avoid overly optimistic metrics, the MinMaxScaler for LSTM was fit exclusively on the training set and then applied to the test set.

**Extrapolation failure:** Tree-based models (Random Forest, XGBoost) cannot predict values outside the training range. Multiple hyperparameter configurations were tested.

---

## Limitations & Future Work

One of the most interesting findings of this project was the stark difference between regression and tree-based models on the test set. Random Forest and XGBoost failed not because of poor tuning (multiple configurations were tested) but because of a structural limitation: decision trees cannot extrapolate beyond price levels seen during training. This became particularly visible when BTC surged from around $70K to $106K in November 2024.

The test period also coincided with one of the strongest bull runs in BTC history, making it a challenging evaluation window. Walk-forward validation across multiple market regimes would provide more robust performance estimates.

A natural next step would be to reformulate the problem as a classification task, predicting market direction (up/down) over a 7 or 30-day horizon rather than exact price levels. This framing would be better suited to tree-based models and more meaningful from a practical standpoint.


