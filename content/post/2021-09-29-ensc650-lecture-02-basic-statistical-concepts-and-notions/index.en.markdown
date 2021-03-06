---
title: 'ENSC650 lecture 02: Basic Statistical Concepts and Notions'
author: Akihiko Mori
date: '2021-09-29'
slug: []
categories: []
tags:
  - R
  - NRES
---

# Statistical Analysis
to discover relationships and features in given dataset

# Statistic types
* Descriptive
* Inference
* rational

# Statistical Anaysis
* Descriptive
* Diagnostic/Inference
* Predictive

## Panel Data
a dataset in which the behavior of entities are observed across time
## Cross sectional Data
a dataset collected by observing many subjects at the one point or period of time
## Time Series Data
a dataset collected at different points in time

# Line graph
for trend of variable over time 

```r
t=0:10 # time point
z= exp(-t/2) # a variable
w = 0.1*exp(t/3) # second variable
plot(t,z, type="l", col="green", lwd=5, xlab="time", ylab="concentration")
lines(t, w, col="red", lwd=2)
title("Exponential growth and decay")
legend(2,1,c("decay","growth"), lwd=c(5,2), col=c("green","red"), y.intersp=1.5)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-1-1.png" width="672" />

# Bar graph
summarize a set of categorical data

```r
# Simple Bar Plot 
counts <- table(mtcars$gear)
barplot(counts, main="Car Distribution", 
   xlab="Number of Gears")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-2-1.png" width="672" />

```r
# Simple Horizontal Bar Plot with Added Labels 
counts <- table(mtcars$gear)
barplot(counts, main="Car Distribution", horiz=TRUE,
  names.arg=c("3 Gears", "4 Gears", "5 Gears"))
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-2-2.png" width="672" />

```r
# Stacked Bar Plot with Colors and Legend
counts <- table(mtcars$vs, mtcars$gear)
barplot(counts, main="Car Distribution by Gears and VS",
  xlab="Number of Gears", col=c("darkblue","red"),
  legend = rownames(counts))
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-2-3.png" width="672" />

```r
# Grouped Bar Plot
counts <- table(mtcars$vs, mtcars$gear)
barplot(counts, main="Car Distribution by Gears and VS",
  xlab="Number of Gears", col=c("darkblue","red"),
  legend = rownames(counts), beside=TRUE)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-2-4.png" width="672" />

# Histogram
summarize data measured on interval scale

```r
# Simple Histogram
hist(mtcars$mpg)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-3-1.png" width="672" />

```r
# Add a Normal Curve (Thanks to Peter Dalgaard)
x <- mtcars$mpg 
h<-hist(x, breaks=10, col="red", xlab="Miles Per Gallon", 
   main="Histogram with Normal Curve") 
xfit<-seq(min(x),max(x),length=40) 
yfit<-dnorm(xfit,mean=mean(x),sd=sd(x)) 
yfit <- yfit*diff(h$mids[1:2])*length(x) 
lines(xfit, yfit, col="blue", lwd=2)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-3-2.png" width="672" />

# Pei Chart
Proportional display from categorical data

```r
# Simple Pie Chart
slices <- c(10, 12,4, 16, 8)
lbls <- c("US", "UK", "Australia", "Germany", "France")
pie(slices, labels = lbls, main="Pie Chart of Countries")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-4-1.png" width="672" />

```r
# 3D Exploded Pie Chart
library(plotrix)
slices <- c(10, 12, 4, 16, 8) 
lbls <- c("US", "UK", "Australia", "Germany", "France")
pie3D(slices,labels=lbls,explode=0.1,
   main="Pie Chart of Countries ")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-4-2.png" width="672" />


# Scatter plots
relationship between two variables

```r
# Simple Scatterplot mtcars
attach(mtcars)
plot(mtcars$wt, mtcars$mpg, main="Scatterplot Example", 
   xlab="Car Weight ", ylab="Miles Per Gallon ", pch=19)

# Add fit lines
abline(lm(mpg~wt), col="red") # regression line (y~x) 
lines(lowess(wt,mpg), col="blue") # lowess line (x,y)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-5-1.png" width="672" />

```r
# Enhanced Scatterplot of MPG vs. Weight 
# by Number of Car Cylinders 
library(car) 
```

