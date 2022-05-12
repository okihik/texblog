---
title: Linear Discriminant Analysis
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
2. Generating Data
3. Regression Analysis
4. Multicollinearity

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
library(MASS)
```

### Assumptions

## Data

### R Code

```r
set.seed(1)
sales <- c(rnorm(50, 2000, 200), rnorm(50, 1700, 200)) %>% round(1)
counts <- c(rpois(50, 30), rpois(50, 20))
customer <- c(rep("Regulaters", 50), rep("Exiters", 50))
data <- data.frame(sales, counts, customer)
```

### Results

```r
data %>% head()
```

```
##    sales counts   customer
## 1 1874.7     26 Regulaters
## 2 2036.7     26 Regulaters
## 3 1832.9     30 Regulaters
## 4 2319.1     26 Regulaters
## 5 2065.9     22 Regulaters
## 6 1835.9     34 Regulaters
```

## Visulization of Data

### R code

```r
data %>%
  ggplot() +
  geom_point(aes(sales, counts, col=customer)) +
  theme_classic(base_family = "") +
  theme(text=element_text(size=20)) +
  ggtitle("Scatter Plot") +
  ggeasy::easy_center_title()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />


## LD Analysis

### R code

```r
model <- lda(data=data, customer~sales+counts)
```
### Results

```r
model
```

```
## Call:
## lda(customer ~ sales + counts, data = data)
## 
## Prior probabilities of groups:
##    Exiters Regulaters 
##        0.5        0.5 
## 
## Group means:
##               sales counts
## Exiters    1723.466  18.76
## Regulaters 2020.086  28.68
## 
## Coefficients of linear discriminants:
##                LD1
## sales  0.003321201
## counts 0.159440308
```

## Visulization of Analysis

### R code

```r
a0 <- - apply(model$means %*% model$scaling,2,mean)
x <- seq(1400, 2200,1)
y <- -model$scaling[1]/model$scaling[2]*x - a0/model$scaling[2]
mu <- apply(model$means, 2, mean)

plot <- ggplot()+
  geom_point(aes(data$sales, data$counts, col=customer))+
  geom_point(aes(mu[1], mu[2]), size=3, col="blue") +
  theme_classic(base_family = "")+
  theme(text=element_text(size=20)) +
  labs(x="Sales", y="Counts") +
  geom_line(aes(x,y), col="blue") + 
  ggtitle("Scatter Plot") +
  ggeasy::easy_center_title()
```

### Result

```r
plot
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

