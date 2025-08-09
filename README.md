# Volatility Forecasting with GARCH and EWMA Models

## Overview
This project implements **volatility forecasting** using two popular models in quantitative finance:

- **GARCH(1,1)** – Generalized Autoregressive Conditional Heteroskedasticity
- **EWMA** – Exponentially Weighted Moving Average

It applies these models to historical price data for the **NIFTY50 Equal Weight index** to predict future volatility, which is crucial for **risk management**, **option pricing**, and **trading strategies**.

The workflow includes:
- Data cleaning & preprocessing
- Log returns calculation
- Model fitting & out-of-sample forecasts
- Evaluation using error metrics
- Visualization & interpretation

---

## Motivation
**Volatility** reflects the magnitude of price fluctuations and is a key measure of market risk.  
Predicting volatility helps in:
- Setting risk limits  
- Allocating capital efficiently  
- Pricing derivatives  
- Designing market-neutral trading strategies  

**GARCH models** capture **volatility clustering** and persistence, while **EWMA** offers a simpler smoothing approach. This project compares both to see which works better on Indian market data.

---

## Data Description
- **Source:** Historical daily prices for *NIFTY50 Equal Weight Index* (CSV file).
- **Key Column:** `Price` – daily closing price.
- **Cleaning:**  
  - Removed thousands separators (commas) and converted to numeric.  
  - Dropped any rows with missing values.
- **Returns:**  
  Daily **log returns** were computed as:

$$
R_t = \ln\left(\frac{P_t}{P_{t-1}}\right)
$$

- Data split:  
  - **80%** for training (model fitting)  
  - **20%** for testing (out-of-sample evaluation)

---

## Methodology

### 1. GARCH(1,1) Model
The GARCH(1,1) model estimates time-varying conditional variance:

$$
\sigma_t^2 = \omega + \alpha \epsilon_{t-1}^2 + \beta \sigma_{t-1}^2
$$

- Captures **volatility clustering** and persistence.
- Fitted on training data and used for **rolling one-step-ahead forecasts**.

### 2. EWMA Model
Volatility forecasted via exponentially smoothing squared returns:

$$
\sigma_t^2 = \lambda \cdot \sigma_{t-1}^2 + (1 - \lambda) \cdot \epsilon_{t-1}^2
$$

- Decay factor $$(\lambda)$$ = **0.94**
- Simpler, less reactive to sudden spikes.

### 3. Evaluation Metrics
- **MSE** (Mean Squared Error)
- **MAE** (Mean Absolute Error)

Lower values indicate better forecasts.

---

## Results

| Model      | MSE        | MAE        |
|------------|------------|------------|
| GARCH(1,1) | 0.000015   | 0.002904   |
| EWMA       | 0.000018   | 0.003450   |

**Observations:**
- GARCH(1,1) produced **slightly lower errors** than EWMA.
- Plots show GARCH responds faster to spikes in volatility, while EWMA is smoother but lags.

---

## Inferences & Conclusion
- **GARCH(1,1)** better tracks realized volatility, making it suitable for **short-term trading** and **dynamic risk management**.
- **EWMA** is more stable and could be preferred for **long-term volatility estimates**.
- Proper data cleaning (removing commas, handling NaNs) is critical before model fitting.
- This framework can be extended to:
  - Other indices or assets  
  - Alternative GARCH variants (EGARCH, TGARCH)  
  - More complex ML-based volatility models

---

## How to Run

### 1. Install dependencies
pip install numpy pandas matplotlib arch scikit-learn

### 2. Prepare the data
- Place your CSV file (e.g., `NIFTY50 Equal Weight Historical Data.csv`) in the `data/` folder.
- Ensure the `Price` column contains daily closing prices.

### 3. Run the script
python volatility_forecasting.py

The script will:
1. Load and clean the data.
2. Compute log returns.
3. Fit GARCH(1,1) and EWMA models.
4. Perform forecasts.
5. Save/Display:
   - Evaluation metrics (MSE, MAE)
   - Forecast vs realized volatility plot

---

## Future Work
- Add **more advanced volatility models** (EGARCH, TGARCH, Heston SV).
- Perform **multi-step-ahead forecasts**.
- Test on other markets and assets.
- Integrate with **trading / risk management strategies**.

