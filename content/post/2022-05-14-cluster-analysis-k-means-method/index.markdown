---
title: Cluster Analysis (k-means method)
author: Akihiko Mori
date: '2022-05-14'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose
We will implement cluster analysis (k-means method) using R. 
Implemented from the generation of data, it can be copied and pasted.

## Contents
1. Prerequisites
2. Data generation
3. Data visualization
4. Cluster Analysis
5. Analysis visualization

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
cls <- kmeans(data, 2)
cls
```

```
## K-means clustering with 2 clusters of sizes 6, 4
## 
## Cluster means:
##   english math
## 1   70.50 33.5
## 2   47.75 56.0
## 
## Clustering vector:
##  [1] 2 2 2 1 2 1 1 1 1 1
## 
## Within cluster sum of squares by cluster:
## [1] 309.00 346.75
##  (between_SS / total_SS =  78.9 %)
## 
## Available components:
## 
## [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
## [6] "betweenss"    "size"         "iter"         "ifault"
```

## Analysis visualization

```r
data$cluster <- factor(cls$cluster)
data %>%
  ggplot()+
  geom_point(aes(english, math, col=cluster)) +
  theme_classic(base_family = "")+
  theme(text=element_text(size=20))+
  labs(title="Plot")+
  ggeasy::easy_center_title()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />
