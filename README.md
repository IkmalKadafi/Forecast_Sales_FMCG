# FMCG Sales Forecasting using ARIMA

## 📌 Project Overview
This project presents a comprehensive time-series analysis and forecasting model for a Fast-Moving Consumer Goods (FMCG) company, specializing in food products and frozen foods. Using historical transaction data spanning daily sales in 2024, the objective is to uncover sales patterns, analyze product performance across different regional markets, and construct a robust predictive model (ARIMA) to forecast future sales quantities.

> **Note on Data Privacy:** The dataset (`anonymized_data.csv`) and notebook (`Forecast_Arima_Anonymized.ipynb`) in this repository have been anonymized. Sensitive information such as Customer Names, Salesperson Names, PICs, and specific Company Brands have been replaced with placeholders or generalized terms to protect business privacy, while fully retaining the statistical distribution and data utility for modeling.

---

## 💼 Business Problem (Business Analyst POV)

In the highly competitive FMCG and Frozen Food industry, balancing supply chain operations with fluctuating market demand is the ultimate challenge. The core business problems addressed in this project are:

1. **Demand Uncertainty & Supply Chain Bottlenecks:** 
   Fluctuations in daily sales lead to either **stockouts** (missed revenue and customer dissatisfaction) or **overstocking** (increased holding costs and potential food waste).
2. **Resource Allocation:** 
   Without data, it is difficult to determine which provinces, cities, and product lines drive the most impactful revenue, making targeted marketing and sales team deployment inefficient.
3. **Reactive vs. Proactive Inventory Management:** 
   Relying purely on historical moving averages or gut feeling limits the company's ability to prepare for sudden surges in B2B/B2C demand.

**Business Objective:**  
To transform sales data into actionable business intelligence by identifying core markets and creating a predictive engine (ARIMA) that allows the business to anticipate demand volume days in advance.

---

## 🔬 Exploratory Data Analysis & Insights (Data Analyst POV)

Before forecasting, a deep dive into the transaction data generated several strategic insights:

### 1. Market Penetration (Geospatial Analysis)
- **Dominant Provinces:** `JAWA TIMUR`, `JAWA BARAT`, and `DKI JAKARTA` generated the absolute majority of revenue. This suggests the company’s distribution network is highly centralized in Java.
- **Untapped Opportunities:** Regions outside Java (like Sulawesi and Bali) show growing traction but require different supply chain strategies (e.g., localized cold storage) to offset potential logistic bottlenecks.
- **Recommendation:** Marketing and promotional budgets should be aggressively funneled into sustaining dominance in Java, while secondary campaigns can be tested in Bali and Sulawesi.

### 2. Product Portfolio Performance (The Pareto Principle)
- Based on the sales quantity and total revenue (Omzet), a small subset of "Main Products" (e.g., *Specific Tortilla* or *Meat* categories) serves as the "cash cow" of the business.
- **Recommendation:** Ensure these top-performing SKUs *never* experience a stockout. Meanwhile, underperforming SKUs can be bundled with the top-sellers to boost inventory turnover.

### 3. Time Series Behavior
- **Seasonality & Spikes:** Daily aggregated sales exhibit volatile variations, heavily influenced by sudden "bulk orders" from B2B partners (e.g., wholesalers, hotels). 
- **Outlier Detection:** Applying Statistical Z-Scores helped identify these extreme baseline anomalies. Smoothing these outliers was a critical step so the ARIMA model wouldn't "learn" a rare anomaly as a standard recurring pattern.

---

## ⚙️ Solution Architecture & Modeling (Data Scientist POV)

To predict future demand, an end-to-end data pipeline was implemented focusing on statistical Time Series forecasting:

### 1. Data Preprocessing
- Aggregated thousands of daily transactional orders into a continuous daily time-series format (`Date` vs `Total Quantity`).
- Handled missing timeline gaps with linear interpolation.

### 2. Stationarity & Statistical Testing
- Performed **Augmented Dickey-Fuller (ADF)** and **KPSS** tests to evaluate time-series stationarity. 
- Differencing ($d$) was applied to stabilize the mean mathematically before feeding the data to the ARIMA algorithm.

### 3. ARIMA (Auto-Regressive Integrated Moving Average)
- Explored different combinations of $p$ (Lag order), $d$ (Degree of differencing), and $q$ (Order of moving average).
- **Model Evaluation:** The chosen model was optimized by minimizing the **Root Mean Squared Error (RMSE)**.
- **Forecasting:** The model successfully captured the baseline trend of daily sales, enabling the company to predict baseline restocking needs for the upcoming weeks.

---

## 📊 Business Impact & Results
With carefully tuned ARIMA predictions:
1. **Cost Efficiency:** Procurement teams can now place raw material orders based on statistically backed forecasts rather than intuition, reducing over-purchasing.
2. **Operational Resilience:** Production floors can pre-plan staffing and machinery utilization based on expected demand curves.
3. **Data-Driven Culture:** The notebook translates raw transactional noise into a clean, predictable strategic asset for the company.

---

## 🛠️ Tech Stack
- **Language:** Python 3
- **Data Manipulation:** `pandas`, `numpy`
- **Data Visualization:** `matplotlib`, `seaborn`
- **Statistical Analysis & Forecasting:** `scipy`, `statsmodels.tsa.arima` (ARIMA), `scikit-learn`

---

## 🚀 How to Run the Project
1. Clone this repository to your local machine:
   ```bash
   git clone https://github.com/yourusername/forecast-sales-fmcg.git
   ```
2. Navigate to the project directory:
   ```bash
   cd forecast-sales-fmcg
   ```
3. Install the required dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn
   ```
4. Open and run the Jupyter Notebook:
   - Launch Jupyter Notebook or open it via VS Code.
   - Run **`Forecast_Arima_Anonymized.ipynb`**. 
   - *Note: Don't forget that the notebook relies on `anonymized_data.csv` which is included alongside it in this repository.*