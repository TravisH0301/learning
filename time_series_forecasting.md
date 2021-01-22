# Time Series Forecasting
Time series is a data where the data is recorded at regular or irregular timestamps. 

https://otexts.com/fpp2/autocorrelation.html

## Charateristics of time series data
There are several components of the time series data to look at to understand the nature of the data. 
It's always good to have a look at a general time series plot (for patterns), histogram (for skewness & stationarity), autocorrelation and partial autocorrelation.
As well as decomposition of the time series (if seasonality exists, period is required)

### Trend
Trend is a long-term change (increase/decrease) in data. 

### Seasonality
Seasonality is observed when the time series shows a cyclic pattern at certain time intervals that at a fixed and known intervals. 
This can be a weekly, monthly or yearly seasonality. A period of the seasonality can be observed from the sinusoidal function from the autocorrelation. 

https://www.analyticsvidhya.com/blog/2015/12/complete-tutorial-time-series-modeling/

https://people.duke.edu/~rnau/411diff.htm

https://otexts.com/fpp2/stationarity.html

### Cyclic
A cycle occurs when the data exhibit rises and falls that are not of a fixed frequency. These fluctuations are usually due to economic conditions, and are often related to the “business cycle”. 

### Stationarity
Stationarity refers to a constant mean, constant variance and independence to time. This includes a trend and seasonality. 
For statistical (Stochastic) modelling, stationarity of the time seriese is required. 

An underlying idea in statistical learning is repeating an experiment by drawing from the sample space. 
In the time-series context, a single run of stochastic process rather than repeated runs of the process. Therefore, stationarity is required so that 
observing a long run of stochastic process is similar to observing many independent runs of a stochastic process. 

Stationarity can be verified by the Augmented Dickey-Fuller unit root test (p-value < 0.05 rejects the null hypothesis of non-stationarity).


#### Transformation
Transformation can be applied to convert a non-stationary data to a stationary data. 
- Log transformation: helps to make variance constant by limiting variance size with log function
- 1st Order difference transformation: limits mean to be constant by removing changes in the level of time series (reduces trend/seasonality)
| y'(t) = y(t) - y(t-1)
- 2nd Order difference transformation: applied when 1st order doesn't stablise the mean. Removes the change in the change of the original data. 
| y"(t) = y'(t) - y'(t-1) = (y(t) - y(t-1)) - (y(t-1) - y(t-2))
- Seasonal difference transformation: removes the difference between the current and the previous observation from the same season (at season interval)
| y'(t) = y(t) - y(t-m), where m is season period
- Smoothing / Rolling: takes moving average to reduce seasonality & variance 

### Autocorrelation
Autocorrelation measures the correlation with its value at different time intervals. Sinusoidal function on autocorrelation indicates seasonality. <br>
Autocorrelation (ACF) measures both indirect and direct correlation and Particial Autocorrelation (PCAF) only measures direct correlation.<br>
The shaded region in the plot_acf() of statsmodels module represents confidence area (outside of the shaded region is statistically significant to reject no correlation hypothesis).

## Feature selection (for Multivariate forecasting)
Features can be selected by looking at correlations and causality to select features that have high correlation and causality to the target variable. And testing multicollinearity can help to remove features that cause collinearity. 

For causality, the Granger Causality test can be used to determine whether one time series is useful in forecasting another time series. 
It is a sattistical hypothesis test where value lower than confidence level is regarded as statistically confident to reject the null hypothesis of having no causality between two time series.

## Statistical modelling 
AR, MA, ARIMA, SARIMA

## Facebook Prophet (Additive modelling)
https://facebook.github.io/prophet/docs/quick_start.html#python-api<br>
Prophey provides a fast and accurate time series forecasting based on piecewise trend, seasonality and holiday (+ additional regressor variables). It accounts for non-linear trend with daily, weekly and yearly seasonality as well as holiday effects. It is also robust with missing values and outliers. 

***Unlike conventional time series regression model, Prophet doesn't require stationarity.*** Instead of looking at historical values, it looks at decomposed components (ex. trend, seasonality) to build prediction. As Prophet assumes stationary seasonality, if there is a unforeseen event like COVID-19 affecting the values, Prophet cannot take an account of such event. (Lagged moving average of error values may be added to the model after an initial forecast to make the model to learn from its historical values.)

y(t) = g(t) + s(t) + h(t) + e(t)