```
## Loading required package: carData
```

```r
scatterplot(mpg ~ wt | cyl, data = mtcars, 
            xlab="Weight of Car", ylab="Miles Per Gallon", 
   main ="Enhanced Scatter Plot")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-5-2.png" width="672" />

```r
scatterplot(wt ~ mpg, data = mtcars)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-5-3.png" width="672" />

```r
# Prepare the data set
x <- iris$Sepal.Length
y <- iris$Sepal.Width
z <- iris$Petal.Length
grps <- as.factor(iris$Species)
# Plot
library(scatterplot3d)
scatterplot3d(x, y, z, pch = 16)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-5-4.png" width="672" />

# Measurement of Central Tendency
## Mean
sum of all data divided by the number of data
## Median
"middle number"\
found by ordering all data
## Mode
the most frequent number

Comparing mean, median, and mode gets a skewness of distribution.

# Data types
`$$\begin{equation}
    \text{Data} =
    \begin{cases}
      \text{Discrete data} 
      & \begin{cases} 
        \text{Nominal Scale} &- \text{ incomparable}\\
        \text{Ordinal Scale} &- \text{ comparable}\\
      \end{cases} \\
      \text{Continuous data} 
       &\begin{cases} 
        \text{Interval Scale} &- \text{ additive}\\
        \text{Ratio Scale} &- \text{ multiplicative}\\
      \end{cases}
    \end{cases}
  \end{equation}$$`


#Random Variables
## Random variables
characteristics of the observational units which can have different possible values (this is the practical, not the statistical definition)
There are two different types of random variables:

## Quantitative
(numerical, measurement) variables represent an amount or quantity of something (e.g. time spent waiting for the bus)

## Qualitative
(categorical) variables represent things that can be categorized (e.g. the colors of the cars that pass while you wait for the bus)
Letters like X or Y represent random variables if its value is not known before the experiment is run.

# Categorical: Nominal vs. Ordinal

## Nominal
(unordered) random variables have categories where order doesn???t matter.
e.g. gender, ethnic background, religious affiliation

## Ordinal 
(ordered) random variables have ordered categories. ( e.g. grade levels, income levels, school levels, ...)

# Explanatory vs. Response Variable

The explanatory variable attempts to explain (or is purported to cause) differences in a response variable (or outcome variable), (e.g. homework scores and exam scores can be explanatory variables for the final grade).

# Quantitative: Discrete vs. Continuous
## Discrete
random variables can only take on values from a countable set of numbers such as the integers or some subset of integers. (Usually, they can???t be fractions.)

## Continuous
random variables can take on any real number in some interval. (They can be fractions.)

# Continuous Variables
a variable whose value is obtained by measuring, ie one which can take on an uncountable set of values. an unlimited number of values between the lowest and highest points of measurement.
$$
\begin{aligned}
\mathop{\mathbb{E\left(x\right)}} &= \int_x xp\left(x\right)\mathrm{d}x \\
\mathop{\mathbb{E\left(f\left(x\right)\right)}} &= \int_x f\left(x\right)p\left(x\right)\mathrm{d}x
\end{aligned}$$`


# Discrete Varialbes
a variable whose value is obtained by counting. a discrete variable over a particular range of real values is one for which, for any value in the range that the variable is permitted to take on, there is a positive minimum distance to the nearest other permissible value.

$$
\begin{aligned}
\mathop{\mathbb{E\left(x\right)}} &= \sum_i x_ip\left(x_i\right) \\
\mathop{\mathbb{E\left(f\left(x\right)\right)}} &=  \sum_i f\left(x_i\right)p\left(x_i\right) 
\end{aligned}$$`

# Population and Sample
Population is the entire set of cases.\
Sample is a subset of population.

![Population and statistics](statistical_view.jpg)

<image from: https://online.stat.psu.edu/stat504/lesson/populations-and-samples>

# Degrees of freedom (d.f., df)
The number of observation that are free to vary.\
the minimum number of independent coordinates that can specify the phase space.\
The number of independent pieces of information that go into the estimate of a parameter.\
The number of independent scores that go into the estimate minus the number of parameters used as intermediate steps in the estimation of the parameter itself.


# Variance
The expectation of the squared deviation of a random variable from its population mean or sample mean.
It usually means how far a set of numbers is spread out from their average value. \
Definition:
$$
\begin{aligned}
Var(x) &= \mathop{\mathbb{E\left[\left(X-\mathop{\mathbb{E\left(X\right)}}\right)^2\right]}}\\
&= \mathop{\mathbb{E\left(X^2\right)-\left(\mathop{\mathbb{E\left(X\right)}}\right)^2}}
\end{aligned}$$`

