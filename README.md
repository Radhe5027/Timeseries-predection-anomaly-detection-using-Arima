# Time series-prediction-anomaly-detection-using-Arima
ARIMA Model: 

An ARIMA model is characterized by 3 terms: p, d, q
where,
p is the order of the AR term
q is the order of the MA term
d is the number of differences required to make the time series stationary

If a time series, has seasonal patterns, then you need to add seasonal terms and it becomes SARIMA, short for ‘Seasonal ARIMA’. More on that once we finish ARIMA.
So, what does the ‘order of AR term’ even mean? Before we go there, let’s first look at the ‘d’ term.

The first step to building an ARIMA model is to make the time series stationary.

In the context of time series analysis, a stationary time series is one whose statistical properties, such as mean, variance, and autocorrelation, do not change over time. In simpler terms, the properties of the time series remain constant across different time periods.



Constant Mean: The mean of the time series should remain constant over time. This means that the average value of the series does not increase or decrease with time.


Constant Variance: The variance of the time series should also remain constant over time. This implies that the spread or dispersion of the data points around the mean does not change over time.
So how to make a series stationary?

The "Integrated" part of ARIMA refers to the differencing operation applied to make the time series stationary. Differencing is a process of computing the differences between consecutive observations. It helps remove trends and seasonality from the time series, making it stationary and suitable for autoregression.
Steps to check for stationarity:
Step1:check data is stationary or not from  Augmented Dickey-Fuller (ADF) test
Step2: Differencing 
Step3:  again check Augmented Dickey-Fuller (ADF) test

Check from the ADF test if is it stationary or not.
Steps:
ADF is a hypothesis test that assesses the presence of a unit root in a time series. A unit root is an indication of non-stationarity in the data, and the presence of a unit root suggests that the time series has a trend and is not suitable for the ARIMA model.
It is based on 2 hypotheses:
The null hypothesis (H0) of the ADF test:  is that the time series has a unit root, which means it is non-stationary. 
The alternative hypothesis (H1) is that the time series is stationary.
Significance of ADF statistics value:
If the ADF statistic is significantly negative (i.e., more negative than the critical values), it provides evidence against the null hypothesis of non-stationarity. This suggests that the data is likely stationary.



If the ADF statistic is less negative or positive, it means there is weak evidence against the null hypothesis, and the data is likely non-stationary.


If the ADF statistic is close to zero or positive, it indicates strong evidence in favor of the null hypothesis, suggesting that the data is non-stationary.
In summary, the ADF statistic comp

When we perform the ADF test we obtain test statistics and a p-value. If the p-value is below a certain significance (e.g., 0.05), we reject the null hypothesis and conclude that the time series is stationary. On the other hand, if the p-value is above the significance level, we fail to reject the null hypothesis, indicating that the time series is non-stationary.
Conclusion: if p<0.05 TS is stationary else not
 
Step2: Through differencing, we can convert nonstationary data to stationary
Ex.

This is Nonstationary data
Time     Value
------------------
1        10
2        15
3        25
4        30
5        35
6        40

Check from the ADF test if is it stationary or not.
If not apply differencing:
First-order difference = Value(t) - Value(t-1)
Result:
Time     Value   1st Difference
-------------------------------
1        10      NA               (No previous value for the first time step)
2        15      15 - 10 = 5
3        25      25 - 15 = 10
4        30      30 - 25 = 5
5        35      35 - 30 = 5
6        40      40 - 35 = 5

As we can see in first order diff only we obtain constant data let's apply second-order differencing 
2nd Difference
---------------
NA
NA
5 - 10 = -5
5 - 10 = -5
0
0
Hence to deal with stationarity most common approach is to differentiate (). The value of d is the minimum no of differences needed to make the series stationary. If the series is already stationary d=0

p   >> order of Auto regressive term(AR) (with PACF )
# concept of threshold and significance value: whichever lines are crossing the threshold line are more correlated with curr time series

The 'p' parameter in ARIMA represents the number of lags of the time series to be used as predictors for the current value. 
This means how many past values you are using to calculate current ex if p==2 we are using 2 previous values to calculate new

q    >>q' - Order of Moving Average (MA) Term:(with ACF)
The 'q' parameter in ARIMA represents the number of lagged forecast errors that should go into the ARIMA model. The forecast error is the difference between the predicted value and the actual value. The MA term helps account for the autocorrelation in the error terms.

If 'q' is set to 1, it means we are using the error from the most recent forecast to predict the current value
 For example, if the forecast errors show a significant autocorrelation with the one most recent lagged error, we might set 'q' to 1. This means we are using the Moving Average model with one lagged error as a predictor to improve the forecast.

Hence 'p' and 'q' are hyperparameters that you need to determine while fitting an ARIMA model to a time series. 
You can use methods like the
 Akaike Information Criterion (AIC) or
 Bayesian Information Criterion (BIC) 
to choose the best combination of 'p' and 'q' that minimizes the model's error and provides accurate forecasts. It is often done through a process called hyperparameter tuning.

A pure Auto-Regressive (AR only) model is one where Yt depends only on its own lags. That is, Yt is a function of the ‘lags of Yt’. 


where Y{t-1} is the lag1 of the series, beta1 is the coefficient of lag1 that the model estimates, and `alpha` is the intercept term, also estimated by the model.

Likewise a pure Moving Average (MA only) model is one where Yt depends only on the lagged forecast errors.
where the error terms are the errors of the autoregressive models of the respective lags. The errors Et and E(t-1) are the errors from the following equations :


So what does the equation of an ARIMA model look like?



Predicted Yt = Constant + Linear combination Lags of Y (upto p lags) + Linear Combination of Lagged forecast errors (upto q lags)
The objective, therefore, is to identify the values of p, d, and q. But how?
Let’s start with finding the ‘d’.
d is found by taking the differences, like the cumulative differences.

Performed Dickey-Fuller test to check stationarity.Transformed and differenced the data to make it stationary.
Analysed ACF, PACF to implement ARIMA model(p=2,q=2) with 90th % residual value for anomaly detection

