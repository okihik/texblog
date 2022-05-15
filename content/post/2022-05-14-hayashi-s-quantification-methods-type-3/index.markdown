---
title: Hayashi's quantification methods type 3
author: Akihiko Mori
date: '2022-05-14'
slug: []
categories: []
tags: 
- Multivariate Analysis with R
---

## Purpose
We will implement Hayashi's quantification methods type 3 using R. 
Implemented from the generation of data, it can be copied and pasted.

## Contents
1. Prerequisites
2. Data generation
3. Data elimination
4. Hayashi's quantification methods type 3
5. Analysis visualization

## Prerequites

### Library

```r
library(dplyr)
library(ggplot2)
library(MASS)
library(ggrepel)
library(gridExtra)
```


## Data Generation

```r
set.seed(1)
eng <- rnorm(15, 50, 10) %>% round()
math <- rnorm(15, 50, 10) %>% round()
sec <- rnorm(15, eng, 5) %>% round()
sci <- rnorm(15, math, 5) %>% round()
soc <- rnorm(15, sec, 3) %>% round()
data <- data.frame(eng, math, sec, sci, soc) %>%
mutate(eng=ifelse(eng>50, 1, 0)) %>%
mutate(math=ifelse(math>50, 1, 0)) %>%
mutate(sec=ifelse(sec>50, 1, 0)) %>%
mutate(sci=ifelse(sci>50, 1, 0)) %>%
mutate(soc=ifelse(soc>50, 1, 0))
data %>% head()
```

```
##   eng math sec sci soc
## 1   0    0   1   0   1
## 2   1    0   1   1   1
## 3   0    1   0   1   0
## 4   1    1   1   1   1
## 5   1    1   0   1   0
## 6   0    1   0   1   0
```

## Data Elimination

```r
data <- data[data %>% apply(1,sum) != 0,]
rownames(data) <- 1:nrow(data)
data %>% head()
```

```
##   eng math sec sci soc
## 1   0    0   1   0   1
## 2   1    0   1   1   1
## 3   0    1   0   1   0
## 4   1    1   1   1   1
## 5   1    1   0   1   0
## 6   0    1   0   1   0
```

## Type 3

```r
model <- corresp(as.matrix(data), nf=2)
model
```

```
## First canonical correlation(s): 0.5559383 0.2722638 
## 
##  Row scores:
##          [,1]         [,2]
## 1   1.7139702  2.624197987
## 2   0.6015372 -0.430233335
## 3  -2.2165520  0.930015060
## 4  -0.0545884 -0.008539853
## 5  -1.2336275 -1.763698413
## 6  -2.2165520  0.930015060
## 7  -0.5423706 -0.852590955
## 8  -0.0545884 -0.008539853
## 9   1.3867207 -0.634243129
## 10 -0.2512909  1.777106524
## 11  0.6015372 -0.430233335
## 12  1.3867207 -0.634243129
## 13 -0.0545884 -0.008539853
## 
##  Column scores:
##            [,1]        [,2]
## eng   0.4070700 -1.94699287
## math -1.4894093  0.45692246
## sec   0.8513641  0.51205516
## sci  -0.9751232  0.04949649
## soc   1.0543595  0.91689329
```

## Analysis visualization

```r
p1 <- ggplot() +
  geom_text_repel(aes(model$rscore[,1],
                      model$rscore[,2],
                      label=1:nrow(data)), size=5) + 
  theme_classic(base_family = "")+
  theme(text=element_text(size=20)) +
  labs(title="plot", x=expression(x[1]), y=expression(x[2]))

p2 <- ggplot() +
  geom_text_repel(aes(model$cscore[,1],
                      model$cscore[,2],
                      label=colnames(data)), size=5, family="") + 
  theme_classic(base_family = "")+
  theme(text=element_text(size=20)) +
  labs(title="plot", x=expression(y[1]), y=expression(y[2]))

#grid.arrange(p1, p2)
p1; p2
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-2.png" width="672" />
