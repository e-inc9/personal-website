---
title: Simple linear regression (ANOVA cut)
author: ''
date: '2025-09-23'
slug: linreg-anova
categories:
  - General
tags: []
---

Note: I encourage you to first read [simple linear regression](/overview/2025-05-16-linreg-simple/linreg-simple/) and then [simple linear regression (with normality)](/overview/simple-linear-regression-with-normality/). 

Here we present a different _cut_ to linear regression. We don't build on or extend what is discussed above. Rather, we build up from scratch a different way of interpreting linear regression.  Understanding this "alternative angle" will prove crucial in future statistical problems beyond linear regression.  

## Introducing our `\(Y_i\)`'s.
Consider a response variable `\(Y\)`, and observations `\(Y_i\)` within a sample. Inevitably, we are able to calculate `\(\bar Y\)`. It will "run through" the middle of the sample and our `\(Y_i\)`'s will be scattered about it. We measure this variation of our observations about the sample mean using the _total sum of squares_ (SSTO).
$$
SSTO = \sum(Y_i - \bar Y)^2
$$
Insofar, SSTO measures the uncertainty of our response, *absent of any predictors*. More varied observations implies a higher SSTO, and thus more uncertainty.

## Adding on a predictor X
When we conduct a simple linear regression with a predictor `\(X\)`, and thus fit line `\(\hat Y = \beta_0 + \beta_1X\)` to the data, there still exists scatter. We use this scatter about the fitted line as a measure of the uncertainty of our response *when predictor X is taken into account*. This is measured by the error sum of squares (SSE). 
$$
SSE = \sum(Y_i - \hat Y)^2
$$
If the line is any good, there will be less variation of our observations `\(Y_i\)` about it;  `\(SSE < SSTO\)`. 



## What's left? (SSR)
We have (a) SSTO and (b) SSE depicted below. Notice how we haven't accounted for one remaining difference, pictured in (c). This is the distance between the fitted line `\(\hat Y\)` and the sample mean `\(\bar Y\)`. 
<img src="../../../../../../../../pics/SSE_SSTO_SSR.png" width="100%" />

We refer to this difference as regression sum of squares (SSR):
$$
SSR =\sum(\hat Y_i - \bar Y)^2
$$
As we can literally see, `\(SSR = SSTO - SSE\)`. It is the difference in variance about the mean and variance about the fitted line. In other words, it is the variance explained by our line `\(\hat Y\)` (and by extension, the variance explained by our predictor `\(X\)`).

_"But what do you mean explained"_? If we observe a reduction in variance when we calculate about the fitted line instead of about the sample mean, then the variance of our observations is _better accounted for_ by the relationship between `\(X\)` and `\(Y\)` as described  by the fitted line. 

By rearranging, we get `\(SSTO = SSR+ SSE\)`. The total variance about a sample mean `\(SSTO\)` can be partitioned into 
a) `\(SSR\)` variance explained by a fitted (regression) line and 
b) `\(SSE\)` the remaining variance (error) in spite of the line.

As we'll later see, these pieces can be used in ratios. Thus, there's something to be said about the _magnitude_ of these values. We illustrate with an example.

### Example 
<details>
<summary> We revisit the Toluca Company sample, and compute `\(SSTO, SSR\)`, and `\(SSE\)`. </summary>

```
## [1] "SSTO:307203.04"
```

```
## [1] "SSR:252377.58"
```

```
## [1] "SSE:54825.46"
```

```
## 
## Call:
## lm(formula = work_hours ~ lot_size, data = toluca)
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
We see that the variance about the fitted regression line (`\(SSE \approx 54k\)`) is a small fraction of the total variance about the sample mean (`\(SSTO \approx 307k\)`). We "back in" to the variance explained by the regression line and find that it is very large (`\(SSR \approx 252k\)`). This implies that predictor `\(X\)` *explains* a lot of the variance about the sample mean.
</details>

## A word on degrees of freedom
Just as total variance about the sample mean can be partitioned, so can the degrees of freedom of `\(SSTO\)`:

`$$n-1 = n-2 + 1$$`.

`\(SSTO\)` has `\(n-1\)` degrees of freedom, being that `\(\sum (Y_i - \bar Y) = 0\)`. (If you know all but one, you can impute the remaining value). 

`\(SSE\)` has `\(n-2\)` degrees of freedom. Since `\(SSE\)` relies on `\(n\)` observations `\(Y_i\)` (free to vary however they'd like), but estimates parameters `\(\hat \beta_0, \hat \beta_1\)`, we subtract by 2. Each parameter estimated imposes an additional constraint on `\(Y_i\)`. More on this later.

`\(SSR\)` has `\(1\)` degree of freedom. We start with 2 degrees of freedom for the estimated `\(\hat \beta_0, \hat \beta_1\)`. However, recall that `\(SSR\)` involves a **difference relative to `\(\bar Y\)` (the baseline)**. Given that `\(\bar Y\)` is already known, this imposes a constraint on `\(\hat \beta_0\)`, the intercept estimate (`\(\hat \beta_0 = \bar Y - \hat \beta_1 \bar X\)`), and so we subtract by 1.  This is how it "_maths_", by the way. You can see that since `\(X\)` is fixed across samples, `\(\hat \beta_1\)` is the only thing free to vary.

`$$\begin{aligned}
SSR =\sum(\hat Y_i - \bar Y)^2 = \sum(\hat \beta_0 + \hat \beta_1 X_i - (\hat \beta_0 + \hat \beta_1 \bar X))^2 = \sum(\hat \beta_1 X_i - \hat \beta_1 \bar X)^2 = \hat \beta_1 ^2 \sum (X_i - \bar X)^2
\end{aligned}$$`

These are important. 

## On MSE, MSR
We average our measures of variance about the mean and the regression line by the degrees of freedom they have, such that sample size is taken into account. Since `\(SSE\)` has `\(n-2\)` degrees of freedom, its `\(MSE \ne SSE\)`. `\(SSR\)` has 1 degree of freedom, so `\(MSR = SSR\)`.

``` r
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
`\(MSE\)` and `\(MSR\)` follow distributions centered at:
$$
E({MSE}) = \sigma^2\\
E({MSR}) = \sigma^2 +  \beta_1^2 \sum (X_i - \bar X)^2
$$
The first equation should be familiar. As we established, `\(MSE\)` is an unbiased estimator for `\(\sigma^2\)`, the variance of `\(\varepsilon\)`.

