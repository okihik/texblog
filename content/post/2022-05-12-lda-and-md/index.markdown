---
title: LDA and MD
author: Akihiko Mori
date: '2022-05-12'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose

We will implement single regression analysis using R. 
Implemented from the creation of data, the code can be copied and pasted.

## Contents
1. Prerequisites
2. A single variable case
3. A multivariate case

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
library(MASS)
```

## A single variable case

### Data Generation

```r
set.seed(1)
sales <- c(rnorm(100, 2000, 70), rnorm(100, 1700, 70)) %>% round(1)
customers <- c(rep("customer", 100), rep("exiter", 100))
data <- data.frame(sales, customers)
data %>% head()
```

```
##    sales customers
## 1 1956.1  customer
## 2 2012.9  customer
## 3 1941.5  customer
## 4 2111.7  customer
## 5 2023.1  customer
## 6 1942.6  customer
```

### Data Visualization

```r
data %>%
  ggplot(aes(sales, fill=customers))+
  geom_histogram(col="white")+
  theme_classic(base_family = "")+
  theme(text=element_text(size=20)) +
  labs(title="Histogram")+
  ggeasy::easy_center_title()+
  scale_x_continuous(breaks=seq(1600,2100, 200))+
  geom_vline(xintercept = mean(filter(data, customers=="exiter")$sales), size=2) +
  geom_vline(xintercept = mean(filter(data, customers=="customer")$sales), size=2)
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

### LD Analysis

```r
data1 <- filter(data, customers=="customer")$sales
data2 <- filter(data, customers=="exiter")$sales
D1 <- (data$sales - mean(data1))^2/var(data$sales)
D2 <- (data$sales - mean(data2))^2/var(data$sales)
data$D1 <- D1
data$D2 <- D2
data$disc <- D1 < D2
data %>% sample_n(5)
```

```
##    sales customers        D1         D2  disc
## 1 1732.0    exiter 2.6758339 0.04226957 FALSE
## 2 1910.6  customer 0.3315807 1.60164931  TRUE
## 3 2082.5  customer 0.1974675 5.22472816  TRUE
## 4 1731.5    exiter 2.6855509 0.04105819 FALSE
## 5 2102.6  customer 0.3177171 5.78430036  TRUE
```

## A multivariate case

### Data generation

```r
set.seed(1)
sales <- c(rnorm(50, 2000, 200), rnorm(50, 1700, 200)) %>% round(1)
counts <- c(rpois(50, 30), rpois(50, 20))
customers <- c(rep("customer", 50), rep("exiter", 50))
data <- data.frame(sales, counts, customers)
data %>% sample_n(5)
```

```
##    sales counts customers
## 1 1822.1     15    exiter
## 2 1862.2     25  customer
## 3 1840.0     25    exiter
## 4 1605.3     19    exiter
## 5 1449.3     29    exiter
```

### Data visualization

```r
data %>%
  ggplot()+
  geom_point(aes(sales, counts, col=customers))+
  geom_point(aes(mean(filter(data,customers=="customer")$sales),
                 mean(filter(data,customers=="customer")$counts)),
  col="red", size=5)+
  geom_point(aes(mean(filter(data,customers=="exiter")$sales),
                 mean(filter(data,customers=="exiter")$counts)),
  col="blue", size=5)+
  theme_classic(base_family = "")+
  theme(text=element_text(size=20)) +
  labs(title="Scatter Plot")+
  ggeasy::easy_center_title()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

### LD Analysis

```r
X <- data[,c(1,2)]
mu1 <- apply(filter(data[,c(1,2)], customers=="customer"), 2, mean)
mu2 <- apply(filter(data[,c(1,2)], customers=="exiter"), 2, mean)
V <- cov(X)
D1 <- t(t(X)-mu1) %*% solve(V) %*% (t(X)-mu1)
D2 <- t(t(X)-mu2) %*% solve(V) %*% (t(X)-mu2)
D1 <- mahalanobis(X,mu1,V)
D2 <- mahalanobis(X,mu2,V)
data$D1 <- D1
data$D2 <- D2
data$disc <- D1 < D2
data %>% sample_n(5)
```

```
##    sales counts customers        D1        D2  disc
## 1 2156.4     33  customer 0.4920576 5.1831343  TRUE
## 2 1811.7     14    exiter 4.5255446 1.1414758 FALSE
## 3 1832.9     30  customer 1.0877852 2.7750374  TRUE
## 4 1779.6     23    exiter 1.1941112 0.3797994 FALSE
## 5 2164.2     24  customer 1.6353499 3.6005419  TRUE
```
