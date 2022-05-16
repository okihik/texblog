---
title: Pass Analysis
author: Akihiko Mori
date: '2022-05-16'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose
We will a pass analysis using R. 
Implemented from the generation of data, it can be copied and pasted.

## Contents
1. Prerequisites
2. Data generation
3. Pass Analysis

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
LA <- rnorm(100, 50, 10) %>% round()
calc <- rnorm(100, 50, 10) %>% round()
ML <- rnorm(100, 0.7*LA+0.3*calc, 5) %>% round()
NN <- rnorm(100, 0.2*LA+0.8*calc, 5) %>% round()
data <- data.frame(LA, calc, ML, NN)
data <- apply(data, 2, scale)
data %>% head()
```

```
##               LA        calc          ML          NN
## [1,] -0.79298532 -0.58763590 -0.55601884 -0.23868434
## [2,]  0.09551364  0.03861906  1.06570278 -0.57173226
## [3,] -1.01511006 -0.90076338 -0.09266981  0.09436358
## [4,]  1.65038682  0.24737071  1.06570278  0.31639552
## [5,]  0.20657601 -0.69201173 -1.36687965  0.31639552
## [6,] -1.01511006  1.91738392  1.29737730  2.20366708
```

## Pass Analysis

```r
data.model <- '
ML ~ LA+calc
NN ~ LA+calc
'
model <- sem(data.model, data=data)
summary(model)
```

```
## lavaan 0.6-11 ended normally after 12 iterations
## 
##   Estimator                                         ML
##   Optimization method                           NLMINB
##   Number of model parameters                         7
##                                                       
##   Number of observations                           100
##                                                       
## Model Test User Model:
##                                                       
##   Test statistic                                 0.000
##   Degrees of freedom                                 0
## 
## Parameter Estimates:
## 
##   Standard errors                             Standard
##   Information                                 Expected
##   Information saturated (h1) model          Structured
## 
## Regressions:
##                    Estimate  Std.Err  z-value  P(>|z|)
##   ML ~                                                
##     LA                0.737    0.060   12.232    0.000
##     calc              0.304    0.060    5.041    0.000
##   NN ~                                                
##     LA                0.170    0.055    3.089    0.002
##     calc              0.818    0.055   14.893    0.000
## 
## Covariances:
##                    Estimate  Std.Err  z-value  P(>|z|)
##  .ML ~~                                               
##    .NN                0.034    0.033    1.027    0.304
## 
## Variances:
##                    Estimate  Std.Err  z-value  P(>|z|)
##    .ML                0.360    0.051    7.071    0.000
##    .NN                0.299    0.042    7.071    0.000
```

```r
semPaths(model, "model", "est", rotation = 2,
mar=c(5,8,5,14),
edge.label.cex=2,
sizeMan = 15,
sizeLat = 15,
style = "lisrel",
curve = 3,
nCharNodes = 4
)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />
