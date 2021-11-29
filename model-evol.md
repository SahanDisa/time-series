# <center>From AR to SARIMAX: Mathematical Definitions of Time Series Models</center>

## Overview

AR, MA, ARMA, ARIMA, ARIMA and ARIMAX are univariate time series models that are special cases of SARIMAX. This guide gives the mathematical definitions of these models, but does not go into in-depth explanations, model selection or parameter estimation.

## Precursors to SARIMAX

### Autoregressive (AR) Models
Suppose we have a time series given by $\{ y_{t} \}$. An $AR(p)$ model can be specified by

$$ y_{t} = \beta + \epsilon_{t} + \sum\limits_{i=1}^p \theta_{i} y_{t-i} $$

Where $p$ is the number of time lags to regress on, $\epsilon_{t}$ is the noise at time $t$ and $\beta$ is a constant. 

This equation can be made more concise through the use of the lag operator, $L$.

$$L^{n} y_{t} = y_{t-n}$$

Taking $\Theta(L)^{p}$ to be an order $p$ polynomial function of $L$, we can instead define an autoregressive model by

$$ y_{t} = \Theta(L)^{p} y_{t} + \epsilon_{t}$$

Taking note that the constant has been absorbed into the polynomial $\Theta$.

### Moving average (MA) Models.
Whereas autoregressive models regress on prior values of $y_{t}$, moving average models regress on prior values of error. An $MA(q)$ model can be specified by

$$ y_{t} = \Phi(L)^{q} \epsilon_{t} + \epsilon_{t}$$

Where $q$ is the number of time lags of the error term to regress on and $\Phi$ is defined analagously to $\Theta$.

### Autoregressive Moving Average (ARMA) Models
$ARMA(p,q)$ models are simply a sum of $AR(p)$ and $MA(q)$ models.

$$ y_{t} = \Theta(L)^{p} y_{t} + \Phi(L)^{q} \epsilon_{t} + \epsilon_{t}$$

### Autoregressive Integrated Moving Average (ARIMA) Models
To help tackle non-stationary data, we introduce an integration operator $\Delta^{d}$, defined as follows

$$ y_{t}^{[d]} =\Delta^{d} y_{t} = y_{t}^{[d-1]} - y_{t-1}^{[d-1]} $$

where $ y_{t}^{[0]} = y_{t} $ and $d$ is the order of differencing used.

We can now fit an $ARMA(p, q)$ model to $y_{t}^{[d]}$ rather than $y_{t}$. 

$$ y_{t}^{[d]} = \Theta(L)^{p} y_{t}^{[d]} + \Phi(L)^{q} \epsilon_{t}^{[d]} + \epsilon_{t}^{[d]}$$

This is equivalent to an $ARIMA(p,d,q)$ model on $y_{t}$

$$ \Delta^{d} y_{t} = \Theta(L)^{p} \Delta^{d} y_{t} + \Phi(L)^{q} \Delta^{d} \epsilon_{t} + \Delta^{d} \epsilon_{t}$$

With some algebra, we can re-arrange the equation and absorb constants into the polynomials $\Theta$ and $\Phi$. 

$$ \Theta(L)^{p} \Delta^{d} y_{t} = \Phi(L)^{q} \Delta^{d} \epsilon_{t}$$

### SARIMA
SARIMA models take seasonality into account by essentially applying an ARIMA model to lags that are integer multiples of seasonality. Once the seasonality is modelled, an ARIMA model is applied to the leftover to capture non-seasonal structure.

To see this more clearly, suppose we have a time series $ \{ y_{t} \} $ with seasonality $s$. We can try to eliminate the seasonality with differencing, by applying the differencing operator $\Delta_{s}^{D}$ to take the seasonal differences of the time series. Here $s$ is the number of time lags comprising one full period of seasonality. $D$ takes on a similar meaning to $d$ in ARIMA models, but instead applies to *seasonal* lags.

We can then capture any remaining structure by applying an $ARMA(P, Q)$ model, to the differenced values, but using seasonal lags. i.e. instead of using a regular lag operator $L$, we use $L^{s}$. $P$ and $Q$ are again seasonal time lags

$$ \Delta_{s}^{D} y_{t} = \theta(L^{s})^{P} \Delta_{s}^{D} y_{t} + \phi(L^{s})^{Q} \Delta_{s}^{D} \epsilon_{t} + \Delta_{s}^{D} \epsilon_{t} $$

As with ARIMA, massaging the equation and absorbing constants into polynomials yields the following concise form

$$ \theta(L^{s})^{P} \Delta_{s}^{D} y_{t} =  \phi(L^{s})^{Q} \Delta_{s}^{D} \epsilon_{t} $$

With any seasonality now removed, we can apply another $ARIMA(p, d, q)$ model to $ \Delta_{s}^{D} y_{t} $ by multiplying the seasonal model by the new ARIMA model.

$$ \Theta(L)^{p} \theta(L^{s})^{P} \Delta^{d} \Delta_{s}^{D} y_{t} = \Phi(L)^{q} \phi(L^{s})^{Q} \Delta^{d} \Delta_{s}^{D} \epsilon_{t}$$

This is the general form of a $SARIMA(p, d, q)(P, D, Q, s)$ model.

## ARIMAX and SARIMAX

ARIMAX and SARIMAX models simply take exogenous variables into account - ie variables measured at time $t$ that influences the value of our time series at time $t$, but that are not autoregressed on. To do this, we simply add the terms in on the right hand side of our ARIMA and SARIMA equations.

For $n$ exogenous variables defined at each time step $t$, denoted by  $x_{t}^{i}$ for $ i \leq n $, with coefficients $\beta_{i}$, the $ARIMAX(p, d, q)$ model is defined by

$$ \Theta(L)^{p} \Delta^{d} y_{t} = \Phi(L)^{q} \Delta^{d} \epsilon_{t} + \sum_{i=1}^{n} \beta_{i} x^{i}_{t}$$

and the $SARIMAX(p, d, q)(P, D, Q, s)$ model by

$$ \Theta(L)^{p} \theta(L^{s})^{P} \Delta^{d} \Delta_{s}^{D} y_{t} = \Phi(L)^{q} \phi(L^{s})^{Q} \Delta^{d} \Delta_{s}^{D} \epsilon_{t} + \sum_{i=1}^{n} \beta_{i} x^{i}_{t} $$

## Further References

Chatfield, C 2004, *The Analysis of Time Series : An Introduction*, 6th ed., Chapman & Hall/CRC, Boca Raton, Fla.
