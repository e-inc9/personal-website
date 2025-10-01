---
title: Simple linear regression (with normality)
date: '2025-09-14'
slug: simple-linear-regression-with-normality
categories:
  - General
---
We make one additional assumption to the [simple linear regression assumptions](/overview/2025-05-16-linreg-simple/linreg-simple/), which is that the error variable `\(\varepsilon_i\)` is _normally distributed_. Its center is still 0, and its variance is still `\(\sigma^2\)`. It remains independent and identically distributed across levels of `\(X\)`. 

As a result, `\(Y_i\)` is also normally distributed with `\(E(Y_i) = \beta_0 + \beta_1X_i, V(Y_i) = \sigma^2\)`.

## Adding the assumption of normality

$$ \varepsilon_i \sim N(0, \sigma^2) \implies (\beta_0 + \beta_1X_i + \varepsilon_i)\sim N(\beta_0+\beta_1X_i+0, \sigma^2)\implies Y_i =Y|X_i \sim(\beta_0 + \beta_1 X_i, \sigma^2) $$
`\(Y_i\)` or `\(Y|X_i\)` is normally distributed about a line describing a
linear relationship. Its variance is the same across
`\(X_i\)`'s.

It might seem like a huge assumption to assume that `\(E(\varepsilon) = 0\)` --
in other words, that on average, we'd expect nothing else to affect `\(Y\)`
but `\(X\)` -- but note that this in no way precludes the observation of
nonzero `\(\varepsilon_i\)` affecting `\(Y\)` . Also, note that in most real-world
applications of linear regression, `\(X\)` is not the only variable. See:
[multivariate linear regression](linreg-multi.html), where once you have
many variables, it's more plausible that the expected impact of
"everything else" is zero. But perhaps this is a larger discussion of
"expected value"...

### Linear regression model

The resulting model is:

$$ Y|X = \beta_0 + \beta_1X + \varepsilon, \varepsilon \sim  N(0, \sigma^2) \implies Y|X \sim N(\beta_0+\beta_1X, \sigma^2) $$
Which puts us in the perfect position to conduct inference on `\(Y_i\)`'s. I
mean, look at the picture below. There exists a distribution for every
single point!



# Testing our model
[See here.](/inference_tests/2025-06-17-t-test-linreg/t-test-linreg)

# Caveats
**Again, note that normality of residuals is not required for the OLS method, but IS required for inference using the linear model. (Kutner p.26) **


Linear regressions can be misapplied in the case of: 
- Nonlinear relationships
- Making inferences about `\(Y|X_i\)` when `\(X_i\)` is beyond the
range of the sample `\(X\)` used to fit the model
