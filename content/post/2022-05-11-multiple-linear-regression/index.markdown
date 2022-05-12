---
title: Multiple Linear Regression
author: Akihiko Mori
date: '2022-05-12'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose

We will implement single regression analysis using R. Implemented from the creation of data, the code can be copied and pasted.

## Contents
1. Prerequisites
2. Generating Data
3. Visualization of Data
4. Analysis
5. Visualization of Results

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
```

### Assumptions

## Data

### R Code

```r
set.seed(10)
age <- rnorm(100, 40, 20) 
shoulder <- rnorm(100, 40, 3)
height <- rnorm(100, 170, 10)
err <- rnorm(100, 0, 50) 
income <- 5*age + rnorm(100,10*shoulder,70) + 300 + err
data <-data.frame(age, shoulder, height, income) %>%
filter(18<age&age<60) %>%
mutate(age=round(age), income=round(income), height=round(height), shoulder=round(shoulder))
```

### Results

```r
data %>% head()
```

```
##   age shoulder height income
## 1  40       38    182    949
## 2  36       41    173   1006
## 3  28       42    179    929
## 4  46       38    159    865
## 5  48       42    175    835
## 6  33       35    157    786
```

## Regression Analysis

### R code

```r
model <- lm(data=data, income~age+shoulder+height)
```
### Results

```r
summary(model)
```

```
## 
## Call:
## lm(formula = income ~ age + shoulder + height, data = data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -218.550  -61.292    5.491   66.630  204.817 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 293.0234   269.3576   1.088   0.2806    
## age           5.9241     0.9482   6.248 3.28e-08 ***
## shoulder      8.4364     3.7952   2.223   0.0296 *  
## height        0.2723     1.2235   0.223   0.8245    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 92.37 on 67 degrees of freedom
## Multiple R-squared:  0.3886,	Adjusted R-squared:  0.3612 
## F-statistic: 14.19 on 3 and 67 DF,  p-value: 2.929e-07
```

## Multicollinearity

### R code

```r
set.seed(1)
age <- rnorm(100, 40, 20) 
shoulder <- rnorm(100, age, 1)
height <- rnorm(100, 170, 10)
err <- rnorm(100, 0, 50) 
income <- 5*age + rnorm(100,10*shoulder,70) + 300 + err
data <-data.frame(age, shoulder, height, income) %>%
  filter(18<age&age<60) %>%
  mutate(age=round(age), income=round(income), height=round(height), shoulder=round(shoulder))
data %>% head()
```

```
##   age shoulder height income
## 1  27       27    174    826
## 2  44       44    187   1036
## 3  23       22    186    697
## 4  47       46    147   1046
## 5  24       25    195    721
## 6  50       50    177   1032
```

```r
cor(data[,c(4,1:3)])
```

```
##              income        age   shoulder     height
## income    1.0000000  0.8705654  0.8768171 -0.2291069
## age       0.8705654  1.0000000  0.9950794 -0.2797307
## shoulder  0.8768171  0.9950794  1.0000000 -0.2826242
## height   -0.2291069 -0.2797307 -0.2826242  1.0000000
```

```r
model <- lm(data=data, income~age+shoulder+height)
summary(model)
```

```
## 
## Call:
## lm(formula = income ~ age + shoulder + height, data = data)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -258.73  -49.82    9.44   55.94  215.07 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)  
## (Intercept) 211.9309   198.3031   1.069   0.2888  
## age          -3.6525    10.4760  -0.349   0.7284  
## shoulder     19.4120    10.3242   1.880   0.0642 .
## height        0.3698     1.0634   0.348   0.7291  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 94.56 on 71 degrees of freedom
## Multiple R-squared:  0.7696,	Adjusted R-squared:  0.7598 
## F-statistic: 79.05 on 3 and 71 DF,  p-value: < 2.2e-16
```