<details>
<summary> Derivation here. </summary>
$$ \begin{aligned}
E(MSR) &= E\left[ \sum (\hat Y_i - \bar Y)^2 \right]\\
&=E\left[ \hat \beta_1 ^2 \sum (X - \bar X)^2 \right]  \quad (\text{Let } \sum (X - \bar X)^2 = a)\\
&= E(\hat \beta_1 ^2) \cdot a  \quad (\text{Invoke } \sigma^2(\hat \beta_1) = E(\hat \beta_1^2) - [E(\hat \beta_1)]^2)\\
&= \left[ \sigma^2(\hat \beta_1) + [E(\hat \beta_1)^2] \right] \cdot a\\
&= \left[ \frac{\sigma^2}{\sum(X_i - \bar X)^2} + \beta_1^2 \right] \sum(X_i - \bar X)^2 \\
&= \sigma^2 + \beta_1^2 \sum(X_i - \bar X)^2 
\end{aligned} $$
</details>

Notice how the center of the distribution of `\(MSR\)` is greater than (to the right of) the distribution of `\(MSE\)` when `\(\beta_1\)` is non-zero. If `\(\beta_1 = 0\)`, `\(MSE=MSR\)`. This implies that we can test whether `\(\beta_1  = 0\)` by comparing `\(MSE\)` and `\(MSR\)`, and assessing the likelihood of what we observe under a null hypothesis on `\(\beta_1\)`. We proceed to the [F-test](/inference_tests/2025-09-24-f-test-anova-linear-regressions/f-test/).

## Measures of linear association
The three-letter terms we defined in this section  prove useful for understanding the coefficient of determination and correlation, which are two descriptive measures of linear association. They can be a good way of assessing a sample of `\(X,Y\)` observations before a line is even fit. They come with caveats.

### Coefficient of determination `\(R^2\)`
Its units are the *percentage/proportion* of variance about the mean explained by a linear regression on a sample of `\((Y_1, Y_2)\)` observations.

$$
R^2 = \frac{SSR}{SSTO} = 1 - \frac{SSE}{SSTO}
$$
Some caveats: 
1) Correlation measures a linear relationship. A lack of correlation does not indicate a lack of a relationship, it just suggests that the relationship is not linear. The relationship could very well be exponential, logarithmic, etc.
2) A high correlation using a sample does not indicate that a linear model fitted is necessarily good for prediction. A sample could have very high variance about the mean -- a large _proportion_ of which is explained by a fitted line -- but that remaining variance could still make prediction difficult.
3) Remember that this _sample dependent_. Inference on `\(\rho^2\)` is possible -- more on that later.

Fun facts:
`\(R^2\)` _is_ affected by the spacing of `\(X_i\)`. An outlying `\(X\)` value will tend to have an outlying `\(Y\)` value (in either direction). Technically, it shouldn't affect `\(SSE\)`, which is simply the difference between `\(Y_i\)` and `\(\hat Y\)`. However, since this `\(Y\)` value will be a marked distance from `\(\bar Y\)`, which will increase `\(SSTO\)`. As `\(\frac{SSE}{SSTO}\)` becomes smaller, `\(R^2\)` will 

### Coefficient of correlation `\(r\)`
This applies when both `\(Y\)` _and_ `\(X\)` are random variables (whereas the terms used to calculate the previous, being all downstream of the simple linear regression assumptions, treats `\(X\)` as fixed).
We take the square root of `\(R^2\)` and add `\(\pm\)`, depending on directionality. This brings us to [normal correlation models](/overview/2025-09-26-normal-correlation-models/correlation./).




