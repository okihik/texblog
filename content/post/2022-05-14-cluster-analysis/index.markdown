---
title: (Hierarchical) Cluster Analysis
author: Akihiko Mori
date: '2022-05-14'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose
We will implement cluster analysis (Ward's method) using R. 
Implemented from the generation of data, it can be copied and pasted.

## Contents
1. Prerequisites
2. A single variable case
3. A multivariate case

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
```


## Data Generation

```r
set.seed(1)
english <- c(rnorm(5, 50, 10), rnorm(5, 70, 10)) %>% round()
math <- c(rnorm(5, 50, 10), rnorm(5, 30, 10)) %>% round()
data <- data.frame(english, math)
data %>% head()
```

```
##   english math
## 1      44   65
## 2      52   54
## 3      42   44
## 4      66   28
## 5      53   61
## 6      62   30
```

## Data Visualization

```r
data %>%
  ggplot()+
  geom_point(aes(english, math)) +
  theme_classic(base_family = "")+
  theme(text=element_text(size=20))+
  labs(title="Scatter plot")+
  ggeasy::easy_center_title()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

## Cluster Analysis

```r
D <- dist(data, method = "euclidean")
cls <- hclust(D, method = "ward.D")
plot(as.dendrogram(cls))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

## Analysis visualization

```r
data$cluster <- factor(cutree(cls, k=2))
data %>%
  ggplot()+
  geom_point(aes(english, math, col=cluster)) +
  theme_classic(base_family = "")+
  theme(text=element_text(size=20))+
  labs(title="Scatter plot")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

### LD Analysis

