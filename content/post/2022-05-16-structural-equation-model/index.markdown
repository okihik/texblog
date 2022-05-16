---
title: Structural Equation Model (SEM)
author: Akihiko Mori
date: '2022-05-16'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose
We will a structural equation model/analysis using R. 
Implemented from the generation of data, it can be copied and pasted.

## Contents
1. Prerequisites
2. Data generation
3. Structural equation model

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
library(lavaan)
library(semPlot)
```

## Data generation

```r
set.seed(1)
math <- rnorm(100, 50, 10)
programming <- rnorm(100, math, 5)
calc <- rnorm(100, math, 5) %>% round()
stat <- rnorm(100, math, 5) %>% round()
LA <- rnorm(100, math, 5) %>% round()
Rprog <- rnorm(100, programming, 5) %>% round()
Python <- rnorm(100, programming, 5) %>% round()
data <- data.frame(calc, stat, LA, Rprog, Python)
data <- apply(data, 2, scale)
data %>% head()
```

```
##            calc       stat         LA       Rprog      Python
## [1,] -0.5007136 -0.3365948 -0.1642212 -0.89192146 -0.93453226
## [2,]  0.8370708 -0.4367719  0.8839990  0.03234948  0.86264516
## [3,] -0.1184895  0.0641133 -1.0377380 -1.81619240 -0.84895238
## [4,]  1.2192949  1.2662377  1.1460541  1.51118299  1.63286405
## [5,] -0.8829377  1.0658836  0.0104822  0.40205786 -0.07873349
## [6,]  0.2637346 -0.2364178 -0.9503863  0.77176624 -0.42105299
```

## SEM Analysis

```r
data.model <- '
math=~calc+stat+LA
programming=~Rprog+Python
math=~programming
'
model <- sem(data.model, data)
summary(model)
```

```
## lavaan 0.6-11 ended normally after 23 iterations
## 
##   Estimator                                         ML
##   Optimization method                           NLMINB
##   Number of model parameters                        11
##                                                       
##   Number of observations                           100
##                                                       
## Model Test User Model:
##                                                       
##   Test statistic                                 3.867
##   Degrees of freedom                                 4
##   P-value (Chi-square)                           0.424
## 
## Parameter Estimates:
## 
##   Standard errors                             Standard
##   Information                                 Expected
##   Information saturated (h1) model          Structured
## 
## Latent Variables:
##                    Estimate  Std.Err  z-value  P(>|z|)
##   math =~                                             
##     calc              1.000                           
##     stat              0.990    0.085   11.660    0.000
##     LA                1.005    0.084   11.961    0.000
##   programming =~                                      
##     Rprog             1.000                           
##     Python            1.048    0.091   11.475    0.000
##   math =~                                             
##     programming       0.865    0.094    9.209    0.000
## 
## Variances:
##                    Estimate  Std.Err  z-value  P(>|z|)
##    .calc              0.228    0.046    4.994    0.000
##    .stat              0.243    0.047    5.163    0.000
##    .LA                0.221    0.045    4.903    0.000
##    .Rprog             0.241    0.053    4.515    0.000
##    .Python            0.167    0.051    3.284    0.001
##     math              0.762    0.140    5.443    0.000
##    .programming       0.179    0.052    3.476    0.001
```

```r
semPaths(model, "model", "est", rotation = 2,
mar=c(5,6,5,6),
edge.label.cex=1.0, 
sizeMan = 12,
sizeLat = 12,
style = "lisrel",
curve = 3,
nCharNodes = 7
)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />
