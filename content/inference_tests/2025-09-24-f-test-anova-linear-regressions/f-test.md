---
title: F-test (ANOVA/linear regressions)
date: '2025-09-24'
slug: f-test
categories: 
  - Inference/tests
tags: []
---

In the [ANOVA cut of linear regression](/distrib/2025-09-23-simple-linear-regression-anova-cut/linreg-anova/), we ended on `\(MSR\)` and `\(MSE\)`, which are two measures of variance. `\(MSR\)` measures the variance explained by a fitted line, while `\(MSE\)` measures the variance of observations about that (in spite of) the fitted line. 

Given that they are both random variables with centers:
$$
E(MSE)= \sigma^2\\
E(MSR) = \sigma^2 + \beta_1 ^2 \sum(X_i - \bar X)^2
$$
We conclude that the distribution of `\(MSR\)` is _to the right of_ the distribution of `\(MSE\)` if `\(\beta_1 \ne 0\)`, and thus computed `\(MSR\)` is likely to be greater than `\(MSE\)`. If `\(\beta_1 = 0\)`, then `\(MSR\)` is likely to equal `\(MSE\)`.

**Note that this intuition cannot be understood (visually) from an image, as `\(MSE\)` is `\(\sum( Y_i - \hat Y_i)^2\)` DIVIDED by `\(n-2\)`!**

### The F-statistic
We test the likelihood of `\(\beta_1 = 0\)` using `\(F = \frac{MSR}{MSE}\)`.

As discussed above, if `\(\beta_1 = 0\)`, then `\(MSR\)` is likely to equal `\(MSE\)`, which means `\(F\)` is likely to be 1. If `\(\beta_1 \ne 0\)`, then `\(MSR\)` is likely to be greater than `\(MSE\)`. The greater the departure of `\(\beta_1\)` from `\(0\)`, the larger `\(F\)` is likely to be. 

When we use the words "likely" and "likelihood", we imply a probabilistic distribution. However, what distribution, exactly, does this ratio follow? 

The ratio of `\(MSR\)` over `\(MSE\)` is an [F-distributed](link here) variable with (`\(1, n-2\)`) degrees of freedom. 

 `$$\frac{\frac{SSR}{\sigma^2}}{1} \div \frac{\frac{SSE}{\sigma^2}}{n-2} = \frac{MSR}{MSE} \sim F(1, n-2)$$`
 

<details>
<summary> Derivation here. </summary>
By Cochran's theorem, if `\(n\)` observations `\(Y_i\)` come from the same normal distribution with mean `\(\mu\)` and variance `\(\sigma^2\)`, and `\(SSTO = \sum (Y_i - \bar Y)^2\)` is partitioned into `\(k\)` sums of squares `\(SS_1... SS_k\)`, each with degrees of freedom `\(df_{1}...df_k\)`, then `\(\frac{SS_1}{\sigma^2} ... \frac{SS_k}{\sigma^2}\)` are independent `\(\chi^2\)` variables with `\(df_r\)` degrees of freedom.

Under the null hypothesis where `\(\beta_1 = 0\)`, each observation `\(Y_i\)` has `\(E(Y_i) = \beta_0 = \mu\)` (contrast this with `\(\beta_1 \ne 0\)`, where we expect the mean response to vary with the predictor). Owing to the assumptions of linear regression, variance is constant `\(\sigma^2\)`. `\(SSTO\)`, as we know, is partitioned into `\(SSR\)` and `\(SSE\)`, with 1 and `\(n-2\)` degrees of freedom, respectively.

Then, `\(\frac{SSR}{\sigma^2}, \frac{SSE}{\sigma^2}\)` are independent and follow `\(\chi^2\)` distributions with 1 and `\(n-2\)` degrees of freedom, respectively. If we then divide these two fractions by their degrees of freedom, we obtain: 

 `$$\frac{\frac{SSR}{\sigma^2}}{1}, \frac{\frac{SSE}{\sigma^2}}{n-2}$$`
 
 When we take the ratio of these two fractions, by the definition of an F-distributed random variable, we obtain an F-distributed random variable with `\((1, n-2)\)` degrees of freedom.
</details> 

<details>
<summary> We examine whether `\(\hat \beta_1=0\)` for the fitted line for the Toluca company. </summary>

``` r
library(api2lm)

lm1 <- lm(work_hours~lot_size, data=toluca)
anova(lm1)
```

```
## Analysis of Variance Table
## 
## Response: work_hours
##           Df Sum Sq Mean Sq F value    Pr(>F)    
## lot_size   1 252378  252378  105.88 4.449e-10 ***
## Residuals 23  54825    2384                      
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
Judging by the F-value of 105.88 in the ANOVA table (corresponding to `\(p < .05\)`), we reject the null hypothesis of `\(\beta_1 = 0\)`. The (large) amount of variance explained by `\(X\)` is unlikely under conditions where `\(\beta_1 = 0\)`.
</details>

### Relation to the t-test
If we revisit `\(F = \frac{MSR}{MSE}\)`, we can express it in terms of a `\(t\)` statistic. We make use of the following identities:
$$
SSR =\sum(\hat Y_i - \bar Y)^2 = \sum(\hat \beta_0 + \hat \beta_1 X_i - (\hat \beta_0 + \hat \beta_1 \bar X))^2 = \sum(\hat \beta_1 X_i - \hat \beta_1 \bar X)^2 = \hat \beta_1 ^2 \sum (X_i - \bar X)^2 = MSR \quad \text{(divide by 1 df)} \\
s^2({\hat \beta_1}) = \frac{MSE}{\sum(X_i - \bar X)^2} \quad \text{our estimate of } \sigma^2(\hat \beta_1)
$$

$$ \begin{aligned}
F &= \frac{MSR}{MSE} \sim F(1, n-2) \\
&= \frac{\hat \beta_1^2 \sum(X_i - \bar X)^2}{s^2(\hat \beta_1) \sum (X_i - \bar X)^2} = \left(\frac{\hat \beta_1}{s({\hat \beta_1})} \sim t(1)\right)^2  \\
\end{aligned}$$`

In other words, an `\(F\)` statistic (used for testing `\(\beta_1 = 0\)`) is equivalent to a squared `\(t\)` statistic. That means that given a `\(t\)` statistic, you can square it to get an F-statistic.  Given an `\(F\)` statistic, you take the square root to get a t-statistic.

However, what is the _probabilistic_ relation of `\(t\)` to `\(F\)`? We define it below. We've established that since `\(F = T^2\)`, `\(P(F > t_0) = P(T^2 > t_0)\)`. 
Then, 

`$$\begin{aligned}
P(F > t_0) &= P(T^2 > t_0) \\
P(F > t_0) &= P(T < -\sqrt t_0) + P(T > \sqrt t_0) \\
P(F > t_0) &= 2P(T > \sqrt t_0) \quad \text{ (owing to symmetry of T) }
\end{aligned}$$`

We conclude that the probabilistic relation is that the probability of `\(F\)` being greater than `\(t_0\)` is equivalent to the probability of `\(t\)` being less than `\(-\sqrt{t_0}\)` or greater than `\(\sqrt{t_0}\)`.

**In other words, a one-sided F-test using `\(F\)` is equivalent to a two-sided t-test using `\(T\)`.**

``` r
pf(105.88, df1=1, df2=nrow(toluca)-2, lower.tail=FALSE)
```

```
## [1] 4.44711e-10
```

``` r
pt(sqrt(105.88), df=nrow(toluca)-2, lower.tail=FALSE)*2
```

```
## [1] 4.44711e-10
```


