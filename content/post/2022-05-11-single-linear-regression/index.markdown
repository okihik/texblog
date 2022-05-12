---
title: Single Linear Regression Analysis
author: Akihiko Mori
date: '2022-05-11'
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
err <- rnorm(100, 0, 50) 
income <- 5*age + 300 + err
data <- data.frame(age, income) %>%
  filter(18<age&age<60) %>%
  mutate(age=round(age), income=round(income))
```

### Results

```r
data %>% head()
```

```
##   age income
## 1  40    464
## 2  36    503
## 3  28    476
## 4  46    498
## 5  48    567
## 6  33    381
```

## Visualization of Data

### R Code

```r
x <- seq(20,60,1)
plot <- ggplot()+
  geom_point(aes(data$age, data$income))+
  theme_classic(base_family = "") +
  theme(text=element_text(size=20)) +
  labs(x="Age", y="Income") +
  ggtitle("Single Linear Regression") +
  ggeasy::easy_center_title()
```

### Results

```r
plot
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

## Analysis

### R code

```r
model <- lm(data=data, income~age)
```
### Results

```r
summary(model)
```

```
## 
## Call:
## lm(formula = income ~ age, data = data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -110.452  -32.765    4.869   36.106  107.472 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 306.7034    20.7579  14.775  < 2e-16 ***
## age           4.7634     0.5033   9.463 4.31e-14 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 49.16 on 69 degrees of freedom
## Multiple R-squared:  0.5648,	Adjusted R-squared:  0.5585 
## F-statistic: 89.56 on 1 and 69 DF,  p-value: 4.31e-14
```

## Visualization of Results

### R code

```r
x <- seq(20,60,1)
plot2 <- ggplot()+
  geom_point(aes(data$age, data$income))+
  theme_classic(base_family = "") +
  theme(text=element_text(size=20)) +
  geom_line(aes(x, model$coefficients[2]*x+model$coefficients[1]), col="blue") +
  labs(x="Age", y="Income") +
  ggtitle("Single Linear Regression") +
  ggeasy::easy_center_title()
```

### Results

```r
plot2
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

