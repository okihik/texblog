---
title: Principal component analysis
author: Akihiko Mori
date: '2022-05-14'
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
2. Two variables
3. A multivariate case

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
```

## Two variables

### Data Generation

```r
set.seed(10)
English <- rnorm(100, 50, 10) %>% round()
Math <- rnorm(100, English, 10) %>% round()
data <- data.frame(English, Math)
data %>% head()
```

```
##   English Math
## 1      50   42
## 2      48   52
## 3      36   26
## 4      44   51
## 5      53   47
## 6      54   60
```

### Data Visualization

```r
data %>%
  ggplot() +
  geom_point(aes(English, Math))+
  theme_classic(base_family = "")+
  theme(text=element_text(size=20)) +
  labs(title="Scatter plot")+
  ggeasy::easy_center_title()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

### PCA

```r
model <- prcomp(data, scale = TRUE)
summary(model)
```

```
## Importance of components:
##                           PC1    PC2
## Standard deviation     1.2933 0.5722
## Proportion of Variance 0.8363 0.1637
## Cumulative Proportion  0.8363 1.0000
```

### Scares

```r
z <- model$rotation[,1] %*% t(scale(data)) %>% round(2) %>% t() %>% as.data.frame()
colnames(z) <- "z"
z %>% head()
```

```
##       z
## 1  0.20
## 2 -0.19
## 3  2.11
## 4  0.17
## 5 -0.29
## 6 -1.07
```

## A multivariate case

### Data generation

```r
set.seed(10)
English <- rnorm(100, 50, 10) %>% round()
Math <- rnorm(100, 50, 10) %>% round()
Second <- rnorm(100, English, 5) %>% round()
Science <- rnorm(100, Math, 5) %>% round()
Social <- rnorm(100, Second, 3) %>% round()
data <- data.frame(English, Math, Second, Science, Social)
data %>% head()
```

```
##   English Math Second Science Social
## 1      50   42     56      50     56
## 2      48   54     50      57     54
## 3      36   40     43      37     50
## 4      44   57     48      61     49
## 5      53   44     48      45     46
## 6      54   56     56      58     50
```

### PCA

```r
model <- prcomp(data, scale = TRUE)
summary(model)
```

```
## Importance of components:
##                           PC1    PC2     PC3     PC4     PC5
## Standard deviation     1.6832 1.3553 0.42296 0.33797 0.19152
## Proportion of Variance 0.5666 0.3674 0.03578 0.02285 0.00734
## Cumulative Proportion  0.5666 0.9340 0.96982 0.99266 1.00000
```

### Biplot

```r
biplot(model, family="HiraKakuPro-W3")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

### Component scores

```r
z <- t(model$rotation[,1:2]) %*% t(scale(data)) %>% round(2) %>% t() %>% as.data.frame()
colnames(z) <- c("z1", "z2")
z %>% head()
```

```
##      z1    z2
## 1 -0.96 -0.32
## 2 -0.12  0.83
## 3  0.64 -1.71
## 4  0.58  1.15
## 5 -0.24 -0.62
## 6 -0.54  1.17
```

