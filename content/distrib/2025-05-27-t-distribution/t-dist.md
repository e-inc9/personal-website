---
title: T-distribution
date: '2025-05-27'
slug: t-dist
categories:
  - Distributions
tags: []
---


``` r
knitr::opts_chunk$set(echo = FALSE)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

``` r
library(ggplot2)
rm(list=ls())
set.seed(1)
```


A t-distributed random variable with `\(\nu\)` degrees of freedom is the ratio of a standard normal variable `\(Z \sim N(0,1)\)` over the square root of a chi-squared variable `\(Y \sim \chi^2_{\nu}\)` divided by its degrees of freedom.

$$
T_\nu = \frac{Z}{\sqrt{{Y \over \nu}}}, \quad Z \perp Y
$$

Its pdf is given by

\[ f(t) = \frac{1}{\sqrt{\pi\nu} \Gamma({\nu \over 2})}\
\cdot \frac{1}{\left({t^2 \over \nu} + 1\right)^{\frac{\nu+1}{2}} } \cdot \Gamma\left({\nu+1 \over 2}\right) \]

[Derivation if you're interested.](/proofs_deriv/2025/05/27/deriving-the-t-pdf/)

The pdf isn't as important as the distribution itself. This distribution is bell-shaped and symmetric about 0 and *approaches normality* as `\(\nu\)` increases. 

We can demonstrate this mathematically using the properties of `\(Y\)`, which are downstream from the properties of the Gamma distribution. 
Given that a Gamma random variable has distribution `\(G \sim Gamma(\alpha\beta, \alpha\beta^2)\)`, we apply the law of large numbers: 

\[E(Y)_{\nu \rightarrow \infty} = \sum_{1}^{\infty} E(Z^2) = \sum_{1}^{\infty} \frac{\nu 2 }{2} = \nu\]
\[T_{\nu \rightarrow \infty} = \frac{Z}{\sqrt{{\nu \over \nu}}} = Z\]

<img src="/distrib/2025-05-27-t-distribution/t-dist_files/figure-html/unnamed-chunk-2-1.png" width="672" />


