# Demand Forecasting
## Objective
The objective of this project is to forecast medium-term electricity demand in Austin, Texas, with a one-week-ahead horizon and one-hour resolution. The forecast utilizes historical data spanning from 2002 to 2017 and incorporates both electricity demand and weather data.

## Data Collection
### Electricity Demand Data: 
The Electric Reliability Council of Texas (ERCOT) provides historical electricity demand data, available here. The data for each year is segmented by weather zones, with this project focusing on the South Central weather zone, using corresponding weather data for the city of Austin.

### Weather Data:
Weather Underground provides historical weather data API, which can be found here. To use this API, an API key must be obtained from Weather Underground. The project's parser extracts relevant data from a single API endpoint and scrapes all dates of interest to compile hourly weather data from 2002-2017 into a single dataframe.

## Feature Engineering
1. One-Hot Encoding: The categorical feature "condition" is converted into a one-hot encoding, grouping the original 17 weather conditions into 6 broad categories.

2. Merging Data: The electricity demand and weather data are merged into a single dataframe. Missing values created by the merge are filled in using back-filling for one-hot encoding features and interpolation for continuous features.

3. Temporal Features: Features such as "weekday" and the sine/cosine transformation of the hour of the day are created to capture temporal effects.

4. Normalization: Continuous features are normalized to be in the range [0, 1] to facilitate model training. The minimum and maximum values for each feature are saved to transform them back to their original scale later.

## Forecasting Models
### Monthly Averages Model: 
The monthly average electricity demand is modeled using a seasonal ARIMA (SARIMA) model due to strong yearly seasonality in the data.

### Hourly Residuals Model: 
The hourly residuals (hourly demand above or below the monthly average) are modeled using linear regression, gradient boosting regression (GBR), and multi-layer perceptron (MLP) regression.

## Results
The GBR model performs best, followed by MLP, and linear regression performs significantly worse.
Visualizations of one-week-out forecasts for each model over a span of two weeks on the test set show that GBR has the best performance, followed by MLP, while linear regression performs poorly.

![image](https://github.com/PriyankaL97/Demand-Forecasting/assets/166748907/e4c7dd37-29ea-4928-926a-a159ee9ebe77)

## Conclusion

This project considered the issue of forecasting electricity demand at one week intervals with a resolution of one hour. This is an important application for utilities and power plant operators so that they can make informed fuel purchase decisions and schedule maintenece at optimal times. In order to make more accurate forecasts, the original timeseries data was decomposed into monthly averages and hourly residuals. The monthly averages were modeled using a SARIMA model, and the hourly residuals were modeled using three different regression models. Of these, gradient boosting regression showed the best performance on the test set, with a 61% improvement over the baseline persistance model.

The accuracy of the best model (gradient boosting regression) is actually quite good. This model could certainly be used for one-week-out forecasting, at least as a first-order estimate. If a higher accuracy model is required, the authors present two suggestions for further study:

- For both GBR and MLP, only a very rough grid search over hyperparameters was performed, and the possible values of hyperparameters were chosen somewhat arbitrarily. A more thorough investigation of hyperparameters is strongly suggested, especially for MLP.
- The most obvious features for predicting electricity demand were used in this project: autoregressive features, temporal features, and weather features. A study of where the model is predicting with the lowest accuracies could be interesting, with the hope of finding a pattern which could be fixed with additional features. For example, if one found that the model was consistently under-predicting electricity demand during major sporting events, adding a boolean feature of "major_event" could help improve model accuracy. 





