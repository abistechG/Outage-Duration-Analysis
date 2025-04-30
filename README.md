# Predicting Power Outage Duration from Temperature Anomalies

**Name**: Abyesolome Assefa  
**Email**: [abistech@umich.edu]  
**Website**: [https://abistechg.github.io/Outage-Duration-Analysis]

---

# Introduction

The dataset **Masterdata** chronicles 1,534 major power‐outage events in the continental U.S. from **January 2000 to July 2016**. It includes:

- **Timing**: Year, month, start/restoration times  
- **Location**: State, NERC region, climate region  
- **Climatic context**: Temperature anomalies and climate category  
- **Outage details**: Duration (minutes), cause, customers affected  
- **Socioeconomic**: Energy sales, prices, and population metrics  

**Project Question**:  
How does the magnitude of temperature anomalies correlate with the duration of major power‐outage events in the U.S.?

**Why it matters**:  
As climate variability increases, showing a link between temperature and outage length can help utilities prepare for more extreme conditions.

---

# Data Cleaning and Exploratory Data Analysis

We cleaned the data by:
- Skipping metadata rows, dropping nulls from key columns
- Converting timestamps to datetime
- Creating new features: `outage_length_hours`, `start_hour`, and `start_weekday`

## Univariate Plot (OUTAGE.DURATION):
<iframe src="assets/univariate_plot.html" width="800" height="600" frameborder="0"></iframe>

## Bivariate Plot (Anomaly vs Duration):
<iframe src="assets/bivariate_plot.html" width="800" height="600" frameborder="0"></iframe>

## Groupby Table (Avg Duration by Climate):
<iframe src="assets/groupby_table.html" width="800" height="400" frameborder="0"></iframe>

---

# Framing a Prediction Problem

We framed this as a **regression task**:  
> Predict `OUTAGE.DURATION` (in minutes) using temperature anomalies, climate categories, and time-based features.

Our main evaluation metric is **MAE** (mean absolute error), which penalizes large misses proportionally.

---

# Baseline Model

- Features: `ANOMALY.LEVEL`, `CLIMATE.CATEGORY`
- Model: Linear Regression with scaling + one-hot encoding
- Performance:
  - MAE: 3,234.28 minutes
  - RMSE: 5.7e+07 minutes²
  - R²: –0.003

This basic model established a performance baseline for later comparison.

---

# Final Model

We engineered cyclical time features:

- `hour_sin`, `hour_cos` (for hour-of-day)
- `month_sin`, `month_cos` (for seasonal trends)

Final model: **Random Forest Regressor** with `GridSearchCV`.  
Features included anomaly level, time cycles, climate category, and NERC region.

## Best hyperparameters:
- `n_estimators`: 200
- `max_depth`: 10
- `min_samples_split`: 5

## Final Performance:
- MAE: 2,766.95 minutes (↓ 467 from baseline)
- RMSE: 54,350,967.27 minutes²
- R²: 0.114

The model improved prediction accuracy by ~7.8 hours. Although R² remains low due to outliers, this model captures more signal than a simple linear baseline. Further improvements could involve transforming `OUTAGE.DURATION` or removing extreme events.

---

