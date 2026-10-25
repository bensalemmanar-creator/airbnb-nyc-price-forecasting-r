# Forecasting Airbnb Prices in New York City (R)

Time-series forecasting of monthly median Airbnb listing prices in NYC, comparing ARIMA, SARIMA, ETS and Prophet.

**Course:** ITM508, Applied Time Series for Forecasting Analysis with R (AY 2025/26), South Mediterranean University

## Objective
Build a monthly price series from NYC Airbnb listings and find which model forecasts it most accurately on held-out data. Better price forecasts help hosts set prices, help travelers plan budgets and support platform strategy.

## Data
- [New York City Airbnb Open Data (2019)](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data) on Kaggle: about 48,000 listings (`AB_NYC_2019.csv`). Download it and place it next to the `.Rmd` file.

## Method
1. **Cleaning:** removed 10,052 listings with no review date, and listings with price = 0 or above $1,000. Mean price fell from $152.7 to $135.9 and the maximum from $10,000 to $1,000.
2. **Aggregation:** monthly median price by last-review month (Jan 2015 to Jun 2019, months with at least 10 listings). Remaining outliers handled with `tsclean()`.
3. **Stationarity:** the original series gave conflicting tests (ADF p = 0.038, KPSS p = 0.042). After a log transform and first differencing, both pass (ADF p = 0.01, KPSS p = 0.1).
4. **Split:** train Jan 2015 to Dec 2017 (36 months), validation Jan to Jun 2018 (6 months), test Jul 2018 to Jun 2019 (12 months).
5. **Models:** ARIMA(0,1,1), SARIMA(0,0,0)(1,1,0)[12], ETS(M,N,N) and Prophet.

## Results (test period, Jul 2018 to Jun 2019)

| Model | RMSE | MAE | MAPE |
|---|---|---|---|
| ARIMA (0,1,1) | 4.92 | 4.21 | 4.06% |
| SARIMA (0,0,0)(1,1,0)[12] | 14.76 | 12.68 | 12.33% |
| **ETS (M,N,N)** | **4.84** | **4.13** | **3.97%** |
| Prophet | 8.16 | 7.10 | 6.91% |

![Model comparison] and ![Stationarity]: 
[Airbnb-Manar-final.pdf](https://github.com/user-attachments/files/33934851/Airbnb-Manar-final.pdf)

**ETS is the best model**, with forecasts on average within about 4% of the actual monthly median price.

## Interpretation and limitations
- ETS and ARIMA are very close, and both produce a flat forecast (a level near the end of the training data). The price series has little predictable trend, and the seasonal SARIMA model did worse because 36 training months give it only three seasonal cycles to learn from.
- The dataset is a single 2019 snapshot. Each listing's price is assigned to the month of its last review, so the series approximates price over time rather than recording true historical prices.

## How to run
Open `airbnb_Manar.Rmd` in RStudio, install the packages loaded in the first chunk (`forecast`, `prophet`, `tseries`, `tidyverse`, etc.), and click **Knit**.

## Files
- `airbnb_Manar.Rmd`: full analysis and report
- `Airbnb-Manar-final.pdf`: key slides from the project presentation

## Author
Manar Ben Salem. South Mediterranean University, Business Analytics.
