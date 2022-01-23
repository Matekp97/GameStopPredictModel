---
author:
- Mattia Pavan
title: Project3
---

Ex. 1 {#ex.-1 .unnumbered}
=====

For this project I picked the data from American Express, a
multinational financial service.\
I’ve decided to analyze this data because my parents have a credit card
of this society and I was curious about the return of it, considering
also the Covid.\
More precisely the data are about the Adjusted Closure from 25/04/2016
to 29/04/2021, weekly.\

\
\
\
` `<span>“`%Y-%m-%d`”</span>\
\
`        `\
`    `\
`      `<span>“`%m/%y`”</span>\

![image](figure/unnamed-chunk-2-1){width="3in"}

`  `\

    ## [1] 263

We start from reading the dataset and plotting the closure.\
We can see that from april 2016 there is a positive trend almost
everywhere, we see only two big drop, one in December 2018, caused by
the America’s trade war with China, and the other one in March 2020, due
to Coronavirus.\

    ##           0           1           2           3           4           5 
    ## 672.4085740   0.1494207   0.0000000   1.9975248   3.0586610   4.9402828 
    ##           6           7           8           9 
    ##   6.9232955   8.5863670  10.5859466   7.9985271

` `

    ##           
    ## 0.9541256

`  `\

    ##         0         1         2         3         4         5         6 
    ## 20.502842  0.000000  1.636217  3.618661  2.438781  4.017572  5.297177 
    ##         7         8         9 
    ##  5.925949  4.267838  5.721747

` `

    ##      
    ## 0.01

![image](figure/unnamed-chunk-3-1){width="2in"}

We use the Akaike criterion to get the order of the autoregressive
model, and thanks to this we get that the p-value of our Augmented
Dickey-Fuller test is $0.954$. And so we can say that it is not
stationary, therefore it is not a white noise.\
For this reason, we introduce another time series, such that it’s
stationary, the returns.\
From the plot we can see that we have some evidence heteroscedasticity,
in fact we can see that the variance changes a lot, not only in the data
regarding the Coronavirus, but also the ones before.\

Ex.2 {#ex.2 .unnumbered}
====

` `

![image](figure/unnamed-chunk-4-1){width="2in"}

` `

![image](figure/unnamed-chunk-4-2){width="2in"}

    ## 
    ##  Box-Ljung test
    ## 
    ## data:  r
    ## X-squared = 37.913, df = 10, p-value = 3.93e-05

    ## AR/MA
    ##   0 1 2 3 4 5 6 7 8 9 10 11 12 13
    ## 0 x o o o o o o x o o o  o  o  o 
    ## 1 x o o o o o o o o o o  o  o  o 
    ## 2 x o x x o o o o o o x  o  o  o 
    ## 3 o x x o o o o o o o o  o  o  o 
    ## 4 x x x o o o o o o o o  o  o  o 
    ## 5 x o o x o o o o o o o  o  o  o 
    ## 6 x o x x x x o o o o o  o  o  o 
    ## 7 x x o x x x x o o o o  o  o  o

    ##            [,1]        [,2]        [,3]        [,4]        [,5]
    ## [1,] -0.2868861  0.11648731 -0.03535857 -0.08861711  0.01551432
    ## [2,]  0.1287697  0.03132215 -0.03723742 -0.10241349 -0.02603603
    ## [3,] -0.2038127  0.01625591 -0.14014160 -0.13432100  0.05181391
    ## [4,]  0.0703324  0.29773872 -0.28170036 -0.02965473 -0.05579571
    ## [5,] -0.3201994 -0.26084218 -0.20444399  0.02709464 -0.11390663

` `\

    ## Series: r 
    ## ARIMA(0,0,1) with non-zero mean 
    ## 
    ## Coefficients:
    ##           ma1    mean
    ##       -0.2582  0.4723
    ## s.e.   0.0566  0.2091
    ## 
    ## sigma^2 estimated as 20.91:  log likelihood=-769.08
    ## AIC=1544.17   AICc=1544.26   BIC=1554.87

` `\
\
\
\
` `\
\
` `\
\
` `\
\
` `\
\
\
\
\
\
`  `\
\
` `\
\
`  `\
`  `\
`  `\
`  `\

    ## [1] 2

`   `\

Firstly we plot the autocorrelation and the partial autocorrelation of
the returns, where we can easily see that in some lags the value are out
of the limits. Moreover from the Box.test we get a p-value very small,
and so we can say that they are not independent.\
Thanks to the sample extended acf, we get the possible orders for the
ARMA models. I’ve decided to analyze 9 models, although only few of them
are useful.\
The MA(1) results in AIC=1544.17 and BIC=1544.87.\
The MA of order superior to one don’t have significant values and they
can be reduced to the MA(1) case.\
The ARMA(1,2) and ARMA(2,1) have not significant values.\
The ARMA(2,2) has AIC=1545.13 and BIC=1566.54.\
The ARMA(3,3), with ar(1), ar(2), ma(1) and ma(2) fixed, has AIC=1563.21
and BIC=1577.49.\
The ARMA(4,3) get reduced to the MA(1) case.\
And so from this analysis we get that the MA(1) is the best ARMA model.\
After this we use the periodogram to get the seasonality and we test
the\
SARIMA(0,0,1)$(0,1,0)_2$, but also in this case the AIC and the BIC
result much bigger than in the MA(1).\

![image](figure/unnamed-chunk-6-1){width="2in"}

`  `\

![image](figure/unnamed-chunk-6-2){width="2in"}

![image](figure/unnamed-chunk-6-3){width="2in"}

![image](figure/unnamed-chunk-6-4){width="2in"}

![image](figure/unnamed-chunk-6-5){width="2in"}

Now we use the tsdiag to study our model and we see that we have some
autocorrelation on the residuals, and also the p-value for the Ljung-Box
test are very low in some lags.\
Then we see the plot of the acf and pacf of the residuals and of the
squared residuals and we see many autocorrelation on both analysis.

Ex.3 {#ex.3 .unnumbered}
====

From this lasts analysis we see that probably there is some ARCH effect
and so we are going to consider some ARCH and GARCH models for our
series.

`     `

    ## 
    ##  Box-Ljung test
    ## 
    ## data:  a^2
    ## X-squared = 122.26, df = 9, p-value < 2.2e-16

    ##          0          1          2          3          4          5 
    ## 126.314587 116.067001 106.778220  29.696336  22.702768  19.982141 
    ##          6          7          8          9         10         11 
    ##  14.737218  13.216422   0.000000   1.999050   1.318360   3.284279

Firstly we use the Box.test to confirm that there is strong ARCH EFFECT
on our residuals.\
Then we use the Akaike criterion to look for some order of our ARCH
models.\

`  `

\
`     `

\
\
\
\
\
\
\
\
\
\
\
\
`   `\
`   `\
`   `\
\
\

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(1, 1), data = a, include.mean = FALSE, 
    ##     trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(1, 1)
    ## <environment: 0x0000000022361a20>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##   omega   alpha1    beta1  
    ## 1.25430  0.32815  0.66341  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## omega    1.25430     0.54659    2.295  0.02174 *  
    ## alpha1   0.32815     0.08984    3.653  0.00026 ***
    ## beta1    0.66341     0.06569   10.099  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -705.8506    normalized:  -2.694086 
    ## 
    ## Description:
    ##  Fri May 14 10:48:42 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  903.0889  0           
    ##  Shapiro-Wilk Test  R    W      0.9075677 1.273637e-11
    ##  Ljung-Box Test     R    Q(10)  6.122245  0.8048903   
    ##  Ljung-Box Test     R    Q(15)  7.645099  0.937222    
    ##  Ljung-Box Test     R    Q(20)  9.70351   0.973235    
    ##  Ljung-Box Test     R^2  Q(10)  4.7521    0.9071109   
    ##  Ljung-Box Test     R^2  Q(15)  6.546966  0.9690251   
    ##  Ljung-Box Test     R^2  Q(20)  7.952876  0.9921749   
    ##  LM Arch Test       R    TR^2   4.891274  0.9615121   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.411073 5.451932 5.410815 5.427495

We now fit some Garch models.\
The first one we try is the GARCH(8,0) but many values are not
significant.\
For this reason we try GARCH(1,0), GARCH(1,1), GARCH(1,2) and
GARCH(2,1).\
The GARCH(1,2) and the GARCH(2,1) are characterized by the fact that
there are some not significant values, so we consider only the other
two.\
The GARCH(1,0) has AIC=5.559787 and BIC=5.587026.\
The GARCH(1,1) has AIC=5.411073 and BIC=5.451932.\
And so the best model is the GARCH(1,1). By using the summary function
we see that the Standardised Residuals are not correlated (and also the
squared standardised residuals).\

`  `\

![image](figure/unnamed-chunk-9-1){width="2in"}

![image](figure/unnamed-chunk-9-2){width="2in"}

![image](figure/unnamed-chunk-9-3){width="2in"}

    ##                std.res
    ## nobs        262.000000
    ## NAs           0.000000
    ## Minimum      -6.851764
    ## Maximum       3.023412
    ## 1. Quartile  -0.403155
    ## 3. Quartile   0.525711
    ## Mean         -0.033628
    ## Median        0.042208
    ## Sum          -8.810623
    ## SE Mean       0.061676
    ## LCL Mean     -0.155075
    ## UCL Mean      0.087818
    ## Variance      0.996639
    ## Stdev         0.998318
    ## Skewness     -1.401387
    ## Kurtosis      8.558773

\
\
\

    ## [1] 2

![image](figure/unnamed-chunk-9-4){width="2in"}

    ## [1] 200 138

From the analysis of the standardised residuals, and the squared one, we
can confirm that they are not autocorrelated.\
Moreover we see that the mean is near zero, the kurtosis is very high
(and this with the qqplot, tells us that the distribution is not normal,
it is similar to a t-student) and in the end, although from *basicStats*
we get a Skewness equal to $-1.4$, the test tells us that we can accept
the null hypothesis of skewness=0.\

`   `\

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~arma(0, 1) + garch(1, 1), data = r, cond.dist = "std", 
    ##     trace = FALSE) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ arma(0, 1) + garch(1, 1)
    ## <environment: 0x00000000216fe3f8>
    ##  [data = r]
    ## 
    ## Conditional Distribution:
    ##  std 
    ## 
    ## Coefficient(s):
    ##       mu       ma1     omega    alpha1     beta1     shape  
    ##  0.56999  -0.20012   1.56170   0.31279   0.68791   3.07509  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## mu       0.56999     0.12060    4.726 2.29e-06 ***
    ## ma1     -0.20012     0.06319   -3.167  0.00154 ** 
    ## omega    1.56170     1.00383    1.556  0.11977    
    ## alpha1   0.31279     0.17203    1.818  0.06903 .  
    ## beta1    0.68791     0.11645    5.908 3.47e-09 ***
    ## shape    3.07509     0.73865    4.163 3.14e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -675.58    normalized:  -2.57855 
    ## 
    ## Description:
    ##  Fri May 14 10:48:42 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  830.983   0           
    ##  Shapiro-Wilk Test  R    W      0.9093768 1.741811e-11
    ##  Ljung-Box Test     R    Q(10)  5.415468  0.8617548   
    ##  Ljung-Box Test     R    Q(15)  6.966806  0.9585643   
    ##  Ljung-Box Test     R    Q(20)  8.970082  0.9832512   
    ##  Ljung-Box Test     R^2  Q(10)  6.130428  0.8041887   
    ##  Ljung-Box Test     R^2  Q(15)  7.767244  0.9327747   
    ##  Ljung-Box Test     R^2  Q(20)  9.23876   0.9799732   
    ##  LM Arch Test       R    TR^2   6.330079  0.8985374   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.202901 5.284618 5.201883 5.235745

`   `\

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(1, 1), data = r, cond.dist = "std", 
    ##     trace = FALSE) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(1, 1)
    ## <environment: 0x00000000242172f0>
    ##  [data = r]
    ## 
    ## Conditional Distribution:
    ##  std 
    ## 
    ## Coefficient(s):
    ##      mu    omega   alpha1    beta1    shape  
    ## 0.55513  1.68435  0.38358  0.62965  3.24384  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## mu        0.5551      0.1470    3.776 0.000159 ***
    ## omega     1.6843      1.0563    1.595 0.110794    
    ## alpha1    0.3836      0.1875    2.046 0.040799 *  
    ## beta1     0.6297      0.1275    4.938 7.91e-07 ***
    ## shape     3.2438      0.7946    4.083 4.45e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -680.6081    normalized:  -2.597741 
    ## 
    ## Description:
    ##  Fri May 14 10:48:42 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  631.0779  0           
    ##  Shapiro-Wilk Test  R    W      0.9237155 2.440765e-10
    ##  Ljung-Box Test     R    Q(10)  7.880998  0.6404597   
    ##  Ljung-Box Test     R    Q(15)  9.927672  0.8242647   
    ##  Ljung-Box Test     R    Q(20)  12.62396  0.892929    
    ##  Ljung-Box Test     R^2  Q(10)  5.542505  0.8521254   
    ##  Ljung-Box Test     R^2  Q(15)  7.07131   0.9556406   
    ##  Ljung-Box Test     R^2  Q(20)  8.744693  0.9856801   
    ##  LM Arch Test       R    TR^2   6.247984  0.9030687   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.233650 5.301748 5.232939 5.261020

We fit the ARMA(0,1)+GARCH(1,1) model and we see that every value is
significant, the only one with an high p-value is alpha1 that has a
value equal to 0.069.\
Moreover we can look at the information criterion for this model,
AIC=5.202901 and BIC=5.284618.\
We try to fit also a GARCH(1,1), in this case every value is
significant, although alpha1 has still a p-value=0.04; but AIC=5.233650
and BIC=5.301748 and so we prefer the first model.\
In the end the model is: $$\begin{aligned}
X_t &= \mu_t +a_t=\phi_0 - \theta_1 a_{t-1}+ \sigma_t \epsilon_t =\phi_0 - \theta_1 a_{t-1}+ (\alpha_0+ \alpha_1 a^2_{t-1} + \beta_1 \sigma^2_{t-1}) \epsilon_t \\\end{aligned}$$
where $\epsilon_t$ are IID random variables with mean 0 and variance 1,
while $\sigma^2_t$ is the stochastic volatility.

`  `\

![image](figure/unnamed-chunk-11-1){width="2in"}

![image](figure/unnamed-chunk-11-2){width="2in"}

![image](figure/unnamed-chunk-11-3){width="2in"}

![image](figure/unnamed-chunk-11-4){width="2in"}

![image](figure/unnamed-chunk-11-5){width="2in"}

    ##                std.res
    ## nobs        262.000000
    ## NAs           0.000000
    ## Minimum      -6.290853
    ## Maximum       2.916395
    ## 1. Quartile  -0.432358
    ## 3. Quartile   0.427994
    ## Mean         -0.061931
    ## Median       -0.011305
    ## Sum         -16.225937
    ## SE Mean       0.057169
    ## LCL Mean     -0.174502
    ## UCL Mean      0.050639
    ## Variance      0.856282
    ## Stdev         0.925355
    ## Skewness     -1.303585
    ## Kurtosis      8.235089

\
\
\

    ## [1] 2

![image](figure/unnamed-chunk-11-6){width="2in"}

    ## [1] 200 138

As before we analyze the standardised residuals and we see that there is
no autocorrelation. While studying the squared of the standardised
residuals we can notice that at lag 3 we have some autocorrelation.\
Moreover we see that the mean is almost 0, the kurtosis is the very high
and we can accept the hypothesis that the skewness is equal to zero.\

Ex.4 {#ex.4 .unnumbered}
====

\

    ## [1] "dt.2.length.dt.." "std.res"          "r"

`    `\
\
`        `\
`        `\
`     `\
`      `<span>“`%m/%y`”</span>\

![image](figure/unnamed-chunk-12-1){width="2in"}

From this plot we can see 3 specific peaks on the estimated variance,
the same we saw in the first point.\
The first one is in october 2016, and in this month American Express
ended the partnership with Costco, one of the biggest retailer in the
world; the second one is in december 2018, caused by the America’s trade
war with China; and the last one in march 2020, due to Covid.\
Moreover we see that there is no leverage effect, big incremenents and
drops generate both a small increment in the volatility; we can confirm
this hypothesis throught the next plot:

\
\
\
\

![image](figure/unnamed-chunk-13-1){width="2in"}

This plot tells us there is no leverage effect and so the eGARCH is
useless.\
Moreover from the fact that we are studying the returns of a financial
multinational, probably also the Garch in the mean model is useless.\
Firstly we try to fit a GARCH in the Mean model:

`        `\
`                        `\
`                    `\
`   `\

    ## 
    ## *---------------------------------*
    ## *          GARCH Model Fit        *
    ## *---------------------------------*
    ## 
    ## Conditional Variance Dynamics    
    ## -----------------------------------
    ## GARCH Model  : sGARCH(1,1)
    ## Mean Model   : ARFIMA(0,0,1)
    ## Distribution : std 
    ## 
    ## Optimal Parameters
    ## ------------------------------------
    ##         Estimate  Std. Error   t value Pr(>|t|)
    ## mu      0.028675    0.399111  0.071847 0.942724
    ## ma1    -0.205765    0.062415 -3.296709 0.000978
    ## archm   0.164482    0.117597  1.398692 0.161905
    ## omega   1.363883    0.809179  1.685514 0.091890
    ## alpha1  0.289299    0.142941  2.023909 0.042980
    ## beta1   0.709701    0.096469  7.356744 0.000000
    ## shape   3.127505    0.740239  4.224993 0.000024
    ## 
    ## Robust Standard Errors:
    ##         Estimate  Std. Error   t value Pr(>|t|)
    ## mu      0.028675    0.425311  0.067421 0.946247
    ## ma1    -0.205765    0.053100 -3.875053 0.000107
    ## archm   0.164482    0.115575  1.423166 0.154688
    ## omega   1.363883    0.772482  1.765586 0.077465
    ## alpha1  0.289299    0.157567  1.836041 0.066352
    ## beta1   0.709701    0.098386  7.213440 0.000000
    ## shape   3.127505    0.688371  4.543341 0.000006
    ## 
    ## LogLikelihood : -675.0116 
    ## 
    ## Information Criteria
    ## ------------------------------------
    ##                    
    ## Akaike       5.2062
    ## Bayes        5.3015
    ## Shibata      5.2048
    ## Hannan-Quinn 5.2445
    ## 
    ## Weighted Ljung-Box Test on Standardized Residuals
    ## ------------------------------------
    ##                         statistic p-value
    ## Lag[1]                     0.1132  0.7365
    ## Lag[2*(p+q)+(p+q)-1][2]    0.3222  0.9925
    ## Lag[4*(p+q)+(p+q)-1][5]    1.0129  0.9449
    ## d.o.f=1
    ## H0 : No serial correlation
    ## 
    ## Weighted Ljung-Box Test on Standardized Squared Residuals
    ## ------------------------------------
    ##                         statistic p-value
    ## Lag[1]                     0.2641  0.6073
    ## Lag[2*(p+q)+(p+q)-1][5]    2.8591  0.4334
    ## Lag[4*(p+q)+(p+q)-1][9]    3.8460  0.6150
    ## d.o.f=2
    ## 
    ## Weighted ARCH LM Tests
    ## ------------------------------------
    ##             Statistic Shape Scale P-Value
    ## ARCH Lag[3]     4.023 0.500 2.000 0.04489
    ## ARCH Lag[5]     4.068 1.440 1.667 0.16791
    ## ARCH Lag[7]     4.213 2.315 1.543 0.31693
    ## 
    ## Nyblom stability test
    ## ------------------------------------
    ## Joint Statistic:  1.1344
    ## Individual Statistics:              
    ## mu     0.08930
    ## ma1    0.21048
    ## archm  0.06394
    ## omega  0.42072
    ## alpha1 0.23268
    ## beta1  0.47077
    ## shape  0.26190
    ## 
    ## Asymptotic Critical Values (10% 5% 1%)
    ## Joint Statistic:          1.69 1.9 2.35
    ## Individual Statistic:     0.35 0.47 0.75
    ## 
    ## Sign Bias Test
    ## ------------------------------------
    ##                    t-value   prob sig
    ## Sign Bias           1.2765 0.2029    
    ## Negative Sign Bias  0.2009 0.8410    
    ## Positive Sign Bias  0.2313 0.8173    
    ## Joint Effect        2.7303 0.4351    
    ## 
    ## 
    ## Adjusted Pearson Goodness-of-Fit Test:
    ## ------------------------------------
    ##   group statistic p-value(g-1)
    ## 1    20     21.66       0.3013
    ## 2    30     34.34       0.2272
    ## 3    40     42.73       0.3139
    ## 4    50     58.99       0.1552
    ## 
    ## 
    ## Elapsed time : 0.8267889

    ## [1] 0.08239704

We confirm again, from the Sign Bias test, that there is no leverage
effect; furthermore the archm estimate is not significant and the
correlation between the return and the fitted $\sigma^2$ is low, so we
can do not validate this model.\
Then we implement an eGARCH:

`        `\
`                      `\
`                    `\
`    `\

    ## 
    ## *---------------------------------*
    ## *          GARCH Model Fit        *
    ## *---------------------------------*
    ## 
    ## Conditional Variance Dynamics    
    ## -----------------------------------
    ## GARCH Model  : eGARCH(1,1)
    ## Mean Model   : ARFIMA(0,0,1)
    ## Distribution : std 
    ## 
    ## Optimal Parameters
    ## ------------------------------------
    ##         Estimate  Std. Error  t value Pr(>|t|)
    ## mu       0.49793    0.118957   4.1858 0.000028
    ## ma1     -0.18570    0.063243  -2.9362 0.003322
    ## omega    0.17360    0.104852   1.6556 0.097795
    ## alpha1  -0.20665    0.081937  -2.5221 0.011666
    ## beta1    0.92859    0.039769  23.3495 0.000000
    ## gamma1   0.36290    0.130569   2.7794 0.005446
    ## shape    3.72468    1.021155   3.6475 0.000265
    ## 
    ## Robust Standard Errors:
    ##         Estimate  Std. Error  t value Pr(>|t|)
    ## mu       0.49793    0.120919   4.1179 0.000038
    ## ma1     -0.18570    0.055543  -3.3433 0.000828
    ## omega    0.17360    0.103763   1.6730 0.094325
    ## alpha1  -0.20665    0.083841  -2.4648 0.013709
    ## beta1    0.92859    0.040382  22.9952 0.000000
    ## gamma1   0.36290    0.112952   3.2129 0.001314
    ## shape    3.72468    1.066190   3.4934 0.000477
    ## 
    ## LogLikelihood : -672.1034 
    ## 
    ## Information Criteria
    ## ------------------------------------
    ##                    
    ## Akaike       5.1840
    ## Bayes        5.2793
    ## Shibata      5.1826
    ## Hannan-Quinn 5.2223
    ## 
    ## Weighted Ljung-Box Test on Standardized Residuals
    ## ------------------------------------
    ##                         statistic p-value
    ## Lag[1]                    0.01713  0.8959
    ## Lag[2*(p+q)+(p+q)-1][2]   0.46357  0.9723
    ## Lag[4*(p+q)+(p+q)-1][5]   1.07668  0.9347
    ## d.o.f=1
    ## H0 : No serial correlation
    ## 
    ## Weighted Ljung-Box Test on Standardized Squared Residuals
    ## ------------------------------------
    ##                         statistic p-value
    ## Lag[1]                     0.4124  0.5208
    ## Lag[2*(p+q)+(p+q)-1][5]    0.6428  0.9336
    ## Lag[4*(p+q)+(p+q)-1][9]    1.0449  0.9843
    ## d.o.f=2
    ## 
    ## Weighted ARCH LM Tests
    ## ------------------------------------
    ##             Statistic Shape Scale P-Value
    ## ARCH Lag[3]    0.2808 0.500 2.000  0.5962
    ## ARCH Lag[5]    0.4126 1.440 1.667  0.9091
    ## ARCH Lag[7]    0.6955 2.315 1.543  0.9574
    ## 
    ## Nyblom stability test
    ## ------------------------------------
    ## Joint Statistic:  1.6666
    ## Individual Statistics:              
    ## mu     0.31197
    ## ma1    0.20848
    ## omega  0.58626
    ## alpha1 0.27231
    ## beta1  0.64962
    ## gamma1 0.07275
    ## shape  0.12126
    ## 
    ## Asymptotic Critical Values (10% 5% 1%)
    ## Joint Statistic:          1.69 1.9 2.35
    ## Individual Statistic:     0.35 0.47 0.75
    ## 
    ## Sign Bias Test
    ## ------------------------------------
    ##                    t-value   prob sig
    ## Sign Bias          1.39095 0.1654    
    ## Negative Sign Bias 0.80079 0.4240    
    ## Positive Sign Bias 0.04248 0.9662    
    ## Joint Effect       2.40663 0.4924    
    ## 
    ## 
    ## Adjusted Pearson Goodness-of-Fit Test:
    ## ------------------------------------
    ##   group statistic p-value(g-1)
    ## 1    20     13.88       0.7908
    ## 2    30     24.95       0.6810
    ## 3    40     28.08       0.9029
    ## 4    50     36.09       0.9148
    ## 
    ## 
    ## Elapsed time : 0.621335

From this analysis the eGARCH model seems pretty good, the gamma and the
alpha are significant, but we know that there is no leverage, in fact
the Sign Bias test shows a p-value equal to $0.49$ in the joint effect.
For this reason, probably, the joint significance of gamma and alpha
would take us a p-value greater than 0.05, and so, also this model is
useless.\

Appendix Code {#appendix-code .unnumbered}
=============

    library(ggplot2)
    library(TSA)
    library(forecast)
    library(fUnitRoots)
    library(fGarch)
    library(rugarch)
    library(car)

    AXP=read.csv("AXP.csv",sep=",");
    AXP=data.frame(AXP$Date ,AXP$Adj.Close)

    dt=as.Date(AXP$AXP.Date, format="%Y-%m-%d");
    X=AXP$AXP.Adj.Close
    AXPg= ggplot(AXP, aes(x = dt, y = X)) + geom_line() +
      labs(title="Weekly adjusted closure of American Express,
           from 25/04/2016 to 29/04/2021", x="Date", y="Value")+
      scale_x_date(breaks = pretty(dt,n=10),date_labels = "%m/%y");
    AXPg
    T = length(dt);
    T

    ar(X)$aic
    adfTest(X,lags=2,type="nc")@test$p.value #no stationarity 

    r = ((X[2:T]-X[1:(T-1)])/X[1:(T-1)])*100
    ar(r)$aic
    adfTest(r,lags=1,type="nc")@test$p.value #stationarity
    plot(r)

    e=eacf(r)
    e$eacf[1:5,1:5]

    M0=Arima(r, order=c(0,0,1))
    M0
    M1=Arima(r,order =c(0,0,2))
    M1
    M2=Arima(r,order=c(0,0,3))
    M2
    M3=Arima(r, order=c(0,0,4))
    M3
    M4=Arima(r, order=c(1,0,1))
    M4
    M5=Arima(r, order=c(2,0,1))
    M5
    M6=Arima(r, order=c(1,0,2))
    M6
    M7=Arima(r,order=c(2,0,2))
    M7
    M8=Arima(r,order=c(3,0,3))
    M8
    M8=Arima(r, order=c(3,0,3), fixed=c(0,0,NA,0,0,NA,NA))
    M8
    M9=Arima(r, order=c(4,0,3))
    M9 as MA(3)
    prd = periodogram(r);
    ord = order(-prd$spec);
    d = cbind(prd$freq[ord],prd$spec[ord]);
    period1 = round(1/d[1,1])
    MS=Arima(r, order=c(0,0,1), seasonal=list(order= c(0,1,0),period=2))
    MS

    tsdiag(M0)

    a = M0$residuals;
    acf(a)
    pacf(a)

    acf(a^2)
    pacf(a^2)

    Box.test(a^2 ,lag = 10,fitdf=1,type = "Ljung")
    ar(a^2)$aic

`  `

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(8, 0), data = a, trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(8, 0)
    ## <environment: 0x000000001a383e58>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##          mu        omega       alpha1       alpha2       alpha3  
    ## -0.02439766   1.78255418   0.05239698   0.35739431   0.21953460  
    ##      alpha4       alpha5       alpha6       alpha7       alpha8  
    ##  0.02650875   0.27985990   0.33102241   0.00000001   0.00000001  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##          Estimate  Std. Error  t value Pr(>|t|)    
    ## mu     -2.440e-02   1.403e-01   -0.174 0.861993    
    ## omega   1.783e+00   7.247e-01    2.460 0.013903 *  
    ## alpha1  5.240e-02   5.467e-02    0.958 0.337832    
    ## alpha2  3.574e-01   9.714e-02    3.679 0.000234 ***
    ## alpha3  2.195e-01   8.437e-02    2.602 0.009263 ** 
    ## alpha4  2.651e-02   8.588e-02    0.309 0.757565    
    ## alpha5  2.799e-01   2.359e-01    1.186 0.235495    
    ## alpha6  3.310e-01   2.241e-01    1.477 0.139591    
    ## alpha7  1.000e-08          NA       NA       NA    
    ## alpha8  1.000e-08   3.135e-02    0.000 1.000000    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -697.7882    normalized:  -2.663314 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  253.0591  0           
    ##  Shapiro-Wilk Test  R    W      0.9432167 1.561448e-08
    ##  Ljung-Box Test     R    Q(10)  5.335714  0.867654    
    ##  Ljung-Box Test     R    Q(15)  8.839635  0.885752    
    ##  Ljung-Box Test     R    Q(20)  11.23911  0.9397844   
    ##  Ljung-Box Test     R^2  Q(10)  5.358662  0.8659683   
    ##  Ljung-Box Test     R^2  Q(15)  9.582052  0.8451744   
    ##  Ljung-Box Test     R^2  Q(20)  13.39136  0.8599683   
    ##  LM Arch Test       R    TR^2   5.580412  0.9357406   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.402963 5.539160 5.400190 5.457703

`     `

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(8, 0), data = a, include.mean = FALSE, 
    ##     trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(8, 0)
    ## <environment: 0x0000000022977d68>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##      omega      alpha1      alpha2      alpha3      alpha4      alpha5  
    ## 1.77073069  0.05184915  0.35882223  0.21994016  0.03027948  0.27485078  
    ##     alpha6      alpha7      alpha8  
    ## 0.33432856  0.00000001  0.00000001  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## omega  1.771e+00   7.353e-01    2.408 0.016031 *  
    ## alpha1 5.185e-02   5.414e-02    0.958 0.338219    
    ## alpha2 3.588e-01   1.064e-01    3.371 0.000749 ***
    ## alpha3 2.199e-01   8.484e-02    2.592 0.009530 ** 
    ## alpha4 3.028e-02   8.460e-02    0.358 0.720410    
    ## alpha5 2.749e-01   2.295e-01    1.198 0.231071    
    ## alpha6 3.343e-01   2.248e-01    1.487 0.136937    
    ## alpha7 1.000e-08          NA       NA       NA    
    ## alpha8 1.000e-08   3.100e-02    0.000 1.000000    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -697.8562    normalized:  -2.663573 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  259.686   0           
    ##  Shapiro-Wilk Test  R    W      0.9427423 1.397567e-08
    ##  Ljung-Box Test     R    Q(10)  5.371997  0.8649844   
    ##  Ljung-Box Test     R    Q(15)  8.873214  0.8840528   
    ##  Ljung-Box Test     R    Q(20)  11.26041  0.9391876   
    ##  Ljung-Box Test     R^2  Q(10)  5.361909  0.8657291   
    ##  Ljung-Box Test     R^2  Q(15)  9.51537   0.8490677   
    ##  Ljung-Box Test     R^2  Q(20)  13.28052  0.8650262   
    ##  LM Arch Test       R    TR^2   5.570889  0.9361519   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.395849 5.518425 5.393591 5.445115

\
\
\
\
\

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(1, 0), data = a, trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(1, 0)
    ## <environment: 0x000000001a8ff950>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##       mu     omega    alpha1  
    ## -0.00241   9.10895   0.74819  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## mu      -0.00241     0.20919   -0.012    0.991    
    ## omega    9.10895     1.19330    7.633 2.29e-14 ***
    ## alpha1   0.74819     0.19087    3.920 8.86e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -726.3322    normalized:  -2.77226 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value   
    ##  Jarque-Bera Test   R    Chi^2  809.14    0         
    ##  Shapiro-Wilk Test  R    W      0.8941403 1.4049e-12
    ##  Ljung-Box Test     R    Q(10)  8.564588  0.5738625 
    ##  Ljung-Box Test     R    Q(15)  9.732611  0.8362138 
    ##  Ljung-Box Test     R    Q(20)  13.03605  0.8758321 
    ##  Ljung-Box Test     R^2  Q(10)  10.46517  0.4006728 
    ##  Ljung-Box Test     R^2  Q(15)  15.59133  0.4097194 
    ##  Ljung-Box Test     R^2  Q(20)  16.59177  0.6793001 
    ##  LM Arch Test       R    TR^2   12.62723  0.3967069 
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.567421 5.608280 5.567163 5.583843

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(1, 1), data = a, trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(1, 1)
    ## <environment: 0x000000001f761070>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##        mu      omega     alpha1      beta1  
    ## -0.024398   1.244625   0.328162   0.664136  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## mu      -0.02440     0.18385   -0.133 0.894430    
    ## omega    1.24462     0.54846    2.269 0.023249 *  
    ## alpha1   0.32816     0.08966    3.660 0.000252 ***
    ## beta1    0.66414     0.06560   10.125  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -705.8406    normalized:  -2.694048 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  896.3755  0           
    ##  Shapiro-Wilk Test  R    W      0.9076136 1.283724e-11
    ##  Ljung-Box Test     R    Q(10)  6.11056   0.8058905   
    ##  Ljung-Box Test     R    Q(15)  7.637367  0.9374972   
    ##  Ljung-Box Test     R    Q(20)  9.713523  0.973074    
    ##  Ljung-Box Test     R^2  Q(10)  4.804201  0.9038678   
    ##  Ljung-Box Test     R^2  Q(15)  6.621983  0.9673037   
    ##  Ljung-Box Test     R^2  Q(20)  8.042162  0.9915851   
    ##  LM Arch Test       R    TR^2   4.948176  0.9596875   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.418631 5.473109 5.418174 5.440527

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(2, 1), data = a, trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(2, 1)
    ## <environment: 0x0000000021042f90>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##        mu      omega     alpha1     alpha2      beta1  
    ## -0.024398   1.253800   0.053271   0.380857   0.590525  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## mu      -0.02440     0.17127   -0.142 0.886724    
    ## omega    1.25380     0.65121    1.925 0.054188 .  
    ## alpha1   0.05327     0.05516    0.966 0.334166    
    ## alpha2   0.38086     0.11550    3.297 0.000976 ***
    ## beta1    0.59052     0.08307    7.109 1.17e-12 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -700.8678    normalized:  -2.675068 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  452.5637  0           
    ##  Shapiro-Wilk Test  R    W      0.9286195 6.478927e-10
    ##  Ljung-Box Test     R    Q(10)  4.657571  0.9128485   
    ##  Ljung-Box Test     R    Q(15)  7.024892  0.9569553   
    ##  Ljung-Box Test     R    Q(20)  9.13607   0.981277    
    ##  Ljung-Box Test     R^2  Q(10)  4.390797  0.9280004   
    ##  Ljung-Box Test     R^2  Q(15)  6.750868  0.9641978   
    ##  Ljung-Box Test     R^2  Q(20)  8.72706   0.9858583   
    ##  LM Arch Test       R    TR^2   4.740515  0.9660891   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.388304 5.456402 5.387593 5.415674

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(1, 2), data = a, trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(1, 2)
    ## <environment: 0x0000000021faefc8>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##          mu        omega       alpha1        beta1        beta2  
    ## -0.02194830   1.22837566   0.32622772   0.66633717   0.00000001  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##          Estimate  Std. Error  t value Pr(>|t|)   
    ## mu     -2.195e-02   1.853e-01   -0.118    0.906   
    ## omega   1.228e+00   7.484e-01    1.641    0.101   
    ## alpha1  3.262e-01   1.134e-01    2.878    0.004 **
    ## beta1   6.663e-01   4.238e-01    1.572    0.116   
    ## beta2   1.000e-08   3.258e-01    0.000    1.000   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -706.1101    normalized:  -2.695077 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  906.4831  0           
    ##  Shapiro-Wilk Test  R    W      0.907348  1.226453e-11
    ##  Ljung-Box Test     R    Q(10)  6.167934  0.8009623   
    ##  Ljung-Box Test     R    Q(15)  7.704434  0.9350849   
    ##  Ljung-Box Test     R    Q(20)  9.788033  0.9718546   
    ##  Ljung-Box Test     R^2  Q(10)  4.823751  0.9026362   
    ##  Ljung-Box Test     R^2  Q(15)  6.629475  0.9671283   
    ##  Ljung-Box Test     R^2  Q(20)  8.060742  0.9914582   
    ##  LM Arch Test       R    TR^2   4.943231  0.9598482   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.428321 5.496420 5.427611 5.455692

`   `\
`   `\
`   `\
\

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(1, 0), data = a, include.mean = FALSE, 
    ##     trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(1, 0)
    ## <environment: 0x0000000023a87020>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##   omega   alpha1  
    ## 9.10853  0.74834  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## omega     9.1085      1.1931    7.634 2.26e-14 ***
    ## alpha1    0.7483      0.1906    3.926 8.63e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -726.3321    normalized:  -2.77226 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  809.2197  0           
    ##  Shapiro-Wilk Test  R    W      0.8941458 1.406098e-12
    ##  Ljung-Box Test     R    Q(10)  8.563131  0.5740034   
    ##  Ljung-Box Test     R    Q(15)  9.730638  0.8363327   
    ##  Ljung-Box Test     R    Q(20)  13.03387  0.8759262   
    ##  Ljung-Box Test     R^2  Q(10)  10.45763  0.4013012   
    ##  Ljung-Box Test     R^2  Q(15)  15.58701  0.410017    
    ##  Ljung-Box Test     R^2  Q(20)  16.58585  0.6796793   
    ##  LM Arch Test       R    TR^2   12.62442  0.3969195   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.559787 5.587026 5.559671 5.570735

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(1, 1), data = a, include.mean = FALSE, 
    ##     trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(1, 1)
    ## <environment: 0x0000000022d2e6e0>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##   omega   alpha1    beta1  
    ## 1.25430  0.32815  0.66341  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## omega    1.25430     0.54659    2.295  0.02174 *  
    ## alpha1   0.32815     0.08984    3.653  0.00026 ***
    ## beta1    0.66341     0.06569   10.099  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -705.8506    normalized:  -2.694086 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  903.0889  0           
    ##  Shapiro-Wilk Test  R    W      0.9075677 1.273637e-11
    ##  Ljung-Box Test     R    Q(10)  6.122245  0.8048903   
    ##  Ljung-Box Test     R    Q(15)  7.645099  0.937222    
    ##  Ljung-Box Test     R    Q(20)  9.70351   0.973235    
    ##  Ljung-Box Test     R^2  Q(10)  4.7521    0.9071109   
    ##  Ljung-Box Test     R^2  Q(15)  6.546966  0.9690251   
    ##  Ljung-Box Test     R^2  Q(20)  7.952876  0.9921749   
    ##  LM Arch Test       R    TR^2   4.891274  0.9615121   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.411073 5.451932 5.410815 5.427495

    ## 
    ## Title:
    ##  GARCH Modelling 
    ## 
    ## Call:
    ##  garchFit(formula = ~garch(2, 1), data = a, include.mean = FALSE, 
    ##     trace = F) 
    ## 
    ## Mean and Variance Equation:
    ##  data ~ garch(2, 1)
    ## <environment: 0x000000001d9efe78>
    ##  [data = a]
    ## 
    ## Conditional Distribution:
    ##  norm 
    ## 
    ## Coefficient(s):
    ##    omega    alpha1    alpha2     beta1  
    ## 1.249975  0.053667  0.379341  0.591733  
    ## 
    ## Std. Errors:
    ##  based on Hessian 
    ## 
    ## Error Analysis:
    ##         Estimate  Std. Error  t value Pr(>|t|)    
    ## omega    1.24998     0.64863    1.927 0.053968 .  
    ## alpha1   0.05367     0.05519    0.972 0.330832    
    ## alpha2   0.37934     0.11452    3.312 0.000925 ***
    ## beta1    0.59173     0.08169    7.244 4.35e-13 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log Likelihood:
    ##  -700.9209    normalized:  -2.675271 
    ## 
    ## Description:
    ##  Fri May 14 10:48:45 2021 by user: Mattia 
    ## 
    ## 
    ## Standardised Residuals Tests:
    ##                                 Statistic p-Value     
    ##  Jarque-Bera Test   R    Chi^2  460.7408  0           
    ##  Shapiro-Wilk Test  R    W      0.9282345 5.991823e-10
    ##  Ljung-Box Test     R    Q(10)  4.683834  0.9112735   
    ##  Ljung-Box Test     R    Q(15)  7.042945  0.9564471   
    ##  Ljung-Box Test     R    Q(20)  9.134435  0.9812972   
    ##  Ljung-Box Test     R^2  Q(10)  4.337587  0.9308355   
    ##  Ljung-Box Test     R^2  Q(15)  6.636318  0.9669675   
    ##  Ljung-Box Test     R^2  Q(20)  8.590144  0.9871866   
    ##  LM Arch Test       R    TR^2   4.668996  0.9681315   
    ## 
    ## Information Criterion Statistics:
    ##      AIC      BIC      SIC     HQIC 
    ## 5.381076 5.435554 5.380619 5.402972

    std.res = m3ARCH@residuals/m3ARCH@sigma.t
    plot(std.res,type="l")
    acf(std.res)
    acf(std.res^2)

    basicStats(std.res)
    s1=skewness(std.res)[1]
    t1=s1/sqrt(6/length(std.res))
    pv=2*(1-pnorm(t1))
    pv 
    qqPlot(std.res)

    m = garchFit(~arma(0,1)+garch(1,1),data = r,trace=FALSE,cond.dist="std")
    summary(m)
    m1 = garchFit(~garch(1,1),data = r,trace=FALSE,cond.dist="std")
    summary(m1)

    std.res = m@residuals/m@sigma.t
    plot(std.res)
    acf(std.res)
    pacf(std.res)
    acf(std.res^2)
    pacf(std.res^2)

    basicStats(std.res)
    s1=skewness(std.res)[1]
    t1=s1/sqrt(6/length(std.res))
    pv=2*(1-pnorm(t1))
    pv 
    qqPlot(std.res)

    Model=data.frame(dt[2:length(dt)], std.res, r)
    colnames(Model)
    names(Model)[names(Model) == "dt.2.length.dt.."] <- "dt"

    AXPg= ggplot(Model, aes(x = dt, y = std.res^2)) + geom_point() +
      geom_line(data=Model, aes(x = dt, y = r), col="red") +
      labs(title="Estimated Variance and returns", x="Months", y="Value") +
      scale_x_date(breaks = pretty(dt,n=10),date_labels = "%m / %Y");
    AXPg

    plot(m@sigma.t[2:T],m@residuals[-T],xlab="Fitted Volatility",
                    ylab="Previous Return")
    abline(h=0,col="green")
    abline(h=10,col="orange")
    abline(h=-10,col="orange")
    abline(v=10,col="red")

    spec = ugarchspec(variance.model = list(model = "sGARCH",garchOrder = c(1,1)),
                      mean.model = list(armaOrder = c(0,1),archm = TRUE),
                      distribution.model = "std");
    GARCHM = ugarchfit(r,spec = spec);
    GARCHM
    cor(r,GARCHM@fit$sigma^2)

    spec = ugarchspec(variance.model = list(model = "eGARCH",garchOrder = c(1,1)),
                      mean.model = list(armaOrder = c(0,1)),
                      distribution.model = "std");
    eGARCH = ugarchfit(r,spec = spec, solver="hybrid")
    eGARCH
