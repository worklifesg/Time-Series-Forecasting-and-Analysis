## Beginner Course - Time Series with Statsmodel

In this section, we will discuss time series analysis using Statsmodels module in Python. 

<h4> Statsmodel libraries can be used for time series forecasting. </h4>

Statsmodels is the Python modeule that provides classes and functions for the estimation of many different statistical models, as well as for conducting statistical tests and statistical data exploration 

In this whole section, we will go through:

   - Introduction to Statsmodels 
   - ETS Decomposition 
   - Moving Averages
   - Holt WInter Methods
   - Statsmodels Time Series Exercises

### 1. Introduction to Statsmodels

 - Here will learn how to call a function test from Statsmodels that have a lot of statistical tests built in.
 - Learn about Hodrick-Prescott filter

Key important factors to understand in time series forecasting

 - TRENDS : 'Upward' trend - slope is positive, 'Horizontal/Stationary' trend - if you were to avergae it out in the middle (constant line with slope almost zero, 'Downward' trend - negative slope, fall.
 - SEASONALITY : It is a repeating trend. Ex. snowboarding search on google - there is a upward trend in winter and downward trend in summer and it is repeating trend in every known cycle.
 - Cyclical : Trends with no set repetition. Ex: stocks, real world datasets in general

<h5> Hodrick-Prescott filter </h5>

It separates a time series (y_t) into a trend component (\tau_t) and a cyclical component (c_t)

  ![1](http://latex.codecogs.com/gif.latex?\dpi{110}&space;y_t=\tau_t&space;&plus;&space;c_t)   

where these two components can be determined by minimizing following quadratic loss function (![3](http://latex.codecogs.com/gif.latex?\dpi{110}&space;\lambda)) is a soothing parameter

  ![2](http://latex.codecogs.com/gif.latex?\dpi{110}&space;\mathrm{min}_{\tau_t}&space;\sum_{t=1}^T&space;c_t^2&space;&plus;&space;\lambda&space;\sum_{t=1}^T&space;[(\tau_t-\tau_{t-1})-(\tau_{t-1}-\tau_{t-2})]^2)

![3](http://latex.codecogs.com/gif.latex?\dpi{110}&space;\lambda) value handles variations in the growth rate of the trend component. Recommended values for ![3](http://latex.codecogs.com/gif.latex?\dpi{110}&space;\lambda) are 1600 for quarterly data, 6.25 for annual data and 129600 for monthly data


### 2. ETS Decomposition 

 - Error, Trend , Seasonality
 - Models
     - Exponential Smoothing
     - Trend Methods Models
     - ETS Decomposition (will work on this)

So here we are talking about seasonal decomposition where ETS Models will take each of these terms for  <b> smoothing </b> and may add them, multiply them, or even just leave some of them out

ETS Decomposition build an understanding of the behaviour of the model.

 - Trend component - general growth /decline pattern depending upon the dataset (observed data points)
 - Seasonal component - take seasonal components and straightening it out, basically removing trend component
 - Residual (Error) component - things that are not explained by the trend or seasonality. Tells us where the noise is less or more

Types of Models
 
 - additive model = when trend is more linear and seasonality and trend components seems to be constant over time
 - multiplicative model = when we are inc/dec at a non-linear rate (exponential)

### 3. Moving Averages 

Let us we have thousands of airline passanger dataset, we can use <b><i> Simple Moving Averages (SMA) </i></b>

 - Based on window size (larger - can check trend or smaller - can check seasonality)
 
<b> Don't think this SMA analysis as forecasting technique. Just consider this as generalized model to describe general behaviour of your time series dataset </b>

However, SMA consider entire model to be constrained for the same window size thus we can <b><i> expand this SMA analysis to more sophisticated analysis EWMA - Exponentially weighted moving average </i></b>.

Other disadvantages of SMA are:

 - Smaller window sizes will lead to more noise rather than signal.
 - Never reach to full peak or valley of the data due to averaging.
 - In reality, it doesn't tell about future behaviour (only generalized model).
 - Extreme Historical values can skew SMA significantly
 
<b> In EWMA </b> we consider recent data to be more weigthed as compared to old data. It allows to reduce lag effect from SMA and put more weigth on values that occured more recently.

<b> Amount of weight applied depends upon actual parameters used in EWMA and number of periods given in window size </b>

Mathematical details are given in the notebook.