g(t): piecewise linear or logistic growth curve for modelling non-periodic change in time series (trend)<br>
s(t): periodic change (ex. weekly/yearly seasonality)<br>
h(t): effect of holidays with irregular schedules (optional)<br>
e(t): error term accounts for idiosyncratic changed not accompanied by the model<br>
*Time is a regressor for the target variable forecast*

### Additional regressor
Additional regressors can be added as a additive or multiplicative factor. Note that the regressor must have known values for both the past and the future time. If future values are not available, forecast (such as Prophet) can be performed to obtain the future values. In this case, regressor forecast must be an easy forecast unless it will pass its error to the target variable forecast. 

### Multiplicative seasonality
If the seasonality keeps on increasing size or reducing size (of oscilliation), the seasonality can be added as a multiplicative factor instead of an additive factor.<br>
https://facebook.github.io/prophet/docs/multiplicative_seasonality.html

### Problem with Prophet
Prophet deregisters the Pandas converters prohibiting drawing a plot directly from Pandas data format. 
#### Solution 1:
Re-registering Pandas converters with: pd.plotting.register_matplotlib_converters()
#### Solution 2:
Using Seaborn for plotting.

## Considerations for time series regression
### Multicollinearity (Collinearity)
- Multicollinearity occurs when multiple independent variables are highly correlated. 
- It may affect the accuracy of the model yet it mainly affects reliability in determining the effects of individual features and makes it difficult to interpret the model's behaviour.
- Causes of multicollinearity:
  - Badly designed/created data: variables are highly correlated at the time of data creation
  - Creation of a new variable which are dependent on other variables: causing redundancy
  - Variables referring to the same information: causing redundancy
  - Inaccurate use of dummy variables: make sure # of dummy variables = # of categorical variables - 1
  - Insufficient data
- Detection of multicollinearity:
  - Multicollinearity can be detected by looking at correlation between independent variables. This can be Pearson correlation coefficient, R^2 value between variables.
  - Variable Inflation Factors (VIF): measures likelihood of a variable being explained by other variables by taking the variable and regressing it against every other variables. This can overcome the limitation of bivariate correlation testing. 
    - VIF = 1/(1-R^2)
    - VIF starts from 1.
    - VIF larger than 5 or 10 indicates high multicollinearity between the selected variable and other variables. 
- Solution to multicollinearity:
  - Elimination of variables with high mmulticollinearity: iterative approach starting with highest variable
  - Combination of high correlated variables: can involve summation, subtraction, multiplication and etc
  - Increasing sample size

### Spurious Correlation
Correlation doesn't imply causation. Looking at causality between variables can help to provide additional information, yet, Granger causality doesn't 
account for *latent confounding effects* and does not capture instantaneous and non-linear causal relationships. Additionally, it is important to understand the 
problem and the concept to be able to correctly identify variables that are truly causal to the target variable. 

*Latent confounding effects*: Hidden confounding variable affecting the observation of causation. ex. Causation may be due to a hiddent confounding factor.  
*Confounding variable*: Variable that is correlated (or causal to) with an indenpendent variable and causal to dependent variable <br>

### Heteroscedasticity of Residuals (Verifies the consistency of a statistical model)
- Residuals are assumed to have a constant variance, but if variance is inconsistent, then heteroscedasticy exists.
- Residual distribution can be observed (using histogram to see if it resembles Gaussian distribution).
- Goldfled-Quandt test can be used. 

### White Noise 
- White noise time series is a time series where it meet the three criteria below:
  - Zero mean
  - Constant stadnard deviation
  - Weak autocorrelation
- When a time series is white noise, it is basically unpredictable (no pattern to study).
- When residuals of the time series model is a white noise then it means all patterns of the model are extracted by the model (good model). 
- White noise can be used to measure how well the model fits the time series data similar to heteroscedasticity of residuals. 

### Autocorrelation of Residuals (Verifies the statistical model)
- Residuals are assumed to be independent of time. Autocorrelation of residuals should not indicate a pattern.
- Autocorrelation can be visually observed.
- Durbin-Watson statistic can be used.

## Uncertainty of forecasting
- Retrain and predict for new dataset > uncertainty of time series pattern makes pretrained model go wrong
- Forecasting is always wrong. The main idea is to measure uncertainty and make sure business is well prepared 
for uncertainties. 
- Models measuring uncertainties: ARCH & GARCH
- **If forecasting residual resembles a known distribution ex)Gaussian, then predictions are within known uncertainties.**
If a random residual distribution (Heteroscedasticity) is observed, uncertainty is difficult to be measured, hence, it indicates an ill-behaved prediction. 
If it resembles a Gaussian distribution, it implies that the model works consistently throughout the timeline.