Properties:\
`$$\begin{aligned}
Var(X) &\ge 0\\
Var(a) &=0\\
Var(X) &=0 \iff \exists a, P(X=a)=0\\ 
Var(X+a) &= Var(X)\\
Var(aX) &= a^2Var(X)\\ 
Var(aX\pm bY) &= a^2Var(X)+b^2Var(Y)\pm 2abCov(X,Y)\\
\end{aligned}$$`

Decomposition:
`$$Var(X) = \mathop{\mathbb{E}}[Var(X|Y)]+Var(\mathop{\mathbb{E}}[X|Y])$$`
This is a similar idea as 
`$$MST = MSR + MSE$$`


```r
# enter data
y=c(445, 530, 540, 510, 570, 530, 545, 545, 505, 535, 450, 500, 520,
    460, 430, 520, 520, 430, 535, 535, 475, 545, 420, 495, 485, 570, 
    480, 495, 470, 490)
mean((y-mean(y))^2)
```

```
## [1] 1656.222
```

```r
# 'pop. var.' where n > 1
n=length(y); 
var(y)*(n-1)/n
```

```
## [1] 1656.222
```

```r
#built-in R function - sample var
var(y)
```

```
## [1] 1713.333
```

# Normalization / Standarization
the process of putting different variables on the same scale

`$$x_s = \frac{x-\bar x}{\sigma}$$`
mean = 0\
sd = 1


# Covariance
a measure of the joint variability of two random variables\
positive covariance = same relation\
negative covariance = opposite relation

Definition:
`$$\operatorname{cov} (X,Y)=\frac{1}{n}\sum_{i=1}^n (x_i-E(X))(y_i-E(Y))$$`
`$$\operatorname{cov}(X, Y) = \operatorname{E}{\big[(X - \operatorname{E}[X])(Y - \operatorname{E}[Y])\big]}$$`
Definition `\(\rightarrow\)` Computation formula:
`$$\begin{align}
\operatorname{cov}(X, Y) 
&= \operatorname{E}\left[\left( X - \operatorname{E}\left[ X\right]\right) \left( Y - \operatorname{E}\left[ Y\right]\right)\right] \\
&= \operatorname{E}\left[XY - X\operatorname{E}\left[Y\right] - \operatorname{E}\left[X\right] Y + \operatorname{E}\left[X\right] \operatorname{E}\left[Y\right]\right] \\
&= \operatorname{E}\left[X Y\right] - \operatorname{E}\left[X\right] \operatorname{E}\left[Y\right] - \operatorname{E}\left[X\right] \operatorname{E}\left[Y\right] + \operatorname{E}\left[X\right] \operatorname{E}\left[Y\right] \\
&= \operatorname{E}\left[X Y\right] - \operatorname{E}\left[X\right] \operatorname{E}\left[Y\right]
\end{align}$$`

Properties:\
`$$\operatorname{cov}(X, X) =\operatorname{var}(X)\equiv\sigma^2(X)\equiv\sigma_X^2.$$`
Also,\

`$$\begin{aligned}
\operatorname{cov}(X,a) &= 0\\
\operatorname{cov}(X,X) &= \operatorname{var}(X)\\
\operatorname{cov}(X,Y) &= \operatorname{cov}(Y,X)\\
\operatorname{cov}(aX,bY) &= ab\operatorname{cov}(X,Y)\\
\operatorname{cov}(X+a,Y+b)&=\operatorname{cov}(X,Y)\\
\operatorname{cov}(aX+bY, cW+dV) &= ac\operatorname{cov}(X,W)+ad\operatorname{cov}(X,V)+bc\operatorname{cov}(Y,W)+bd\operatorname{cov}(Y,V)
\end{aligned}$$`


