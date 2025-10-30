#  Energy Demand Flexibility Analysis

This project analyzes and models the Demand Flexibility Service (DFS) market activity managed by NESO. The goal is to predict when DFS events will be called and estimate the accepted bid prices using publicly available electricity system data.

---

## Project Overview

Between December 1, 2024 – March 31, 2025, NESO called DFS service requirements in 302 half-hours out of 5808 total, representing ~5% of the period.  
During this time:
- 3574 bids were received  
- 2051 were rejected  
- 1523 were accepted  
- Infinis Limited provided half of all accepted bids, while Axle Energy contributed 20 accepted bids

This project builds two predictive models:
1. Classification model – Predicts *whether* a DFS event will be called  
2. Regression model – Predicts the *average accepted bid price* (£/MWh)

Both models are implemented using Random Forests.

---

## Data Sources

All data were retrieved from the Elexon API and NESO public datasets.  
The key features used include:

- Loss of Load Probability (LoLP) – 12 hours before each settlement period  
- De-rated Margin (DRM) – 12 hours before each settlement period  
- Day-ahead Aggregate Generation Forecast  
- Day-ahead Demand Forecast (National & Transmission) 

For each half-hour period, features were merged with NESO data on accepted DFS bids and average accepted bid prices.

---

## Methodology

### 1. Data Collection
Custom Elexon API functions were developed to fetch hourly and half-hourly data between Dec 2024 – Mar 2025, handling request limits by looping through smaller intervals.

### 2. Feature Engineering
- Merged all datasets into a single DataFrame  
- Added derived features such as Generation-Demand difference  
- Filtered to include only 16:00–19:00 half-hours (when DFS events occur)

### 3. Modeling
#### Classification Model
- Algorithm: Random Forest Classifier  
- Goal: Predict whether a DFS event occurs in each half-hour  
- Balanced dataset by restricting to 16:00–19:00 periods  
- Achieved ROC AUC = 0.87 with strong event detection performance

#### Regression Model
- Algorithm: Random Forest Regressor  
- Goal: Predict average accepted bid price (£/MWh)  
- Achieved R² = 0.72 and MAE = £37/MWh

---

## Results Summary

| Model | Task | Metric | Result |
|--------|------|---------|---------|
| Classification | Predict DFS event occurrence | ROC AUC | 0.87 |
| Regression | Predict accepted bid price | R² | 0.72 |
| Regression | Mean Absolute Error | £37/MWh |

---

## Discussion

Despite time and data constraints (≈3–5 hours total), the models demonstrate that:
- Even simple models can capture meaningful DFS event dynamics  
- Limited but timely features (available by morning) can predict day-ahead DFS behavior  
- Further accuracy could be achieved by incorporating:
  - Additional market indicators (e.g., interconnector prices, imbalance prices)  
  - Advanced feature engineering  
  - Neural networks or gradient boosting methods
