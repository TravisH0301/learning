# Time Series Forecasting

## Charateristics of time series data

### Seasonality

### Stationarity

### Autocorrelation

## Considerations for time series regression
### Stationarity 
- Variables must be stationary with constant mean and variance. Stationarity is independent of time. The distribution of a stationary variable resembles a Gaussian distribution.
- Augmented Dickey-Fuller unit root test can be used to measure stationarity.
- Solution to non-stationarity:
  - Trasformation can be applied that includes:
    - Difference transformation: difference between value at t and value at t-i. It reduces seasonality and trends
    - Rolling means: subtract rolling mean (from previous x timestamps) from value at t. Works well when mean is time dependent.
    - Power transformation: 
      - Square-root transformation: offsets quadratic trend 
      - Log transformation: offsets exponential trend
      - Box-Cox tranformation: automatically configures type of power transformation

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

### Heteroscedasticity of Residuals
- Residuals are assumed to have a constant variance, but if variance is inconsistent, then heteroscedasticy exists.
- Residual distribution can be observed.
- Goldfled-Quandt test can be used. 

### Autocorrelation of Residuals           
- Residuals are assumed to be independent of time. Autocorrelation of residuals should not indicate a pattern.
- Autocorrelation can be visually observed.
- Durbin-Watson statistic can be used.

## Uncertainty of forecasting
- Retrain and predict for new dataset > uncertainty of time series pattern makes pretrained model go wrong
- Forecasting is always wrong. The main idea is to measure uncertainty and make sure business is well prepared 
for uncertainties. 
- Models measuring uncertainties: ARCH & GARCH
- If forecasting residual resembles a known distribution ex)Gaussian, then predictions are within known uncertainties.
If random residual distribution (Heteroscedasticity), difficult to measure uncertainty, hence, ill-behaved prediction. 

## Statistical modelling  for time series forecasting

## Deep learning for time series forecasting
