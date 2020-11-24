## Intermediate Course - General Forecasting Model (ARIMA)

In this section, we will discuss forecasting techniques used for time data series.  

In this whole section, we will go through:

<h3> General Forecasting Methods </h3>

<b> Forecasting Procedure: </b> 

 - Choose Model (through statistical analysis)
 - Split data into train and test sets (for fairly evaluate our model)
 - Fit model on training set
 - Evaluate model on test set
 - Refit model on etire dataset
 - Forecast for future dataset

<b> Section Overview: </b> 

 - Forecasting
 - ACF and PACF plots
 - Autoregression - AR
 - Descriptive Statistics and Tests
 - Choosing ARIMA orders
 - ARIMA based models
 
### 1. Introduction to Forecasting

Here we will discuss few important terms and evaluating metrics used for times series continuous data.

#### Train/Test Split

The test size should ideally be at least as large as maximum forecast horizon required. It is generally around 20% of the total sample. However more is the test size (longer forecast horizon) prediction is less accurate (as more nosie is added)

#### Evaluating Predictions

To quantify how off our predicted values are rather than seeing visually. Metrics like accuracy or recall are not good for times series model as we need metrics designed for continuous values. We will use:

 - MAE - Mean Absolute Error
 - MSE - Mean Squared Error
 - RMSE - Root Mean Square Error

