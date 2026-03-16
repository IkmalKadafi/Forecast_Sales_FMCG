# Interpretations and Insights from ARIMA Forecast

This document summarizes the analysis, interpretations, and insights extracted from `Forecast_Arima_Anonymized.ipynb`.

## 1. Exploratory Data Analysis (EDA)

### **Revenue by Province and City**
![Top Provinces](top_provinces.png)
- **Top Provinces:** **Jawa Timur** is the absolute leader in sales revenue (Omzet), generating approximately **Rp 19.3 Billion** with over 748,000 units sold. It is followed by **Jawa Barat** (~Rp 7.5 Billion), **DKI Jakarta** (~Rp 7.2 Billion), and **Bali** (~Rp 5.1 Billion).

![Top Cities](top_cities.png)
![Bottom Cities](bottom_cities.png)
- **City-Level Distribution:** The distribution of revenue across cities indicates high concentration in several primary hubs (e.g., Bali, Bandar Lampung). The bottom 10 cities contrast sharply by showing very low traction, highlighting potential areas for localized marketing or distribution optimization over time.

### **Revenue by Main Product**
![Product Distribution](product_distribution.png)
- **Dominant Product:** **Tortilla** is overwhelmingly the top-selling category, generating **Rp 24.7 Billion** (over 1.18 million units sold). 
- **Secondary Products:** **Daging Kebab External** (~Rp 6.4 Billion) and **Kebab Frozen** (~Rp 4.6 Billion) make up the next largest revenue streams.
- **Insight:** The business heavily relies on its core product (Tortilla). Ensuring supply chain stability for Tortilla is critical, as it constitutes the vast majority of the revenue distribution pie.

---

## 2. Time-Series Data Preparation

- **Aggregation:** The dataset was grouped continuously by `Date` (daily frequency) mapping to `Total Harga` (Daily Revenue). Missing daily entries were filled using linear interpolation to avoid discontinuity.

![Weekly Revenue Trend](weekly_revenue_trend.png)
- **Handling Outliers:** Revenue spikes that exceeded a **Z-Score of 3** (e.g., massive sales on June 29, 2024, or December 23, 2024) were filtered out, replaced with `NaN`, and smoothed over using linear interpolation. This prevents extreme, potentially anomalous bulk orders from skewing the forecasting variance.

---

## 3. Normality & Stationarity Checks

- **Normality Check:** The daily revenue data initially failed the Shapiro-Wilk test (p-value = 0.001 < 0.05) and showed a right-skewed distribution.
![Revenue Distribution KDE](revenue_distribution_kde.png)
- **Box-Cox Transformation:** To stabilize the variance and normalize the data, a Box-Cox transformation was applied (Optimal $\lambda \approx 0.672$). Post-transformation, the data passed the Shapiro-Wilk normality test (p-value = 0.851).
- **Stationarity:** 
  - **ADF Test (Augmented Dickey-Fuller):** P-value of 0.000 (< 0.05), confirming the transformed series does not have a unit root and is strictly stationary.
  - **KPSS Test:** P-value of 0.048 (< 0.05) indicates slight non-stationarity around a deterministic trend.
  
  ![ACF Plot](acf_plot.png)
  ![PACF Plot](pacf_plot.png)
  - Given the strong stationarity from the ADF test, the model proceeded without heavy differencing (`d=0`).

---

## 4. ARIMA Modeling & Evaluation

The transformed daily revenue series was split temporally (80% Train, 20% Test).

### **Hyperparameter Tuning**
A grid search was evaluated across $p \in [0, 3]$, $d \in [0]$, and $q \in [0, 3]$ to find the hyperparameters that minimized RMSE and MSE.
- **Best Model:** **ARIMA(6, 0, 5)** yielded the lowest error metrics.
  - Lowest RMSE: ~145,671 (in Box-Cox scale)
  - Lowest MSE: ~21,220,135,141 (in Box-Cox scale)
- A baseline standard model **ARIMA(1, 0, 1)** was briefly visualized and printed, but the grid search verified that taking into account up to 6 previous lags (AR=6) and 5 moving average errors (MA=5) captured daily fluctuations slightly better.

### **Forecast Generation**
![ARIMA Forecast](arima_forecast.png)
- **Insight:** By leveraging a `d=0` parameter and normalizing via Box-Cox, the ARIMA model provides a stabilized mean-reverting forecast. While it successfully establishes the baseline expectation for daily revenue, it naturally smooths out erratic daily peaks, acting as a conservative short-term benchmark rather than a highly volatile predictor.
