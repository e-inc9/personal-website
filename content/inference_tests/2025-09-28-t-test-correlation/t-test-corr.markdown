---
title: Correlation
author: ''
date: '2025-09-28'
slug: t-test-corr
categories:
  - Inference/tests
tags: []
---

## Correlation
We consider the definition of population correlation between variables `\(X,Y\)`

$$
\rho_{XY} = \frac{Cov(X,Y)}{\sigma_X \sigma_Y}
$$

## Estimator
The Pearson product-moment correlation coefficient -- `\(r_{XY}\)` -- is an estimator for `\(\rho_{XY}\)`. It is biased, but the bias is small when `\(n\)` is large. Like `\(\rho\)`, it has range `\([-1, 1]\)` and the interpretation is the same.

$$
r_{XY} = \frac{ \sum(X_i - \bar X)(Y_i - \bar Y)}{\sqrt{\sum(X_i - \bar X)^2}\sqrt{\sum(Y_i - \bar Y)^2} }
$$

## Inference 
**When `\(X,Y\)` are bivariate normally distributed** (and with appropriately large n), an appropriate test statistic is 

If we wish to test 
$$
H_0: \rho = 0 \\
H_a: \rho \ne 0 
$$

We can use test statistic
$$
t = \frac{r_{XY}\sqrt{n-2}}{\sqrt{1-r^2_{XY}}} \sim t(n-2)
$$

It turns out that the above `\(H_0, H_a\)` is equivalent to testing whether `\(\beta_{XY} = \beta_{YX} = 0\)` and vice versa. So we could use [t-tests for the slope of linear regression](http://localhost:4321/inference_tests/2025-06-17-t-test-linreg/t-test-linreg/) to test `\(\rho_{XY}\)` as well.

This is because under the bivariate normal distribution assumption, we use [normal correlation models](/overview/2025-09-26-normal-correlation-models/correlation./) whose conditional distributions have the same structure as simple linear regression, with 

$$ 
E[Y|X] = \alpha_{Y|X} + \beta_{Y|X} X
$$

Where `\(\beta_{Y|X} = \rho_{XY} \frac{\sigma_X}{\sigma_Y}\)`.

## Interval estimation (Fisher's z-transformation)
The above test is applicable when we test for `\(H_0: \rho=0\)`. If we'd like to test whether `\(H_0: \rho = \rho_0\)`, where `\(\rho_0\)` is non-zero, we cannot use the above test. We have to use Fisher's z-transformation. 

This is a departure from situations where we're testing a different parameter, like `\(\mu\)`. In that case, we know that the shape of the distribution of the estimator is normal (owing to CLT), and it remains normal across `\(\mu_0\)`. So when we estimate `\(\bar x\)`, we use the critical values of a normal distribution to determine the confidence interval. 

However, when we attempt to perform inference as to `\(\rho_0\)`, the distribution of `\(r\)` depends on `\(\rho\)` itself. If `\(\rho\)` is near 1, our `\(r\)` certainly does not follow a normal distribution (it cannot exceed 1), and thus is compressed and skewed. Thus, we cannot proceed with a test statistic that assumes symmetry and unbounded-ness.

What we can do, however, is come up with a transformation involving `\(r\)` that follows a normal distribution. This is Fisher's z transformation, which maps each `\(r\)` to a unique `\(z'\)`. The resulting distribution of `\(z'\)` is approximately normal when `\(n \ge 25\)`.

$$
z' = \frac{1}{2}log_e\left( \frac{1+r_{12}}{1-r_{12}} \right) \\ 
E\{z'\} = \xi = \tfrac12 \log_{e}\!\left(\frac{1+\rho_{12}}{\,1-\rho_{12}\,}\right)\\
\sigma^{2}\{z'\} = \frac{1}{\,n-3\,}
$$