![1](http://latex.codecogs.com/gif.latex?\dpi{110}&space;\textrm{MAE:}~~~~&space;\dfrac{1}{n}\sum_{i=1}^n&space;&space;|y_i-\hat{y}_i|)

![1](http://latex.codecogs.com/gif.latex?\dpi{110}&space;\textrm{MSE:}~~~~&space;\dfrac{1}{n}\sum_{i=1}^n&space;&space;(y_i-\hat{y}_i)^2)

![1](http://latex.codecogs.com/gif.latex?\dpi{110}&space;\textrm{RMSE:}~~~~&space;\sqrt{\dfrac{1}{n}\sum_{i=1}^n&space;&space;(y_i-\hat{y}_i)^2)

<b> MAE - averaging errors doesn't tell us really whether we are too much or less off from predicted values

MSE - Large errors are noted more than MAE (can tell few points that are bit off as well) but when we square the errors, units are also squared that is hard to unit

RMSE - function as MSE but units are as original value chosen </b>


Acceptable value for RMSE - depends upon data source. Ex RMSE of 20$ is good for future price of house but very bad for future price of candy price

#### Stationary

Time series dataset is said to be stationary if it doesn't exhibit trends or seasonality that is flucatuations in the data are entirely due to outside forces/noise

#### Non-Stationary

Time series dataset is said to be non-stationary if it exhibit trends or seasonality or both.

### 2. AutoCorrelation Function (ACF) and Partial AutoCorrelation Function (PACF)

##### ACF (AutoCorrelation Function), PACF (Partial AutoCorrelation Function)

Correlation : Measure of strength of the linear relationship between 2 variables. [-1 1]

 - -1 means strong neative relationship
 - +1 means strong positive relationship
 - 0 means weaker relationship

AutoCorrelation (Correlogram) shows correlation of the series with itself, lagged by x times itself.(yaxis = correlation, xaxis = no. of times units of lag)

We need to build AC vs Shift (i-1, i-2,....) and it starts to decrease/increase (for all possible x lag units) 

 - Gradual Decline (less correaleted as we shift more and more and bit of seasonality)
 - Sharp Drop off (sudden drop in correlation and then constant in general)

##### PACF:

Residual - Error that is not explained by linear model. 

 - So we calculate erros vs (i-1)
 - Then we calculate residuals fiited on (i-1) vs (i-2) shift
 - Then plot correlation and so on (i-3....)

In general we expect PACF to drop off quite quickly

<b> Summary : </b> ACF describes autocorrelation between an observation and another observation at a prior time step that has both direct/indirect dependence whereas PACF decribes relationship between an observation and its lag. These 2 plots can help choose order parameters for ARIMA based models. Later we will see it is much easier to implement grid search to find parameter values.

##### Autocovariance for 1D
In a <em>deterministic</em> process, like ![1](https://latex.codecogs.com/gif.latex?\sin (x)), we always know the value of y for a given value of x. However, in a <em>stochastic</em> process there is always some randomness that prevents us from knowing the value of y. Instead, we analyze the past (or <em>lagged</em>) behavior of the system to derive a probabilistic estimate for ![1](https://latex.codecogs.com/gif.latex?\hat{y}).

One useful descriptor is <em>covariance</em>. When talking about dependent and independent x and y variables, covariance describes how the variance in x relates to the variance in y. Here the size of the covariance isn't really important, as x and y may have very different scales. However, if the covariance is positive it means that x and y are changing in the same direction, and may be related.

With a time series, x is a fixed interval. Here we want to look at the variance of ![1](https://latex.codecogs.com/gif.latex?\y_t) against lagged or shifted values of ![1](https://latex.codecogs.com/gif.latex?\y_{t+k})

For a stationary time series, the autocovariance function for ![1](https://latex.codecogs.com/gif.latex?\gamma) is given as:

![1](http://latex.codecogs.com/gif.latex?\dpi{110}&space;{\displaystyle&space;{\gamma}_{XX}(t_{1},t_{2})=\operatorname&space;{Cov}&space;\left[X_{t_{1}},X_{t_{2}}\right]=\operatorname&space;{E}&space;[(X_{t_{1}}-\mu&space;_{t_{1}})(X_{t_{2}}-\mu&space;_{t_{2}})]})

We can calculate a specific ![1](https://latex.codecogs.com/gif.latex?\gamma_k) with:

![1](http://latex.codecogs.com/gif.latex?\dpi{110}&space;{\displaystyle&space;\gamma_k&space;=&space;\frac&space;1&space;n&space;\sum\limits_{t=1}^{n-k}&space;(y_t&space;-&space;\bar{y})(y_{t&plus;k}-\bar{y})})

##### Unbiased Autocovariance
Note that the number of terms in the calculations above are decreasing.<br>Statsmodels can return an "unbiased" autocovariance where instead of dividing by n we divide by n-k.

##### Autocorrelation for 1D
The correlation ![1](https://latex.codecogs.com/gif.latex?\rho) (rho) between two variables ![1](https://latex.codecogs.com/gif.latex?y_1,y_2) is given as:

##### ![1](https://latex.codecogs.com/gif.latex?\rho&space;=&space;\dfrac&space;{E[(y_1-\mu_1)(y_2-\mu_2)]}&space;{\sigma_{1}\sigma_{2}}&space;=&space;\dfrac&space;{{Cov}&space;(y_1,y_2)}&space;{\sigma_{1}\sigma_{2}}),

where E is the expectation operator, ![1](https://latex.codecogs.com/gif.latex?\mu_{1},\sigma_{1},&space;\mu_{2},\sigma_{2}) are the means and standard deviations of ![1](https://latex.codecogs.com/gif.latex?y_1,y_2).

When working with a single variable (i.e. <em>autocorrelation</em>) we would consider ![1](https://latex.codecogs.com/gif.latex?y_1) to be the original series and ![1](https://latex.codecogs.com/gif.latex?y_2) a lagged version of it. Note that with autocorrelation we work with ![1](https://latex.codecogs.com/gif.latex?\bar{y_1}), that is, the full population mean, and <em>not</em> the means of the reduced set of lagged factors (see note below).

Thus, the formula for![1](https://latex.codecogs.com/gif.latex?\rho_k) for a time series at lag ![1](https://latex.codecogs.com/gif.latex?k) is:

![1](https://latex.codecogs.com/gif.latex?\displaystyle&space;\rho_k&space;=&space;\frac&space;{\sum\limits_{t=1}^{n-k}&space;(y_t&space;-&space;\bar{y})(y_{t&plus;k}-\bar{y})}&space;{\sum\limits_{t=1}^{n}&space;(y_t&space;-&space;\bar{y})^2})

This can be written in terms of the covariance constant ![1](https://latex.codecogs.com/gif.latex?\gamma_k) as:

![1](https://latex.codecogs.com/gif.latex?\displaystyle&space;\rho_k&space;=&space;\frac&space;{\gamma_k&space;n}&space;{\gamma_0&space;n}&space;=&space;\frac&space;{\gamma_k}&space;{\sigma^2})

Note that ACF values are bound by -1 and 1. That is, ![1](https://latex.codecogs.com/gif.latex?\displaystyle&space;-1&space;\leq&space;\rho_k&space;\leq&space;1)

##### Partial Autocorrelation
Partial autocorrelations measure the linear dependence of one variable after removing the effect of other variable(s) that affect both variables. That is, the partial autocorrelation at lag ![1](https://latex.codecogs.com/gif.latex?k) is the autocorrelation between ![1](https://latex.codecogs.com/gif.latex?y_t) and ![1](https://latex.codecogs.com/gif.latex?y_{t+k}) that is not accounted for by lags 1 through k−1.

A common method employs the non-recursive <a href='https://en.wikipedia.org/wiki/Autoregressive_model#Calculation_of_the_AR_parameters'>Yule-Walker Equations</a>:

![1](https://latex.codecogs.com/gif.latex?\phi_0&space;=&space;1)

![1](https://latex.codecogs.com/gif.latex?\phi_1&space;=&space;\rho_1&space;=&space;-0.50)

![1](https://latex.codecogs.com/gif.latex?\phi_2&space;=&space;\frac&space;{\rho_2&space;-&space;{\rho_1}^2}&space;{1-{\rho_1}^2}&space;=&space;\frac&space;{(-0.20)&space;-&space;{(-0.50)}^2}&space;{1-{(-0.50)}^2}=&space;\frac&space;{-0.45}&space;{0.75}&space;=&space;-0.60)

As ![1](https://latex.codecogs.com/gif.latex?k)  increases, we can solve for ![1](https://latex.codecogs.com/gif.latex?\phi_k)  using matrix algebra and the <a href='https://en.wikipedia.org/wiki/Levinson_recursion'>Levinson–Durbin recursion</a> algorithm which maps the sample autocorrelations ![1](https://latex.codecogs.com/gif.latex?\rho)  to a <a href='https://en.wikipedia.org/wiki/Toeplitz_matrix'>Toeplitz</a> diagonal-constant matrix. The full solution is beyond the scope of this course, but the setup is as follows:


![1](https://latex.codecogs.com/gif.latex?\begin{pmatrix}\rho_0&\rho_1&\cdots&space;&\rho_{k-1}\\&space;\rho_1&\rho_0&\cdots&space;&\rho_{k-2}\\&space;\vdots&space;&\vdots&space;&\ddots&space;&\vdots&space;\\&space;\rho_{k-1}&\rho_{k-2}&\cdots&space;&\rho_0\\&space;\end{pmatrix}\quad&space;\begin{pmatrix}\phi_{k1}\\\phi_{k2}\\\vdots\\\phi_{kk}&space;\end{pmatrix}&space;\mathbf&space;=&space;\begin{pmatrix}\rho_1\\\rho_2\\\vdots\\\rho_k\end{pmatrix})


##### Partial Autocorrelation with OLS
This provides partial autocorrelations with <a href='https://en.wikipedia.org/wiki/Ordinary_least_squares'>ordinary least squares</a> (OLS) estimates for each lag instead of Yule-Walker.
