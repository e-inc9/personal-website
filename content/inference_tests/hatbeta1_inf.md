---
title: "Inference with simple linear regression coefficients"
output: html_document
date: "2025-05-27"
categories:
- Inference/tests
---

What if I told you that just as the parameters mean and variance  follow a distribution, so do the coefficients `\(\hat \beta_0, \hat \beta_1\)` [we previously found](/2025-05-16-linreg-simple/linreg-simple)? Even though it can be argued that there does not exist a "true" `\(\beta_0\)` and `\(\beta_1\)` in the same way a true `\(\mu\)` might (the former comes from an assumption-laden construct that we use to interpret reality while the other objectively exists in reality), we cannot deny the fact that for different samples of a given phenomena, the line fitted will differ as well. Just as we would not state a sample mean as fact, we would not say that the observed effect of `\(\hat \beta_1\)` is the true effect! In both cases, we acknowledge its tendency to vary, which is useful in itself.

Something to keep in mind when exploring these distributions is that our findings are "downstream" of the assumptions of the linear regression model.

## The distribution of hat beta_1, hat beta_0:
\[
\hat{\beta}_1 \sim N\left( \beta_1,\ \frac{\sigma^2}{\sum (X_i - \bar{X})^2} \right)
\]

[Hat beta_1's full derivation with steps, for those interested.](proofs_deriv/2025/05/19/distribution-of-hat-beta_1/)

\[
\hat{\beta}_0 \sim N\left( \beta_0,\ \sigma^2 \left[ \frac{1}{n} + \frac{\bar{X}^2}{S_{xx}} \right] \right)
\]

[Hat beta_0's full derivation with steps, for those interested.](proofs_deriv/2025/05/19/distribution-of-hat-beta_1/)

Given a sample, we use the sample mean squared error -- our estimate of `\(\varepsilon_i\)`'s variance  (and by extension `\(Y_i\)`'s) -- in place of `\(\sigma^2\)`. 

\[ MSE = {SSE \over n-2} = \frac{\sum(\hat Y_i - Y_i)^2}{n-2} \]

This give us the *studentized* distributions of `\(\hat \beta_1, \hat \beta_0\)`

##  The studentized distribution of hat beta_1, hat beta_0:
