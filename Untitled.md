
I analyzed several datasets before choosing this one, but they all turned out to be white noise. Later I remembered what happened to Gamestop actions last month, so I decided to analyze the adjusted closure of the latter. 
I consider a daily analysis and picked a whole year, as time window, to get enough data.


```R
library(ggplot2)
library(TSA)
library(forecast)

GME=read.csv("GME.csv",sep=",");
GME=data.frame(GME$Date,GME$Adj.Close)
dt=as.Date(GME$GME.Date, format="%Y-%m-%d");
X=GME$GME.Adj.Close
```

    Warning message:
    "package 'ggplot2' was built under R version 3.6.3"Warning message:
    "package 'TSA' was built under R version 3.6.3"
    Attaching package: 'TSA'
    
    The following objects are masked from 'package:stats':
    
        acf, arima
    
    The following object is masked from 'package:utils':
    
        tar
    
    Warning message:
    "package 'forecast' was built under R version 3.6.3"Registered S3 method overwritten by 'quantmod':
      method            from
      as.zoo.data.frame zoo 
    Registered S3 methods overwritten by 'forecast':
      method       from
      fitted.Arima TSA 
      plot.Arima   TSA 
    


```R
GMEg= ggplot(GME, aes(x = dt, y = X)) + geom_line() +
  labs(title="Adjusted Closure of GME in the last year", x="Months", y="Value") +
  scale_x_date(breaks = pretty(dt,n=10),date_labels = "%m");
GMEg
```


![png](output_2_0.png)


From the plot we can see that the progression for most of time is stable, from semptember it start growning a bit, untile january where we can see a big spike, and after that it starts moving a lot up and down.


```R
T = length(dt);
r = ((X[2:T]-X[1:(T-1)])/X[1:(T-1)])*100
plot(r)
```


![png](output_4_0.png)


Here as before we can see that most of time the simple percentage return is near zero. We can see an exception on the last days of the study where it diverges a lot.


```R
x = diff(log(X))*100
plot(x)
```


![png](output_6_0.png)


The log return percentage is very similar to the simple one.


```R
library(fBasics)
hist(r)
```


![png](output_8_0.png)



```R
basicStats(r)
s1 = skewness(r)[1]; s1
t1 = s1/sqrt(6/length(r))
pv = 2*(1-pnorm(t1))
pv
t.test(x)
normalTest(x,method="jb")
```


<table class="dataframe">
<caption>A data.frame: 16 × 1</caption>
<thead>
	<tr><th></th><th scope=col>r</th></tr>
	<tr><th></th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>nobs</th><td>250.000000</td></tr>
	<tr><th scope=row>NAs</th><td>  0.000000</td></tr>
	<tr><th scope=row>Minimum</th><td>-60.000000</td></tr>
	<tr><th scope=row>Maximum</th><td>134.835802</td></tr>
	<tr><th scope=row>1. Quartile</th><td> -3.347393</td></tr>
	<tr><th scope=row>3. Quartile</th><td>  4.757696</td></tr>
	<tr><th scope=row>Mean</th><td>  2.832840</td></tr>
	<tr><th scope=row>Median</th><td>  0.195741</td></tr>
	<tr><th scope=row>Sum</th><td>708.209952</td></tr>
	<tr><th scope=row>SE Mean</th><td>  1.108859</td></tr>
	<tr><th scope=row>LCL Mean</th><td>  0.648901</td></tr>
	<tr><th scope=row>UCL Mean</th><td>  5.016779</td></tr>
	<tr><th scope=row>Variance</th><td>307.392192</td></tr>
	<tr><th scope=row>Stdev</th><td> 17.532604</td></tr>
	<tr><th scope=row>Skewness</th><td>  3.234629</td></tr>
	<tr><th scope=row>Kurtosis</th><td> 20.206332</td></tr>
</tbody>
</table>




3.2346288400622



0



    
    	One Sample t-test
    
    data:  x
    t = 1.6369, df = 249, p-value = 0.1029
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
     -0.3215156  3.4854378
    sample estimates:
    mean of x 
     1.581961 
    



    
    Title:
     Jarque - Bera Normalality Test
    
    Test Results:
      STATISTIC:
        X-squared: 1734.6067
      P VALUE:
        Asymptotic p Value: < 2.2e-16 
    
    Description:
     Sun Jan 23 17:37:21 2022 by user: Mattia
    


We can easily see, from the result of the function basicStats, that we the skewness is not zero and that the kurtosis is much bigger that 3. One particular thing, that we can notice from this study, other than the fact that our distribution is not normal having p-value equal to $2.2e-16$, is that the mean is not significally different from zero. This is mainly because, although the mean is equal to 2.832840, the variance is very high.\\
We did this by considering as test for skewness: the null hypothesis equal to $S=0$ and the test statistic: $t=\frac{\hat{S}}{\sqrt{6/T}}\sim \textit{N}(0,1)$.



```R
acf(r); #autocorrelation function
```


![png](output_11_0.png)



```R
pacf(r); #partial autocorrelation function
```


![png](output_12_0.png)



```R
Box.test(r,lag = log(length(r)),type="Ljung-Box")
```


    
    	Box-Ljung test
    
    data:  r
    X-squared = 33.084, df = 5.5215, p-value = 6.269e-06
    


