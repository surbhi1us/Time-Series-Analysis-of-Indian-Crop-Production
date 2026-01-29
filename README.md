# Time Series Analysis of Agricultural Crop Production in India

## 1. Project Overview
This project performs a **time series analysis and forecasting** of agricultural crop production in India using **state-wise annual data**.  
The objective is to understand historical production trends and generate **future forecasts** at:
- **State level**
- **Crop level**
- **All-India aggregate level**

Two forecasting approaches are compared:
- **Holt’s Linear Exponential Smoothing**
- **Auto ARIMA**

The analysis focuses on identifying:
- Trend behavior
- Stationarity
- Model suitability under limited data conditions
- Forecast reliability through model comparison

---

## 2. Dataset Description

### Source
- Excel file: `food production.xlsx`

### Structure
- **Time span:** 1980–2022 (42 years)
- **Frequency:** Yearly
- **Geographical coverage:** All Indian states and union territories  
  *(Telangana, Chhattisgarh, Jharkhand, Uttarakhand, J&K, and Ladakh appear only after state formation)*
- **Number of crops:** 9
  - Rice  
  - Wheat  
  - Coarse cereals  
  - Pulses  
  - Foodgrains  
  - Oilseeds  
  - Cotton  
  - Sugarcane  
  - Raw Jute & Mesta

### Units
- Primary unit: **1000 tonnes**
- Conversions applied:
  - Cotton: 170 kg per bale → tonnes
  - Jute: 180 kg per bale → tonnes

---

## 3. Data Cleaning & Preprocessing

### Fiscal to Calendar Year Conversion
- Fiscal years like `2022–23` were converted to `2022`
- This simplifies time indexing and enables compatibility with Python time series libraries

### Sorting & Indexing
- Data sorted by `Year` and `State`
- `Year` converted to `datetime` and set as index

### Reshaping
- **Wide → Long format** using `melt()`
- Final dataframe columns:
  - `Year`
  - `State`
  - `Crop`
  - `Production`

---

## 4. Exploratory Data Analysis (EDA)

### State-wise Trends
- Line plots for each state showing crop production over time
- Helps identify:
  - Growth patterns
  - Structural breaks
  - Volatility

### Crop-wise Comparison
- Pivot tables used to plot **all states for a single crop**
- Enables cross-state comparison

---

## 5. Challenges in Forecasting

- Data is **yearly**
- Effective usable points per series ≈ **8–15 years**
- No reliable seasonality
- High risk of overfitting with complex models

---

## 6. Stationarity Testing (KPSS)

- KPSS test applied to each `(State, Crop)` time series

### Results
- Total possible series: `29 × 9 = 261`
- Tested series: **240**
  - **69 stationary**
  - **171 non-stationary**

Some series were excluded due to insufficient or missing data.

---

## 7. Forecasting Models

### 7.1 Holt’s Linear Exponential Smoothing
- Captures **level and linear trend**
- No seasonality
- Data scaled using `StandardScaler`
- Works well for smooth and stable trends

**Limitation:**  
May overestimate trends when data is noisy.

---

### 7.2 Auto ARIMA
- Automatically selects `(p, d, q)`
- Differencing based on stationarity:
  - `d = 0` for stationary
  - `d = 1` for non-stationary

Advantages:
- Handles autocorrelation
- More robust to noise
- Prevents unrealistic extrapolation

---

## 8. Forecast Confidence Strategy

Due to limited data:
- Advanced models (LSTM, SARIMA) were avoided
- Confidence assessed using:
  - Model agreement
  - Trend consistency
  - Visual inspection
  - Domain judgment


---

## 9. Model Comparison Examples

### Telangana – Rice
- Strong linear growth
- Holt and ARIMA give similar forecasts
- High confidence

### Rajasthan – Sugarcane
- Noisy and declining trend
- Holt predicts sharp decline
- ARIMA predicts stabilization
- ARIMA preferred

### Maharashtra – Sugarcane
- Strong upward trend
- Both models predict growth
- ARIMA slightly steeper

---

## 10. All-India Analysis

### Observations
- Major drops around:
  - 2002–04
  - 2015–16

### Crop-wise Trends
- **Sugarcane:** Rapid growth post-2015
- **Foodgrains:** Stable growth
- **Rice & Wheat:** Consistent increase
- **Oilseeds, Pulses, Cotton, Jute:** Mostly flat
- **Coarse cereals:** Mild growth after 2000

---

## 11. Why SARIMA Was Not Used
- Yearly data lacks clear seasonality
- SARIMA requires repeating seasonal patterns
- Visual inspection confirmed no seasonality

---

## 12. Final Forecasting Approach

- Forecasts generated using:
  - Holt’s Linear
  - Auto ARIMA
- Judgmental ensemble approach:
  - Compare both models
  - Prefer ARIMA for noisy series
  - Trust agreement when both align

---

## 13. Key Takeaways

- Limited data favors simpler models
- Holt works best for clean trends
- ARIMA is safer for unstable series
- Model agreement increases confidence
- Visualization is crucial in time series analysis
