---
title: Hayashi's quantification methods type 2
author: Akihiko Mori
date: '2022-05-14'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---
## Purpose
We will implement Hayashi's quantification methods type 2 using R. 
Implemented from the generation of data, it can be copied and pasted.

## Contents
1. Prerequisites
2. Data generation
3. Data visualization
4. Data transformation
5. Hayashi's quantification methods type 2

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
library(MASS)
```


## Data Generation

```r
set.seed(10)
N <- 30
lec <- c(rep("lecA", N), rep("lecB", N), rep("lecC", N))
e1 <- e2 <- e3 <- rnorm(N, 0, 10)
res <- c("fail", "pass")
results <- c(res[rbinom(N,1,0.5)+1], res[rbinom(N,1,0.2)+1], res[rbinom(N,1,0.8)+1])
data <- data.frame(lec, results)
data %>% sample_n(5)
```

```
##    lec results
## 1 lecB    fail
## 2 lecC    pass
## 3 lecC    pass
## 4 lecA    fail
## 5 lecB    fail
```

## Data Visualization

```r
table(data)
```

```
##       results
## lec    fail pass
##   lecA   19   11
##   lecB   24    6
##   lecC    7   23
```

## (Data transformation)

```r
data %>% 
  mutate(lecB = ifelse(lec=="lecB", 1, 0)) %>%
  mutate(lecC = ifelse(lec=="lecC", 1, 0)) %>%
  sample_n(5)
```

```
##    lec results lecB lecC
## 1 lecB    fail    1    0
## 2 lecB    pass    1    0
## 3 lecB    fail    1    0
## 4 lecC    pass    0    1
## 5 lecA    fail    0    0
```

## Analysis visualization

```r
lda(data=data, results~lec)
```

```
## Call:
## lda(results ~ lec, data = data)
## 
## Prior probabilities of groups:
##      fail      pass 
## 0.5555556 0.4444444 
## 
## Group means:
##      leclecB leclecC
## fail    0.48   0.140
## pass    0.15   0.575
## 
## Coefficients of linear discriminants:
##                LD1
## leclecB -0.7893169
## leclecC  1.8943606
```
