# Household Carbon Footprint Prediction — Python Regression Portfolio Project

## Problem Statement

Predict a household's annual carbon emission from its lifestyle and consumption habits — transport, diet, energy use, waste, and shopping patterns — using linear regression, to understand which everyday choices contribute most to a household's carbon footprint.

## About the Data

This dataset contains 10,000 household records with 20 features covering transport habits (vehicle type, monthly distance, air travel frequency), diet and lifestyle (diet type, shower frequency, social activity), household energy use (heating source, energy efficiency), waste and recycling habits, and spending patterns (grocery bill, new clothes purchased), with the target variable being the household's total annual CarbonEmission.

## Tools Used

- Python — pandas and numpy for data cleaning and feature engineering, scikit-learn (`ColumnTransformer`, `Pipeline`, `LinearRegression`) for modeling, matplotlib and seaborn for visualization

## Analysis Performed

1. **Data cleaning** — checked for duplicates and missing values, filled missing `Vehicle_Type` values with "None" (since it's genuinely not applicable for households without a vehicle).
2. **Feature engineering** — converted multi-value text columns (Recycling, Cooking_With) into binary flags, mapped ordinal categories (air travel frequency, waste bag size) into numeric scores, and built two composite features: a Total Screen Time measure and a normalized Lifestyle Spend Index.
3. **Exploratory analysis** — examined the distribution and skew of the target variable, compared average emissions across key categories (transport, heating source, diet, body type), and checked correlations between numeric predictors and CarbonEmission.
4. **Modeling** — built a preprocessing pipeline (imputation, scaling, one-hot encoding) feeding into a Linear Regression model, with imputation/encoding fit only on the training split to avoid data leakage.
5. **Model evaluation** — assessed the model with R², Adjusted R², RMSE, and MAE on a held-out test set.
6. **Residual diagnostics** — checked the linear regression assumptions by examining residuals vs. fitted values (linearity/homoscedasticity) and residual distribution (normality).
7. **Coefficient interpretation** — identified the biggest positive and negative drivers of carbon emission from the model's coefficients, and demonstrated the dummy variable trap by comparing a version with and without `drop='first'` in one-hot encoding.

## Key Insights

- The model explains **91.7% of the variance in household carbon emissions** (R² = 0.917, Adjusted R² = 0.915), with a mean absolute error of about 211 units.
- **Residual diagnostics confirm the model's assumptions largely hold**: residuals are roughly normal and show no curve (linearity holds), but do show mild heteroscedasticity — prediction error grows for higher-emission households, meaning estimates are less precise at the high end. A log-transform of the target would be the natural next step to address this.
- **Biggest emission drivers**: vehicle distance driven, air travel frequency, and heating source — coal-heated, high-mileage, and frequent-flying households emit the most.
- **Biggest emission reducers**: electric/hybrid vehicles and cleaner heating sources (electricity, natural gas, wood).
- **A real modeling pitfall was caught and fixed**: the first version of the model didn't use `drop='first'` in one-hot encoding, creating a dummy variable trap — visible as mirrored, cancelling-out coefficients (e.g., `Sex_male: +169.86` and `Sex_female: -169.86`). A corrected version with `drop='first'` fixes this.

## How to Run

1. Make sure `CARBON FOOTPRINT PREDICTION.ipynb` and `Carbon Emission - LMS.csv` are in the same folder.
2. Open the notebook in Jupyter from that folder and run all cells in order.

## Files in This Repository

- `CARBON FOOTPRINT PREDICTION.ipynb` — full analysis notebook: cleaning, feature engineering, modeling, evaluation, and interpretation
- `Carbon Emission - LMS.csv` — the raw dataset
- `README.md` — this file
