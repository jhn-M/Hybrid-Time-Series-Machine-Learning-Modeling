# Inflation Rate Forecasting: ARIMA-Random Forest Hybrid Model

This project aims to forecast Philippine inflation rates using a hybrid approach. By combining the linear modeling capabilities of **ARIMA** with the non-linear strengths of **Random Forest**, the model captures complex patterns in the Bangko Sentral ng Pilipinas (BSP) dataset.

## 📊 Model Performance
To visualize the results, we compare the standalone ARIMA predictions with the Hybrid ARIMA-RF model.

<p align="center">
  <img src="https://github.com/user-attachments/assets/59e9d8be-232b-400b-b97f-3a2af8848bf3" width="100%" alt="forecast_plot" />
</p>

### Evaluation Metrics
The Hybrid ARIMA-RF model consistently outperformed the standalone ARIMA model, showing a measurable reduction in error by capturing the non-linear residuals.

| Metric | Standalone ARIMA | Hybrid ARIMA-RF | Improvement |
| :--- | :---: | :---: | :---: |
| **MSE** | 3.1305 | **2.9073** | ~7.13% |
| **RMSE** | 1.7693 | **1.7051** | ~3.63% |
| **MAE** | 1.6458 | **1.5856** | ~3.66% |

---

## 🚀 Project Objectives
- **Data Preparation**: Import BSP data and convert to time-series format with a monthly frequency (`MS`).
- **Statistical Analysis**: Perform Augmented Dickey-Fuller (ADF) testing to ensure stationarity.
- **Modeling**: 
    - Fit **ARIMA/ARIMA** models and validate residuals (White Noise check).
    - Train a **Random Forest Regressor** on the ARIMA residuals to capture non-linear patterns.
- **Evaluation**: Compare hybrid results against standalone ARIMA using MSE, RMSE, and MAE.

---

## 🛠️ Tech Stack
- **Language**: Python
- **Libraries**: 
    - `pandas`, `numpy` (Data Manipulation)
    - `statsmodels`, `pmdarima` (Time Series Modeling)
    - `scikit-learn` (Machine Learning)
    - `matplotlib`, `seaborn` (Visualization)

## 📈 Methodology
1. **ARIMA**: Handles the seasonal and linear components of the inflation data.
2. **Residual Extraction**: The errors (residuals) from the ARIMA model are extracted as the target variable for the next stage.
3. **Random Forest**: An RF model is trained on these residuals to predict the "error" or gap left by the linear model.
4. **Final Forecast**: The ARIMA forecast and the RF residual prediction are summed to create the final hybrid forecast.

## 📁 Dataset
The analysis utilizes official inflation data from the Bangko Sentral ng Pilipinas (BSP), covering monthly rates processed for time-series forecasting.
