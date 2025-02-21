# Inflation Rate Time Series Analysis
# Hunter Grigsby

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Time Series Plot
- **resources:** <https://fred.stlouisfed.org/series/T10YIE#0>.
- **data:** inflation_time_series
- 
```{r, message=FALSE, out.width="80%", fig.align="center"}
library(tseries)
library(vars)
library(readr)
mydata = read.csv("T10YIE.csv")
inflation_time_series <- ts(mydata$T10YIE, frequency = 12, start = c(2003, 1))
plot(inflation_time_series)
```
## Decomposition Plot
```{r, out.width="80%", fig.align="center", message=FALSE, echo=TRUE}
mydata = diff(inflation_time_series, 1)
decomposed_data = decompose(inflation_time_series)
plot(decomposed_data)
```

## ACF Plot
```{r, out.width="75%", fig.align="center", message=FALSE, echo=TRUE}
acf(inflation_time_series, main = "ACF of Original Time Series")
```

## ADF Test
```{r, r, out.width="75%", fig.align="center", message=FALSE, echo=TRUE}
adf_test <- adf.test(inflation_time_series)
print(adf_test)
```

## Differencing -> ADF Plot
```{r, out.width="75%", fig.align="center", message=FALSE, echo=TRUE}
ts_diff <- diff(inflation_time_series, 1)
plot(acf(ts_diff))

ts1 = diff(inflation_time_series, 1)
adf.test(ts1)

ts2 = diff(inflation_time_series, 2)
adf.test(ts2)
```

## Simple Exponential Smoothing
```{r, out.width="75%", fig.align="center", message=FALSE, echo=TRUE}
library(tseries)
library(forecast)
library(ggplot2)

ses_model <- ses(inflation_time_series, h = 12)
plot(forecast(ses_model))
forecast(ses_model)
```

## Holt's Linear Trend Method
```{r, out.width="75%", fig.align="center", message=FALSE, echo=TRUE}
holt_model <- holt(inflation_time_series, h = 12)
plot(forecast(holt_model))
forecast(holt_model)
```
## Holt-Winters Method
```{r, out.width="75%", fig.align="center", message=FALSE, echo=TRUE}
hw_model <- hw(inflation_time_series, h = 12)
plot(forecast(hw_model))
forecast(hw_model)
```

## Evaluate the Accuracy of the Three Exponential Smoothing Models
```{r}
accuracy(ses_model)
accuracy(holt_model)
accuracy(hw_model)
```

## ARIMA Estimation
```{r}
arima <- auto.arima(inflation_time_series)
```

## ARIMA Results
```{r}
summary(arima)
```

## Equation
$$
y_t = 0.35 y_{t-1} + \varepsilon_t
$$

## ARIMA Forecast
```{r, out.width="75%", fig.align="center", message=FALSE, echo=TRUE}
forecast(arima, h= 12)
plot(forecast(arima, h= 12))
```

## Model Dianogstics by Using Checkresiduals()
```{r, out.width="75%", fig.align="center", message=FALSE, echo=TRUE}
log_inflation <- inflation_time_series %>% log() %>% diff()

fit.arima <- auto.arima(log_inflation)
auto.arima(inflation_time_series)
checkresiduals(fit.arima)
```










