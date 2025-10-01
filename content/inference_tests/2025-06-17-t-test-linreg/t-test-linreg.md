---
title: "T-test (linear regressions)"
output: html_document
date: "2025-06-17"
categories:
- Inference/tests
---

Just as the estimators sample mean and sample variance follow a distribution, so do the estimators  `\(\hat \beta_0, \hat \beta_1\)`. 

Even though it can be argued that there does not exist a "true" `\(\beta_0\)` and `\(\beta_1\)` in the same way a true `\(\mu, \sigma\)` might (the former comes from an assumption-laden construct that we use to interpret reality while the other objectively exists in reality), we could say that having identified phenomena for which a linear regression is applicable, there  _does_ exist a regression line that exists for all existing "population" data on this phenomena. We'd like to infer this `\(\beta_0, \beta_1\)` that's "out there".

Something to keep in mind when exploring these distributions is that our findings are "downstream" of the assumptions of the linear regression model, **including (most crucially) the additional assumption that `\(\varepsilon\)` is normally distributed**. Without that assumption, we cannot perform a t-test (and other inference) on our linear regression model.

## The distribution of `\(\hat \beta_1, \hat \beta_0\)`:
\[
\hat{\beta}_1 \sim N\left( \beta_1,\ \frac{\sigma^2}{S_{xx}} \right)
\]

