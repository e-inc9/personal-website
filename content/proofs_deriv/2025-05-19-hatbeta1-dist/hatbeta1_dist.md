---
title: "Distribution of hat beta_1"
output: html_document
date: '2025-05-19'
categories:
  - Proofs/derivations
---


We want to know the distribution of `\(\hat \beta_1\)` across all possible samples where we treat the `\(X_i\)`'s as fixed each time.

Assume that `\(Y_i \sim(\beta_0 + \beta_1X_i, \sigma^2)\)` as a result of the definition  

`\(Y_i= \beta_0 + \beta_1 X_i + \varepsilon_i\)`, where each `\(\varepsilon_i \sim N(0, \sigma^2)\)`.  

This is what's assumed under the simple linear regression model, **including the additional assumption that `\(\varepsilon\)` is normally distributed (necessary for inference).**

Recall that given a sample,  
\[
\hat{\beta}_1 = \frac{S_{xy}}{S_{xx}} = \frac{\sum(X_i - \bar{X})(Y_i - \bar{Y})}{\sum(X_i - \bar{X})^2}
\]

 `\(Y_i\)` is treated as a random variable, owing to the error term. 

### Defining `\(\hat{\beta}_1\)` Differently

We can redefine the numerator `\(\sum(X_i - \bar{X})(Y_i - \bar{Y})\)` as follows:

\[
\sum(X_i - \bar{X})(Y_i - \bar{Y}) = \sum (X_i - \bar{X}) Y_i - \sum (X_i - \bar{X}) \bar{Y} = \sum (X_i - \bar{X}) Y_i
\]

The second term becomes zero by the definition of the mean. So:

\[
\hat{\beta}_1 = \frac{\sum (X_i - \bar{X}) Y_i }{\sum(X_i - \bar{X})^2} = \sum [ k_i Y_i] 
\]

where:

\[
k_i = \frac{X_i - \bar{X}}{\sum(X_i - \bar{X})^2}
\]

Each `\(k_i\)`, being dependent on `\(X_i\)` that is fixed across all samples, is fixed. Thus, `\(\hat \beta_1\)` is a linear combination of observations `\(Y_i\)` (the random variable) whose coefficients are fixed.

---

### The Useful `\(k\)`

For each `\(x_i\)`, there is a `\(k_i\)`. Consider the following identities:

\[
\sum k_i = \frac{\sum (X_i - \bar{X})}{\sum(X_i - \bar{X})^2} = 0
\]

\[
\sum k_i X_i = \frac{\sum [X_i^2 - X_i \bar{X}]}{\sum(X_i - \bar{X})^2} = \frac{\sum X_i^2 - n \bar{X}^2}{\sum X_i^2 - n \bar{X}^2} = 1
\]

\[
\sum k_i^2 = \frac{\sum (X_i - \bar{X})^2}{\left[\sum (X_i - \bar{X})^2\right]^2} = \frac{1}{\sum (X_i - \bar{X})^2}
\]

---

### Back to `\(\hat{\beta}_1\)`

Since `\(k_i\)` is a constant and `\(Y_i\)` is normal, `\(\hat{\beta}_1 = \sum k_i Y_i\)` is a linear combination of normals _and thus also normally distributed_.

So:

\[
\hat{\beta}_1 \sim N(\mathbb{E}[\hat{\beta}_1], \text{Var}(\hat{\beta}_1))
\]

---

### Expected Value and Variance of `\(\hat{\beta}_1\)`

\[
\mathbb{E}[\hat{\beta}_1] = \mathbb{E}\left[ \sum k_i Y_i \right] = \sum k_i \mathbb{E}[Y_i] = \sum k_i (\beta_0 + \beta_1 X_i)
\]

\[
= \beta_0 \sum k_i + \beta_1 \sum k_i X_i = 0 + \beta_1 = \beta_1
\]

\[
\text{Var}(\hat{\beta}_1) = \text{Var}\left( \sum k_i Y_i \right) = \sum k_i^2 \text{Var}(Y_i) = \sigma^2 \sum k_i^2 = \frac{\sigma^2}{\sum (X_i - \bar{X})^2}
\]

Thus:
\[
\hat{\beta}_1 \sim N \left( \beta_1,\ \frac{\sigma^2}{\sum (X_i - \bar{X})^2} \right)
\]

We note that in addition to being unbiased, our estimator `\(\hat \beta_1\)` is minimum variance. It is MVUE.

In practice, since we don't know `\(\sigma^2\)`, we use the MSE, which is our estimator of `\(\sigma^2\)` given the sample. 

$$
s^2(\hat \beta_1) = \frac{MSE}{\sum(X_i - \bar X)^2} = \frac{\sum(Y_i - \hat Y_i)^2}{n-2} \cdot \frac{1}{\sum(X_i - \bar X)^2}
$$
Notice how the estimated variance of `\(\hat \beta_1\)` decreases with 1) sample size and 2) greater variation in our `\(X_i\)` values about the mean, which is a measure of the range of our `\(X_i\)` observations.

[Read about testing `\(\hat \beta_1\)` here.](/inference_tests/2025-06-17-t-test-linreg/t-test-linreg/)



