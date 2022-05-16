---
title: Confirmatory Factor Analysis
author: Akihiko Mori
date: '2022-05-15'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose
We will Confirmatory Factor Analysis using R. 
Implemented from the generation of data, it can be copied and pasted.

## Contents
1. Prerequisites
2. One factor case
3. Two factor case

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
library(lavaan)
library(semPlot)
```


## One factor case
### Data generation

```r
set.seed(1)
scis <- rnorm(100, 50, 10)
arts <- rnorm(100, 50, 10)
eng <- rnorm(100, arts, 10) %>% round()
math <- rnorm(100, scis, 10) %>% round()
sec <- rnorm(100, arts, 10) %>% round()
sci <- rnorm(100, scis, 10) %>% round()
soc <- rnorm(100, arts, 10) %>% round()
data <- data.frame(eng, math, sec, sci, soc)
data <- apply(data, 2, scale)
data %>% head()
```

```
##             eng       math         sec        sci        soc
## [1,] -0.1407805  0.1034029  0.35862833 -0.4571503 -0.5525677
## [2,]  1.2451414 -0.8157339  1.23029442 -0.1363431  1.2650892
## [3,]  0.5157088  0.7161608 -0.88660894 -1.6601775 -0.1163300
## [4,] -0.1407805  0.7927555 -0.07720471  1.2270877  0.6834390
## [5,] -2.1102484  1.4055134 -0.63756148  0.9864823 -0.4071551
## [6,]  3.1416660  0.4097818  0.91898511  0.5854732  0.6107327
```


### Confirmatory Factor Analysis (cfa)

```r
data.model <- 'scis=~math+sci
arts=~eng+sec+soc'
model <- cfa(data.model, data=data)
summary(model,fit.measures=TRUE)
```

```
## lavaan 0.6-11 ended normally after 40 iterations
## 
##   Estimator                                         ML
##   Optimization method                           NLMINB
##   Number of model parameters                        11
##                                                       
##   Number of observations                           100
##                                                       
## Model Test User Model:
##                                                       
##   Test statistic                                 2.566
##   Degrees of freedom                                 4
##   P-value (Chi-square)                           0.633
## 
## Model Test Baseline Model:
## 
##   Test statistic                                72.036
##   Degrees of freedom                                10
##   P-value                                        0.000
## 
## User Model versus Baseline Model:
## 
##   Comparative Fit Index (CFI)                    1.000
##   Tucker-Lewis Index (TLI)                       1.058
## 
## Loglikelihood and Information Criteria:
## 
##   Loglikelihood user model (H0)               -672.222
##   Loglikelihood unrestricted model (H1)       -670.938
##                                                       
##   Akaike (AIC)                                1366.443
##   Bayesian (BIC)                              1395.100
##   Sample-size adjusted Bayesian (BIC)         1360.359
## 
## Root Mean Square Error of Approximation:
## 
##   RMSEA                                          0.000
##   90 Percent confidence interval - lower         0.000
##   90 Percent confidence interval - upper         0.123
##   P-value RMSEA <= 0.05                          0.732
## 
## Standardized Root Mean Square Residual:
## 
##   SRMR                                           0.027
## 
## Parameter Estimates:
## 
##   Standard errors                             Standard
##   Information                                 Expected
##   Information saturated (h1) model          Structured
## 
## Latent Variables:
##                    Estimate  Std.Err  z-value  P(>|z|)
##   scis =~                                             
##     math              1.000                           
##     sci               0.465    2.864    0.162    0.871
##   arts =~                                             
##     eng               1.000                           
##     sec               1.229    0.299    4.108    0.000
##     soc               0.989    0.230    4.299    0.000
## 
## Covariances:
##                    Estimate  Std.Err  z-value  P(>|z|)
##   scis ~~                                             
##     arts              0.024    0.073    0.328    0.743
## 
## Variances:
##                    Estimate  Std.Err  z-value  P(>|z|)
##    .math              0.232    4.663    0.050    0.960
##    .sci               0.826    1.016    0.813    0.416
##    .eng               0.604    0.120    5.035    0.000
##    .sec               0.407    0.140    2.918    0.004
##    .soc               0.613    0.120    5.127    0.000
##     scis              0.758    4.665    0.162    0.871
##     arts              0.386    0.139    2.772    0.006
```

## Structural Equation Modeling

### Analysis

```r
model2 <- sem(data.model, data=data)
semPaths(model2, "model", "est", rotation = 2,
mar=c(3,5,3,7),
edge.label.cex=1.5,
sizeMan = 15,
sizeLat = 15,
style = "lisrel",
curve = 3
)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />
