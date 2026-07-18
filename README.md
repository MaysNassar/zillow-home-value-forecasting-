# **Time Series Analysis & Tableau — Belt Exam**

This repo contains my submission for the Time Series Analysis and Tableau belt exam. It covers preparing Zillow home value data as a time series, forecasting Oregon home prices with ARIMA/SARIMA, and building a Tableau story from the processed data.

<img width="824" height="581" alt="image" src="https://github.com/user-attachments/assets/4b1a9fce-19d7-4c0f-9e32-c3a8ec0afca1" />

## Data

Source: Zillow Home Value Index (ZHVI), by ZIP code.

The raw file (zillow_home_values-zipcode.csv) is wide-form, with one column per month from January 2000 through November 2022. It was melted into a long-form time series with Date and Home Value columns, then indexed by date.

Data/data-for-tableau.csv is a filtered copy of that processed data:


- States: CA, WA, OR, AZ, NV
- Years: 2010–2020

## Part 1 — Time Series Prep & Exploration


- Melted the wide-form CSV into a long-form time series
- Converted date strings (encoded as DDMMYYYY) to proper datetime values
- Built the filtered Tableau-ready dataset described above
- Resampled home values to yearly frequency, grouped by state, and plotted each state as its own line to compare growth across the five states


## Part 2 — Forecasting Oregon Home Prices

Goal: forecast average Oregon home prices 12 months into the future, using monthly data from 2000–2018.

**Steps:**
- Filtered and aggregated the data to a monthly mean home value series for Oregon
- Checked for and confirmed no null values
- Decomposed the series (additive) to check for seasonality — found a seasonal amplitude of under 1% of the trend's overall range, indicating minimal seasonal effect
Determined differencing order: d=2 (confirmed via ADF test and variance comparison across differencing levels), D=0 (no seasonal differencing needed)
- Used ACF/PACF plots on the differenced series to identify candidate orders
- Split the data into training (through 2017) and a 12-month test set (2018)
- Fit and compared three candidate models:

------------------------------------------------------------
Manual, no seasonal, ARIMA(0,2,1)(0,0,0)
------------------------------------------------------------
- MAE = 381.780
- MSE = 199,058.177
- RMSE = 446.159
- R^2 = 0.991
- MAPE = 0.12%

------------------------------------------------------------
Manual, with seasonal MA, ARIMA(0,2,1)(0,0,1)
------------------------------------------------------------
- MAE = 376.479
- MSE = 195,350.505
- RMSE = 441.985
- R^2 = 0.992
- MAPE = 0.12%

------------------------------------------------------------
auto_arima, ARIMA(0,2,0)(0,0,0)
------------------------------------------------------------
- MAE = 384.272
- MSE = 204,449.052
- RMSE = 452.160
- R^2 = 0.991
- MAPE = 0.12%
---------------


- Selected ARIMA(0,2,1)(0,0,0)₁₂ as the final model — it's directly grounded in the manual ACF/PACF diagnostics, and all three models agreed that a seasonal component wasn't worth the added complexity.
- Refit the final model on the entire dataset and forecast 12 months beyond it (January–December 2019)
  
-------

#### **Result:** Oregon home values are forecast to rise from about $320,540 in January 2019 to about $337,253 by December 2019 — a net increase of $16,665, or 5.20% over the year. This continues the recovery trend visible since Oregon home prices bottomed out around 2012.


-------



## Tools & Libraries

![Python](https://img.shields.io/badge/Python-3.12-blue)
[![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=fff)](#)![statsmodels](https://img.shields.io/badge/statsmodels-0.14-lightgrey)
![pmdarima](https://img.shields.io/badge/pmdarima-2.1-lightgrey)
[![Tableau](https://custom-icon-badges.demolab.com/badge/Tableau-0176D3?logo=tableau&logoColor=fff)](#)