[See `\(\hat \beta_1\)`'s full derivation with steps.](/proofs_deriv/2025/05/19/distribution-of-hat-beta_1/)

\[
\hat{\beta}_0 \sim N\left( \beta_0,\ \sigma^2 \left[ \frac{1}{n} + \frac{\bar{X}^2}{S_{xx}} \right] \right)
\]

[See `\(\hat \beta_0\)`'s full derivation with steps.](/proofs_deriv/2025/05/23/distribution-of-hat-beta_0/)

### Inference on `\(\beta_1\)`:
Since `\(\hat \beta_1\)` is normally distributed, we can use a standardized test statistic that is also normally distributed for inference, where `\(\sigma^2_{\hat \beta_1} = \frac{\sigma^2}{S_{xx}}\)`. Recall that `\(\sigma^2\)` is the variance of `\(\varepsilon_i\)` and `\(Y_i\)`.
$$
 \frac{\hat \beta_1 - \beta_1}{\sigma_{\hat \beta_1}} \sim N(0,1)
$$
However, as with inference on a sample mean, we don't know `\(\sigma^2_{\hat \beta_1}\)` because we don't know `\(\sigma^2\)`. So we use our estimator `\(MSE = \frac{\sum(Y_i - \hat Y)^2}{n-2}\)` in place of `\(\sigma^2\)`. This gives us a studentized t-statistic. As the name suggests, it follows a [ `\(t\)` distribution](/distrib/2025-05-27-t-distribution/t-dist/).

$$
 \frac{\hat \beta_1 - \beta_1}{\sqrt{\frac{\sum(Y_i - \hat Y)^2}{(n-2)S_{xx}}}} =  \frac{\hat \beta_1 - \beta_1}{\sqrt{\frac{MSE}{S_{xx}}}} \sim t(n-2)
$$

We primarily care about making inferences for `\(\hat \beta_1\)`. After all, that measures the expected effect of `\(X\)` on `\(Y\)`.

### Fitting a linear regression model (estimating `\(\hat \beta_1, \hat \beta_0\)`)
<details> 
<summary>We look at the 25-observation dataset toluca from the api2lm library.</summary>


``` r
library(api2lm)
tol <- toluca
head(tol)
```

```
##   lot_size work_hours
## 1       80        399
## 2       30        121
## 3       50        221
## 4       90        376
## 5       70        361
## 6       60        224
```

``` r
mod <- lm(work_hours~lot_size, data=tol)
mod$coefficients 
```

```
## (Intercept)    lot_size 
##   62.365859    3.570202
```

``` r
b1 <- unname(mod$coefficients[2])
n <- nrow(tol)
```

The fitted linear regression model is `\(\hat Y = 3.57X + 62.57\)`. For a 1 unit increase in lot size, we expect work hours to increase by approximately `\(\hat \beta_1 =  3.6\)` hours. Were we to obtain a different sample, this `\(\hat \beta_1\)` would differ. How do we learn about `\(\beta_1\)`, the population effect of `\(X\)`, on `\(Y\)` from our estimate of it? We can a) test what it's *not* or b) find a range which `\(\beta_1\)` would reside in.
</details>

### Hypothesis testing (value of `\(\beta_1\)`)

<details>
<summary>We test whether `\(\beta_1 = 0\)` (two-tailed test).</summary>

\[ H_0: \beta_1 = 0\]
\[ H_a: \beta_1 \ne 0 \]

Our test statistic is

\[ T= \frac{ 3.57  - 0}{ \sqrt{\frac{MSE}{S_{xx}}}} \]

Based on the low p-value, we reject the null hypothesis (`\(\alpha = .05\)`).


``` r
SSE <- sum((tol$work_hours -fitted(mod))^2)
MSE <- SSE / (n-2)
S_xx <- sum((tol$lot_size - mean(tol$lot_size))^2)

#estimate of standard deviation of hat beta_1
SE_beta1 <- sqrt(MSE/S_xx)

#test statistic
stat <- (b1-0)/SE_beta1; stat
```

```
## [1] 10.28959
```

``` r
#p-value (two-sided)
pt(stat, n-2, lower.tail=FALSE)*2
```

```
## [1] 4.448828e-10
```

We can verify our test statistic and p-value:

``` r
summary(mod)
```

```
## 
## Call:
## lm(formula = work_hours ~ lot_size, data = tol)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -83.876 -34.088  -5.982  38.826 103.528 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   62.366     26.177   2.382   0.0259 *  
## lot_size       3.570      0.347  10.290 4.45e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 48.82 on 23 degrees of freedom
## Multiple R-squared:  0.8215,	Adjusted R-squared:  0.8138 
## F-statistic: 105.9 on 1 and 23 DF,  p-value: 4.449e-10
```

Next, we test whether `\(\beta_1  \ge  2\)` (one-tailed test).

\[ H_0: \beta_1  = 2\]
\[ H_a: \beta_1 >  2 \]

Based on the low p-value, we reject the null hypothesis (`\(\alpha = .05\)`).

``` r
#test statistic
stat <- (b1-2)/SE_beta1

#p-value (one-sided)
pt(stat, n-2, lower.tail=FALSE)
```

```
## [1] 7.595892e-05
```

</details>

### 95% confidence interval:

<details>
<summary>Given that `\(\hat \beta_1\)` follows a t-distribution, our confidence interval is determined by the `\(1-\alpha%\)` critical values. </summary>


``` r
#critical value:
crit <- qt(.975, df=n-2, lower.tail=TRUE); crit
```

```
## [1] 2.068658
```

``` r
#lower bound:
b1 - crit * SE_beta1
```

```
## [1] 2.852435
```

``` r
# upper bound:
b1 + crit * SE_beta1
```

```
## [1] 4.287969
```

With confidence coefficient 0.95, our estimate of the mean number of work hours increases between 2.85 and 4.28 hours for each additional unit of lot size.

</details>

## The distribution of `\(\hat Y_h\)`
Say we would like to know `\(Y_h\)` at a fixed `\(X_h\)`. Across multiple samples of size `\(n\)` with fixed `\(X_i\)`'s, our `\(Y_h\)`'s would necessarily vary.

We do know that `\(Y_i\)` is normally distributed. Furthermore, its expected value is `\(E(Y_h) = \beta_0 + \beta_1X_h + 0\)`, the population value. Its variance is rather unwieldy.

$$
\hat Y_h \sim N\left(E(Y_h),\quad \sigma^2\left[\frac{1}{n} + \frac{(X_h - \bar X)^2}{\sum (X_i - \bar X)^2}\right]\right)
$$

Note that the variance is lessened by the dispersion of the `\(X_i\)`'s collected, but increased by the `\(X_h\)` of interest's distance from `\(\bar X\)`. This latter component is visually explained below. For two fitted lines that have the same `\((\bar X, \bar Y)\)` and thus intersect at that point, the difference in `\(\hat Y_h\)`'s for an `\(X_h\)` far from `\(\bar X\)` will be greater the further `\(X_h\)` is from `\(\bar X\)`.

<img src="../../../../../../../../pics/divergence.png" width="70%" />

### Inference on `\(E(Y_h)\)`
Like `\(\hat \beta_1\)`, `\(\hat Y_h\)` is normally distributed. We can use a studentized statistic, once again subbing in MSE for `\(\sigma^2\)`. What MSE is multiplied by is a constant.

$$
\frac{\hat Y_h - E(Y_h)}{MSE[\frac{1}{n} + \frac{(X_h - \bar X)^2}{\sum (X_i - \bar X)^2}]}\sim t(n-2)
$$

## Prediction of `\(Y_h\)`
In the previous section, we wanted to infer the _expected_ value of `\(Y_h\)` at `\(X_h\)` --  the `\(Y_h\)` that is the _center_ of the sampling distribution of `\(\hat Y_h\)`'s that could be found across samples of size `\(n\)` and fixed values `\(X_i\)`. Now, we'd like to predict  `\(Y_{hnew}\)`. This is different from trying to estimate the center of a distribution (a parameter). We are now trying to estimate an _individual outcome_ (a random variable) on a distribution.

We thus have to account for two types of variance:
a) variance of the location of the distribution of `\(\hat Y_{hnew}\)` (which is our estimate of `\(Y_{hnew}=E(Y|X_{hnew})\)`)
b) variance of `\(Y_{hnew}\)` about `\(\hat Y_{hnew}\)` (within the probability distribution)

We can express the above in mathematical terms:

$$ \begin{aligned}
\sigma^2({pred}) = \sigma^2({Y_h - \hat Y_h}) = \sigma^2({Y_h)} + \sigma^2({\hat Y_h}) = \sigma^2 + \sigma^2\left[\frac{1}{n} + \frac{(X_h - \bar X)^2}{\sum (X_i - \bar X)^2}\right] = \sigma^2\left[ 1+\frac{1}{n} + \frac{(X_h-\bar X)^2}{S_{xx}} \right]
\end{aligned} $$

Of course, we substitute `\(MSE\)` in for `\(\sigma^2\)`. Notice how this variance is larger than that for estimating `\(E(\hat Y_h)\)`. This should make sense. An individual outcome from a distribution has higher variation than the center of said distribution.

Our test statistic then looks like:
$$
\frac{{Y_{hnew} - \hat Y_h}}{s_{pred}}\sim t(n-2)
$$

It's worth noting that an interval created from this statistic is sensitive to departures from normality.