We have already the idea that our return are not white noise, we use this this analysis to confirm it.\\
We see that in both graphics there are some spikes, more precisely, in the pacf's graph, we see that we have the higher values in 2,3,4 and 10, so we will use this lags for our future analysis.\\
Finally we look at the results from the Box.test to confirm that our data are not White Noise.\\

Firstly we use the Akaike's information criterior to choose the number of parameters for our autoregressive model, then we find the characteristic roots of the equation.


```R
ar(r)$aic
M=Arima(r,order = c(10,0,0))
Arima(r,order = c(4,0,0))

roots = polyroot(c(1,-M$coef[1:10])) 
croots = 1/roots
Mod(croots)
```


<style>
.dl-inline {width: auto; margin:0; padding: 0}
.dl-inline>dt, .dl-inline>dd {float: none; width: auto; display: inline-block}
.dl-inline>dt::after {content: ":\0020"; padding-right: .5ex}
.dl-inline>dt:not(:first-of-type) {padding-left: .5ex}
</style><dl class=dl-inline><dt>0</dt><dd>32.1232690673944</dd><dt>1</dt><dd>32.743087818983</dd><dt>2</dt><dd>28.187781179015</dd><dt>3</dt><dd>22.0529702244726</dd><dt>4</dt><dd>0.0647341602300457</dd><dt>5</dt><dd>2.02691160555992</dd><dt>6</dt><dd>3.73155885533311</dd><dt>7</dt><dd>5.46344913088046</dd><dt>8</dt><dd>4.63953766485861</dd><dt>9</dt><dd>4.05553831230645</dd><dt>10</dt><dd>0</dd><dt>11</dt><dd>1.10013775465586</dd><dt>12</dt><dd>3.10012213427353</dd><dt>13</dt><dd>3.83966095178653</dd><dt>14</dt><dd>5.21920531331602</dd><dt>15</dt><dd>6.08313074590455</dd><dt>16</dt><dd>4.8417141506236</dd><dt>17</dt><dd>5.35484336443506</dd><dt>18</dt><dd>7.12759306421594</dd><dt>19</dt><dd>6.91401615364998</dd><dt>20</dt><dd>6.50799639904699</dd><dt>21</dt><dd>6.80794438832572</dd><dt>22</dt><dd>8.45961564506365</dd><dt>23</dt><dd>10.3852452670158</dd></dl>




    Series: r 
    ARIMA(4,0,0) with non-zero mean 
    
    Coefficients:
             ar1     ar2     ar3      ar4    mean
          0.0897  0.1973  0.1898  -0.3033  2.8572
    s.e.  0.0601  0.0593  0.0592   0.0602  1.2349
    
    sigma^2 estimated as 265.3:  log likelihood=-1050.09
    AIC=2112.17   AICc=2112.52   BIC=2133.3



<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>0.860536475476584</li><li>0.894471581701726</li><li>0.894471581701726</li><li>0.860536475476584</li><li>0.841992591467478</li><li>0.841098786829885</li><li>0.841992591467478</li><li>0.760171346936879</li><li>0.76017134693688</li><li>0.841098786829885</li></ol>



We see that the number of our parameter is 10 and that the modulus of our characteristic roots is always smaller than 1, so our series is stationary. In the end we analyse the residuals.\\ 


```R
res = M$residuals
plot(res)
```


![png](output_18_0.png)



```R
acf(res)
```


![png](output_19_0.png)



```R
Box.test(res,lag = 25, fitdf=10)
tsdiag(M)
normalTest(res,method="jb")
```


    
    	Box-Pierce test
    
    data:  res
    X-squared = 17.451, df = 15, p-value = 0.2926
    



    
    Title:
     Jarque - Bera Normalality Test
    
    Test Results:
      STATISTIC:
        X-squared: 2331.6508
      P VALUE:
        Asymptotic p Value: < 2.2e-16 
    
    Description:
     Sun Jan 23 17:38:19 2022 by user: Mattia
    



![png](output_20_2.png)


We can see that they are not a serially correlated sequence, and they not normally distributed.

To conclude the project, we will use the function predict to forecast our series.


```R
T=length(r)
t=T-10
Fr=r[1:t]
FM=Arima(Fr,order = c(10,0,0))
P = predict(FM,10)                           
ls.str(P)                                      
plot(c(Fr,P$pred))
points(c(rep(NA,240),P$pred),col="red")
```


    pred :  Time-Series [1:10] from 241 to 250: 0.326 -2 -1.123 6.056 2.85 ...
    se :  Time-Series [1:10] from 241 to 250: 15.7 15.8 16.2 16.8 17 ...



![png](output_23_1.png)



```R
MR=max(r[t:T])
mR=min(r[t:T])

k = 30;
plot(r[(T-k):T],ylim=c(mR-1,MR+1))
points(c(rep(NA,k-10+1),P$pred),col="red")
lines(c(rep(NA,k-10),x[T],P$pred+1.96*P$se),col="red",lwd=2,lty=2)
lines(c(rep(NA,k-10),x[T],P$pred-1.96*P$se),col="red",lwd=2,lty=2)
```


![png](output_24_0.png)


In the end we can see that, except from the first two data, the other one are similar and inside the prediction interval of 95\%. \\
I don't think that this prediction model is good. It is good when the behaviour is constant but when the returns get strange value then the prediction is completely wrong


```R

```
