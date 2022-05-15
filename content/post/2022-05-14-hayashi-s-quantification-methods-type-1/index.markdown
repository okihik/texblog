---
title: Hayashi's quantification methods type 1
author: Akihiko Mori
date: '2022-05-14'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose
We will implement Hayashi's quantification methods type 1 using R. 
Implemented from the generation of data, it can be copied and pasted.

## Contents
1. Prerequisites
2. Data generation
3. Data visualization
4. Data transformation
5. Hayashi's quantification methods type 1

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
```


## Data Generation

```r
set.seed(100)
N <- 30
lec <- c(rep("lecA", N), rep("lecB", N), rep("lecC", N))
e1 <- e2 <- e3 <- rnorm(N, 0, 10)
data <- data.frame(lec) %>% 
  mutate(test=ifelse(lec=="lecA", 50+e1, 
                     ifelse(lec=="lecB", 30+e2, 
                            80+e3))) %>%
  mutate(test=round(test))
data %>% sample_n(5)
```

```
##    lec test
## 1 lecB   38
## 2 lecB   31
## 3 lecC   68
## 4 lecA   58
## 5 lecB   28
```

## Data Visualization

```r
data %>%
  ggplot() +
  geom_point(aes(lec, test)) +
  theme_classic(base_family = "") +
  theme(text=element_text(size=20)) +
  labs(title="Scatter plot", x=NULL)+
  ggeasy::easy_center_title()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

## (Data transformation)

```r
data %>%
  mutate(lecB = ifelse(lec=="lecB", 1, 0)) %>%
  mutate(lecC = ifelse(lec=="lecC", 1, 0)) %>%
  sample_n(5)
```

```
##    lec test lecB lecC
## 1 lecA   44    0    0
## 2 lecC   79    0    1
## 3 lecC   81    0    1
## 4 lecA   42    0    0
## 5 lecA   73    0    0
```

## Analysis visualization

```r
model1 <- lm(data=data, test~lec)
summary(model1)
```

```
## 
## Call:
## lm(formula = test ~ lec, data = data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -12.2667  -4.2667   0.7333   2.7333  22.7333 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   50.267      1.278   39.32   <2e-16 ***
## leclecB      -20.000      1.808  -11.06   <2e-16 ***
## leclecC       30.000      1.808   16.59   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 7.002 on 87 degrees of freedom
## Multiple R-squared:  0.8991,	Adjusted R-squared:  0.8968 
## F-statistic: 387.5 on 2 and 87 DF,  p-value: < 2.2e-16
```
