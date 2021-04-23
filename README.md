# Retail-TimeSeries-Forecasting-Methods
 A comparative breakdown of traditional econometric timeseries models vs. more modern ML methods for predicting a retail firm's (supermarket) sales over a short to medium horizon

## BUSINESS PROBLEM / PURPOSE OF THIS PROJECT
- Because purchase budgets are contingent on the sales forecast (calculated as COGS benchmarks) at the end of each month for the proceding month, the sales forecasts are running 4w static forecasts. This means that the next month's sales (forecasted by week) must be forecasted in advance and cannot be iteratively updated (walk-forward predictions). This problem has been further exacerbated by COVID creating a sort of paradigm shift in supermarket trends
- Thus, sales (and hence, expense budgets) must be confirmed on a rolling 1M (~4W) basis
- The overarching challenge is to create a robust 1M static forecast method, from which the relative department COGS benchmarks can be applied and purchasing budgets can be distributed in advance to stores on a rolling 1 month basis
 - overly optimistic sales forecasts result in high budgets, inventory ballooning and potentially lower bottom-line GP/NP through higher shrinkage
 - overly pessimistic forecasts could result in inventory gaps which result in less sales, and less GP/NP as a result


## THE DATA 
- Roughly 3Y of daily sales data (from 2019:2021) 
 - includes items by day, baskets (# transactions), average spend per customers, marked-down sales, promotional sales

## PROJECT FLOW
- Data cleaning (for privacy concerns, the name of the store and other sensitive information has been censored)
- Create Data Features:
 - Create dummy variables to mark public holidays (automatically achieved by use of a package called "holidays" and specialised to the QLD region
Acknowledge the impact of COVID occuring half-way through the sample. Statistically determine the data's breakpoint (noted by a drastic change in trend to reflect customer's new shopping habits)- use this knowledge to further restrict the time horizon of the sample to maintain representativeness
 - Add additional time series features in the XGboost section, including: day of the week, week of the year
  - Also experiment with a "days since last public holiday" and "days until" time series feature, but it was ultimately found to be insignificant.
-Split the now restricted post structural break data into test-train split to match the business problem (4W static forecast)
- Initial Data Exploration:
 -View some time series characteristics of the data which can help inform the SARIMAX / TBATS models
 -ACF / PACF etc.
 
 ## MODELS
- Baseline (Naive Forecast) (~ 10% MAPE)
- Walk-forward SARIMAX Vs. Static SARIMAX (~ 3.56% MAPE)
- Automate the procedure of optimisting the BIC for selecting the length of the (AR, MA, SAR, SMA) terms in the SARIMAX model; whereby, X = holiday_dummy variable
 - Note: the walk-forward procedure only beat the static SARIMAX procedure MAPE by (0.08%)
- TBATS forecast (~ 7.56% MAPE) with seasonality informed from the ACF/PACF plots
- XGBoost - lengthy parameter tuning and refinement process to settle on the final model iteration which achieved (~2.99% MAPE)
 - Present an analysis on the significance of each feature in the XGboost mode


## PRESENT A FINAL MODEL COMPARISON AND GRAPHICAL BREAKDOWN
- Generally the static sarimax is presented as performing surprisingly well given its ease of implementation and relative simplicity to the XGBoost
- XGboost performd the best, but the trade-off is computational time and complexity

## FUTURE PROJECT DEVELOPMENT:
- Increase the forecast horizon from 1M to 3M and recalibrate the models to determine if the SARIMAX still performs reasonably well Vs. the more complicated XGBoost model
