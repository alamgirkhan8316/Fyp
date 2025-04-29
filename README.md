## Air Pollution PM2.5 Forecasting for India

This repository contains a single Jupyter notebook, Air_Pollution_(1).ipynb, that walks through the complete workflow of loading raw air‑quality data for India, cleaning it, exploring it visually, and training multiple forecasting models (SARIMAX, LSTM, XGBoost) that are finally combined into a weighted ensemble. The project is intended as a reproducible template for time‑series pollution forecasting with open data.

## What does the notebook do?

Import libraries – pandas, numpy, matplotlib, seaborn, statsmodels, scikit‑learn, tensorflow/keras, xgboost.

Load dataset – reads who.xlsx from the WHO Global Ambient Air Quality Database.

Filter for India – keeps only rows where WHO Country Name = India.

Clean & prepare data

Handle missing values and unrealistic outliers (drop top & bottom 1 %).

Convert the Measurement Year column to a proper datetime index.

Create calendar features (year, month, quarter) and domain features (Monsoon season indicator, GDP growth).

Exploratory Data Analysis (EDA)

Trend lines for PM2.5, PM10, NO₂.

Correlation heat‑maps, top‑10 hotspot cities.

# Model building

SARIMAX with exogenous variables.

LSTM neural network on sliding windows.

XGBoost regressor on lagged tabular features.

Ensembling & evaluation

Compute MAE, RMSE, R² on a chronologically held‑out 20 % test set (≈ 2020‑2024).

Derive optimal weights and blend the three models.

Diagnostics & visualisation – residual plots, ACF/PACF, and a forecast‑vs‑actual line chart.

What are the outputs?

Output

Where it appears

Purpose

df_clean** DataFrame**

In‑memory (can be exported to data/df_clean.csv)

Fully cleaned, feature‑enriched monthly pollution data for India (≈ 2 k rows × 9 columns).

Plots (figures/*.png)

Automatically saved by Matplotlib

Trend lines, correlation heat‑map, residual diagnostics, final forecast chart.

Model artefacts (models/)

Saved with joblib/model.save()

sarimax.pkl, lstm.h5, xgb.json – reusable for inference without retraining.

predictions.csv

Root folder

Ensemble point forecasts for the 20 % test horizon plus historical actuals.

Console metrics

Notebook output

MAE, RMSE, R² for each model and for the ensemble (ensemble ≈ MAE 5.7 µg/m³, R² 0.91).

Quick start

# Clone the repository
git clone https://github.com/your‑org/air‑pollution‑india.git
cd air‑pollution‑india

# Create Python 3.11 virtual environment
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt  # ~ 350 MB incl. TensorFlow

# Launch JupyterLab and run the notebook
jupyter lab

Expected runtime on a modern laptop: 3‑5 min for EDA, ~10 min for model training (CPU).

# Requirements (condensed)

pandas numpy matplotlib seaborn scikit‑learn statsmodels xgboost tensorflow openpyxl

A full, pinned list is provided in requirements.txt.

Re‑using the code for another country

Change the country filter in Step 4 from "India" to your target country (must match WHO naming).

Optionally add country‑specific exogenous variables (e.g. humidity, traffic counts).

Re‑run all cells – the hyper‑parameter searches will adapt automatically.

# Limitations & future work

Rural pollution is under‑represented because WHO monitoring sites are concentrated in major cities.

GDP growth is an annual figure interpolated to monthly, which may add noise.

The LSTM model is data‑hungry; with only ≈ 2 k timesteps there is risk of overfitting.

Incorporating additional drivers (meteorology, satellite AOD) could further improve accuracy.
