---
title: Exploratory Factor Analysis
author: Akihiko Mori
date: '2022-05-15'
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
2. One factor case
3. Two factor case

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
library(gridExtra)
library(psych)
```


## One factor case
### Data generation

```r
set.seed(1)
f <- rnorm(100, 50, 10) 
eng <- rnorm(100, f, 3) %>% round()
math <- rnorm(100, f, 3) %>% round()
data <- data.frame(eng,math)
data %>% head()
```

```
##   eng math
## 1  42   45
## 2  52   57
## 3  39   46
## 4  66   65
## 5  51   46
## 6  47   49
```
### Data visualization

```r
data %>%
  ggplot() +
  geom_point(aes(eng, math))+
  theme_classic(base_family = "")+
  theme(text=element_text(size=20)) +
  labs(title="scatter plot")+
  ggeasy::easy_center_title()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

### Factor Analysis

```r
model <- fa(r=data, nfactors=1, fm="ml")
par(family= "")
fa.diagram(model)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

## Two factor case

### Data generation

```r
set.seed(10)
scis <- rnorm(100, 50, 10)
arts <- rnorm(100, 50, 10)
eng <- rnorm(100, 0.7*arts+0.3*scis, 5) %>% round()
math <- rnorm(100, 0.3*arts+0.7*scis, 5) %>% round()
sec <- rnorm(100, 0.7*arts+0.3*scis, 5) %>% round()
sci<- rnorm(100, 0.3*arts+0.7*scis, 5) %>% round()
soc <- rnorm(100, 0.7*arts+0.3*scis, 5) %>% round()
data <- data.frame(eng, math, sec, sci, soc)
data <- apply(data, 2, scale)
data %>% head()
```

```
##             eng       math        sec        sci         soc
## [1,]  0.2349844  0.5859149 -0.5442507  0.3336090 -1.03458355
## [2,]  0.6021477  0.3622833  1.0769217 -0.2356896  0.38589730
## [3,] -0.3769542 -1.7622174  0.1505374 -1.2604272 -1.98157079
## [4,]  1.0916986  0.2504674  0.7295276 -0.2356896  0.74101751
## [5,] -0.9888929  0.1386516 -0.6600488  0.5613284  0.50427070
## [6,]  1.0916986  0.6977307 -0.4284527  1.5860660  0.03077709
```

### Data visualization

```r
model1 <- fa(data, nfactors = 2, rotate = "none", fm="ml")
model2 <- fa(data, nfactors = 2, rotate = "varimax", fm="ml")
model1 ; model2
```

```
## Factor Analysis using method =  ml
## Call: fa(r = data, nfactors = 2, rotate = "none", fm = "ml")
## Standardized loadings (pattern matrix) based upon correlation matrix
##       ML1   ML2   h2   u2 com
## eng  0.78 -0.31 0.71 0.29 1.3
## math 0.74  0.34 0.67 0.33 1.4
## sec  0.73 -0.27 0.61 0.39 1.3
## sci  0.73  0.33 0.64 0.36 1.4
## soc  0.76 -0.03 0.57 0.43 1.0
## 
##                        ML1  ML2
## SS loadings           2.81 0.40
## Proportion Var        0.56 0.08
## Cumulative Var        0.56 0.64
## Proportion Explained  0.88 0.12
## Cumulative Proportion 0.88 1.00
## 
## Mean item complexity =  1.3
## Test of the hypothesis that 2 factors are sufficient.
## 
## The degrees of freedom for the null model are  10  and the objective function was  2.19 with Chi Square of  211.2
## The degrees of freedom for the model are 1  and the objective function was  0 
## 
## The root mean square of the residuals (RMSR) is  0 
## The df corrected root mean square of the residuals is  0 
## 
## The harmonic number of observations is  100 with the empirical chi square  0  with prob <  0.98 
## The total number of observations was  100  with Likelihood Chi Square =  0  with prob <  0.96 
## 
## Tucker Lewis Index of factoring reliability =  1.05
## RMSEA index =  0  and the 90 % confidence intervals are  0 0
## BIC =  -4.6
## Fit based upon off diagonal values = 1
## Measures of factor score adequacy             
##                                                    ML1  ML2
## Correlation of (regression) scores with factors   0.94 0.74
## Multiple R square of scores with factors          0.89 0.54
## Minimum correlation of possible factor scores     0.78 0.08
```

```
## Factor Analysis using method =  ml
## Call: fa(r = data, nfactors = 2, rotate = "varimax", fm = "ml")
## Standardized loadings (pattern matrix) based upon correlation matrix
##       ML1  ML2   h2   u2 com
## eng  0.79 0.31 0.71 0.29 1.3
## math 0.31 0.76 0.67 0.33 1.3
## sec  0.72 0.30 0.61 0.39 1.3
## sci  0.31 0.74 0.64 0.36 1.3
## soc  0.57 0.49 0.57 0.43 2.0
## 
##                        ML1  ML2
## SS loadings           1.66 1.55
## Proportion Var        0.33 0.31
## Cumulative Var        0.33 0.64
## Proportion Explained  0.52 0.48
## Cumulative Proportion 0.52 1.00
## 
## Mean item complexity =  1.5
## Test of the hypothesis that 2 factors are sufficient.
## 
## The degrees of freedom for the null model are  10  and the objective function was  2.19 with Chi Square of  211.2
## The degrees of freedom for the model are 1  and the objective function was  0 
## 
## The root mean square of the residuals (RMSR) is  0 
## The df corrected root mean square of the residuals is  0 
## 
## The harmonic number of observations is  100 with the empirical chi square  0  with prob <  0.98 
## The total number of observations was  100  with Likelihood Chi Square =  0  with prob <  0.96 
## 
## Tucker Lewis Index of factoring reliability =  1.05
## RMSEA index =  0  and the 90 % confidence intervals are  0 0
## BIC =  -4.6
## Fit based upon off diagonal values = 1
## Measures of factor score adequacy             
##                                                    ML1  ML2
## Correlation of (regression) scores with factors   0.85 0.84
## Multiple R square of scores with factors          0.73 0.70
## Minimum correlation of possible factor scores     0.45 0.41
```

### Analysis visualization

```r
fa.diagram(model1)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

```r
fa.diagram(model2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-2.png" width="672" />