If X and Y are independent random variables:\
`$$\begin{aligned}
  \operatorname{cov}(X, Y) &= \operatorname{cov}\left(X, X^2\right) \\
         &= \operatorname{E}\left[X \cdot X^2\right] - \operatorname{E}[X] \cdot \operatorname{E}\left[X^2\right] \\
         &= \operatorname{E}\left[X^3\right] - \operatorname{E}[X]\operatorname{E}\left[X^2\right] \\
         &= 0 - 0 \cdot \operatorname{E}[X^2] \\
         &= 0.  
\end{aligned}$$`
Let `\(\mathbf {X}\)`  be a random vector with covariance matrix ??, and let A be a matrix that can act on `\(\mathbf {X}\)`  on the left. The covariance matrix of the matrix-vector product A X is:\
`$$\begin{align}
  \operatorname{cov}(\mathbf{AX},\mathbf{AX}) &=
  \operatorname{E}\left[\mathbf{AX(A}\mathbf{X)}^\mathrm{T}\right] - \operatorname{E}[\mathbf{AX}] \operatorname{E}\left[(\mathbf{A}\mathbf{X})^\mathrm{T}\right] \\
  &= \operatorname{E}\left[\mathbf{AXX}^\mathrm{T}\mathbf{A}^\mathrm{T}\right] - \operatorname{E}[\mathbf{AX}] \operatorname{E}\left[\mathbf{X}^\mathrm{T}\mathbf{A}^\mathrm{T}\right] \\
  &= \mathbf{A}\operatorname{E}\left[\mathbf{XX}^\mathrm{T}\right]\mathbf{A}^\mathrm{T} - \mathbf{A}\operatorname{E}[\mathbf{X}] \operatorname{E}\left[\mathbf{X}^\mathrm{T}\right]\mathbf{A}^\mathrm{T} \\
  &= \mathbf{A}\left(\operatorname{E}\left[\mathbf{XX}^\mathrm{T}\right] - \operatorname{E}[\mathbf{X}] \operatorname{E}\left[\mathbf{X}^\mathrm{T}\right]\right)\mathbf{A}^\mathrm{T} \\
  &= \mathbf{A}\Sigma\mathbf{A}^\mathrm{T}.
\end{align}$$`

For real random vectors `\(\mathbf {X} \in \mathbb {R} ^{m}\)` and `\(\mathbf {Y} \in \mathbb {R}\)` ^{n}, the `\(m\times n\)` cross-covariance matrix is
`$$\begin{align}
  \operatorname{K}_{\mathbf{X}\mathbf{Y}} = \operatorname{cov}(\mathbf{X},\mathbf{Y}) 
    &= \operatorname{E}\left[
         (\mathbf{X} - \operatorname{E}[\mathbf{X}])
         (\mathbf{Y} - \operatorname{E}[\mathbf{Y}])^\mathrm{T}
       \right] \\
    &= \operatorname{E}\left[\mathbf{X} \mathbf{Y}^\mathrm{T}\right] - \operatorname{E}[\mathbf{X}]\operatorname{E}[\mathbf{Y}]^\mathrm{T}
\end{align}$$`


```r
# by hand
x <- c(25, 27, 29)
y <- c(5, 15, 9)
xdev <- x - mean(x)
ydev <- y - mean(y)
xdev_ydev <- xdev * ydev
xdev_ydev
```

```
## [1]  9.333333  0.000000 -1.333333
```

```r
sum_xdev_ydev <- sum(xdev_ydev)
cov_xy <- sum_xdev_ydev / (3 - 1)
cov_xy
```

```
## [1] 4
```

```r
#built-in function
cov(x, y)
```

```
## [1] 4
```

# Correlation
Correlation standardizes covariance on a scale of negative one to one, whereby the magnitude from zero indicates strength of relationship.\
`\(-1 \le r_{xy} \le +1\)`.


`$$\begin{align}
\rho_{X,Y} &= \operatorname{cor}(X,Y) \\
&= \frac{\operatorname{cov}(X,Y)}{\sigma_X \sigma_Y} \\
&= \frac{\operatorname{E}[(X-\mu_X)(Y-\mu_Y)]}{\sigma_X\sigma_Y}
\end{align}$$`

`$$\rho_{X,Y} = \frac {\operatorname{E}(XY)-\operatorname{E}(X)\operatorname{E}(Y)}{ \sqrt{\operatorname{E}(X^2)-\operatorname{E}(X)^2}\cdot \sqrt{\operatorname{E}(Y^2)-\operatorname{E}(Y)^2}}$$`
`$$\begin{align}
X,Y \text{ independent} \quad & \Rightarrow \quad \rho_{X,Y} = 0 \quad (X,Y \text{ uncorrelated})\\
\rho_{X,Y} = 0 \quad (X,Y \text{ uncorrelated})\quad & \nRightarrow \quad X,Y \text{ independent}
\end{align}$$`


```r
# by hand
x <- c(25, 27, 29)
y <- c(5, 15, 9)
xdev <- x - mean(x)
ydev <- y - mean(y)
xdev_ydev <- xdev * ydev
sum_xdev_ydev <- sum(xdev_ydev)
cov_xy <- (1 / (3 - 1)) * sum_xdev_ydev
stnd.dev <- sd(x)*sd(y)
cov_xy/stnd.dev
```

```
## [1] 0.3973597
```

```r
# built-in function
cor(x,y)
```

```
## [1] 0.3973597
```


# Skewness
a measure of the asymmetry of the probability distribution of a real-valued random variable about its mean\
The skewness value can be positive, zero, negative, or undefined.

![Relationship between mean and median under different skewness](skewness.png)
$$    \tilde{\mu}_3 = \operatorname{E}\left[\left(\frac{X-\mu}{\sigma}\right)^3 \right]
             = \frac{\mu_3}{\sigma^3}
             = \frac{\operatorname{E}\left[(X-\mu)^3\right]}{( \operatorname{E}\left[ (X-\mu)^2 \right] )^{3/2}}
             = \frac{\kappa_3}{\kappa_2^{3/2}}
 $$

`$$\begin{aligned}
\tilde{\mu}_3
&= \operatorname{E}\left[\left(\frac{X-\mu}{\sigma}\right)^3 \right] \\
&= \frac{\operatorname{E}[X^3] - 3\mu\operatorname E[X^2] + 3\mu^2\operatorname E[X] - \mu^3}{\sigma^3}\\
&= \frac{\operatorname{E}[X^3] - 3\mu(\operatorname E[X^2] -\mu\operatorname E[X]) - \mu^3}{\sigma^3}\\
&= \frac{\operatorname{E}[X^3] - 3\mu\sigma^2 - \mu^3}{\sigma^3}.
\end{aligned}$$`



```r
# by hand
```

# Bias
a feature of a statistical technique or of its results whereby the expected value of the results differs from the true underlying quantitative parameter being estimated

`\(\operatorname{bias}(\theta)\)` is he bias of the statistic `\(T\)`
`$$\operatorname E(T)=\theta + \operatorname{bias}(\theta)$$`



# Mean Squared Error (MSE)
a measurement of the quality of a predictor (i.e., a function mapping arbitrary inputs to a sample of values of some random variable)\
or of an estimator (i.e., a mathematical function mapping a sample of data to an estimate of a parameter of the population from which the data is sampled).

the within-sample MSE of the predictor is computed as
`$$\operatorname{MSE}=\frac{1}{n}\sum_{i=1}^n(Y_i-\hat{Y_i})^2.$$`

In matrix notation,
`$$\operatorname{MSE}=\frac{1}{n}\sum_{i=1}^n(e_i)^2=\frac{1}{n}\mathbf e^\mathsf T \mathbf e$$`

`$$\begin{aligned}
R^2 &=  1-\frac{MSE(X)}{Var(X)}\\
&= 1-\frac{SSE}{SST}
\end{aligned}$$`

The MSE of an estimator `\(\hat{\theta}\)` with respect to an unknown parameter `\(\theta\)`  is defined as
`$$\operatorname{MSE}(\hat{\theta})=\operatorname{E}_{\theta}\left[(\hat{\theta}-\theta)^2\right].$$`

The MSE can be written as the sum of the variance of the estimator and the squared bias of the estimator, providing a useful way to calculate the MSE and implying that in the case of unbiased estimators, the MSE and variance are equivalent.\

`$$\operatorname{MSE}(\hat{\theta})=\operatorname{Var}_{\theta}(\hat{\theta})+ \operatorname{Bias}(\hat{\theta},\theta)^2.$$`


