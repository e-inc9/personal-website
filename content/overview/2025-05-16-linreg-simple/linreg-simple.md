---
title: "Simple linear regression"
output: html_document
date: '2025-05-16'
categories:
  - General
editor_options: 
  markdown: 
    wrap: 72
---



## Mathematical and statistical relations

Consider two variables `\(x,y\)` that you know to be related.

-   They are related *mathematically* if there exists a *deterministic*
    relationship between the two, such that `\(x\)` perfectly determines `\(y\)`
    and vice versa. Such relationships can be expressed with a function
    `\(Y=f(X)\)`. One example is the relationship between inches and
    centimeters `\(Y=2.54X\)`. A man who is `\(x=65\)` inches tall must be
    `\(y=165.1\)` centimeters tall.

-   They are related *statistically* if there exists a relationship
    between the two, but `\(X\)` does *not* perfectly determine `\(Y\)` and vice
    versa. You know the two are related in some way -- whether visually
    or intuitively -- but you cannot say you know everything about `\(Y\)`
    just by `\(X\)`. Take the below scatter plot relating `\(X\)`, the velocity
    at which a bullet is fired, and `\(Y\)`, the depth of penetration into
    body armor. Perhaps you didn't even have to look at the below graph
    to know that a relationship exists. Faster bullet `\(\implies\)` deeper
    penetration. Yet the presence of other factors at play -- angle,
    wind speed, freak properties of physics -- make you unable to
    perfectly determine penetration given velocity.

Does that mean we should just *give up*? Forget fitting a mathematical
equation to this relationship? Get scared in the face of unknowing,
uncertainty, the potential of being *wrong*? Hell no. Patterns exist,
trends lurk beneath; you can fit some rules, induce some structure, make
some sense of it all -- potential life lesson there...

<img src="/overview/2025-05-16-linreg-simple/linreg-simple_files/figure-html/unnamed-chunk-1-1.png" width="672" />

# Building up to the simple linear regression model

Our model is best understood as a *formal counterpart* for two phenomena
observed in statistical relations: 1) the tendency of `\(Y\)` to vary with `\(X\)` yet 2) the inexactitude of `\(Y\)` given `\(X\)`.

We can translate these observations to a probabilistic framework by saying that at a given level of `\(X\)`, `\(Y\)` will follow a probabilistic distribution. Furthermore, the mean of this distribution will differ with (moves with) `\(X\)`.

![](../../../../../../../../pics/3d_distrib_linreg.png)<!-- -->

In the case of simple linear regression, we'll make some assumptions about the nature of this distribution. For now, note that the mean of our distributions always lie along a straight line.

## Basic setup

Our regression model starts off similar to that of a mathematical
function `\(Y=f(X)\)`. However, we add an *error variable*, `\(\varepsilon_i\)`,
to account for both randomness and the existence of *other variables* that make the
relationship between `\(X\)` and `\(Y\)` imperfect.

This model assumes a relationship exists between `\(X\)` and `\(Y\)`, but
concedes that it is non-mathematical and that `\(Y\)` is influenced by
factors other than `\(X\)`. It is the formal counterpart to what we know
from the graph and our intuition.

$$ Y =f(X) + \varepsilon $$

## Adding assumption of linearity

At this stage, we must define `\(f(X)\)`, the nature of the relationship
between `\(X\)` and `\(Y\)`. It can't just be a black box function. Returning to
the graph above, we could say that penetration increases linearly with
velocity. The scatter does appear to mostly be a straight blob and not a
curvy one. It's simple and straightforward. `\(\beta_1\)` is the degree to
which penetration increases when velocity increases by 1 m/sec.
`\(\beta_0\)` is penetration when velocity is 0. For most purposes,
`\(\beta_0\)` isn't useful, but since we are modelling a linear
relationship, there must be a `\(y\)`-intercept. Note that we do not
dispense with `\(\varepsilon_i\)`; the assumption is still that there exist
randomness and other variables affecting `\(Y\)`, but `\(X\)`'s effect on `\(Y\)` is linear.
$$ Y_i = \beta_0 + \beta_1X_i + \varepsilon_i $$

We assume that `\(X_i\)` are known constants.

## On the error variable
`\(\varepsilon_i\)` incorporates randomness and other variables. By definition, it is a random variable. Since `\(X_i\)` are known constants, and `\(\beta_0, \beta_1\)` are constants also, this `\(\varepsilon_i\)` is what makes `\(Y_i\)` a random variable and how we account for the inexactitude of `\(Y\)` given `\(X\)`.

The following assumptions for `\(\varepsilon_i\)` are crucial:

-   `\(\varepsilon_i\)` follows a distribution centered at 0 with
    variance `\(\sigma^2\)`. 
-   Thus, `\(E(\varepsilon)\)` = 0.
-   `\(\varepsilon_i\)` is uncorrelated; one `\(\varepsilon_i\)` is not related to
    another.

## The resulting model
1.  `\(E(Y_i) = E(\beta_0+\beta_1X_i+\varepsilon) = \beta_0 + \beta_1X_i + 0\)`.
    The expected value of `\(Y_i\)` is solely dependent on `\(X_i\)`. Our fitted `\(\beta_0, \beta_1\)`, are used to predict the expected value of `\(Y_i\)` at `\(X_i\)`.

2.  `\(V(Y_i) = V(\beta_0+\beta_1X_i+\varepsilon_i) = V(\varepsilon_i) = \sigma^2\)`. We expect `\(Y_i\)` to vary about `\(E(Y_i)\)` by the variance of `\(\varepsilon_i\)` _at all levels of `\(X_i\)`_.

3. Since `\(\varepsilon_i\)` is uncorrelated with `\(\varepsilon_j\)`, `\(Y_i, Y_j\)` are uncorrelated as well.

Summarily, `\(Y_i\)` follows a distribution centered at `\(\beta_0 + \beta_1X_i\)` with variance of `\(\sigma^2\)`. This tendency to vary comes from error term `\(\varepsilon_i\)`. `\(\beta_1\)` gives the change in the mean of this distribution with `\(X_i\)`; it tells us how we'd _expect_ `\(Y_i\)` to change with `\(X_i\)`. Any departures from `\(E(Y_i) = \beta_0 + \beta_1X_i\)` are due to `\(\varepsilon_i\)`'s tendency to vary.   

## Estimating regression coefficients
If there exist sample data for which we think the above model applies, we then use
that data to best estimate coefficients `\(\hat \beta_0, \hat \beta_1\)`. Recall that these two parameters are used to compute the expected `\(Y_i\)` at `\(X_i\)`. Thus, `\(\hat Y_i\)` is also an estimate of the expected `\(Y_i\)` at `\(X_i\)`.

A general rule of thumb for fitting a linear regression line is that it should
run through our sample points such that the absolute distance between
the line and the points in our data is minimized. This is called the
*ordinary least squares (OLS)* method. We can express this distance as a
sum of squared differences:

$$  \sum_{i=1}^{n} (Y_i - \hat Y_i)^2 = \sum_{i=1}^n(Y_i - (\hat \beta_0 + \hat \beta_1 X_i))^2 $$
Considering that this is a convex function, taking the first derivative
of it with respect to `\(\hat \beta_0, \hat \beta_1\)`, equating both
results to 0, and solving the system of equations give us the
`\(\hat \beta_0, \hat \beta_1\)` where the sum of squared differences is
minimized:

$$ \hat \beta_0 = \bar Y - \hat \beta_1 \bar X$$
$$ \hat \beta_1 = \frac{\sum (X_iY_i) - n \bar X \bar Y}{ \sum X_i^2 - n \bar X^2} $$

[Full derivation here, for those
interested.](/proofs_deriv/2025/05/19/deriving-the-coefficients-for-linear-regression/)

Under the _Gauss-Markov theorem_, the OLS method is the _best_ way of estimating `\(\beta_0, \beta_1\)` because our estimates will be [a) unbiased and b) have minimum variance out of all unbiased linear estimators.](/overview/2025-09-13-point-estimation/point-estimation/). We also arrive at the same expressions for `\(\hat \beta_0, \hat \beta_1\)` when we use [maximum likelihood estimation](/overview/2025-09-15-maximum-likelihood-estimation/mle/).


Given `\(\hat \beta_0, \hat \beta_1\)`, we have 

$$ \hat{Y} = \hat \beta_0 + \hat \beta_1X $$

which gives us the expected or mean point estimate `\(\hat Y_i\)` for a given `\(X_i\)`. This point estimate is our fitted value. It is liable to differ from our observed value `\(Y_i\)` by `\(e_i = Y_i - \hat Y_i\)`, or the `\(i^{th}\)` residual.

## Residuals
Note that `\(e_i\)`, the residual corresponding to a given `\(Y_i\)`, is different from `\(\varepsilon_i\)`, the random variable. 

We cannot know `\(\varepsilon_i\)` since it measures deviation from a "true" regression line which itself is unknown. We just know it's a random variable.

On the other hand, we know `\(e_i\)` as a measure of deviation from a _fitted_ regression line. It's downstream of our sample.

Some properties of `\(e_i\)` to note. 

1. `\(\sum e_i^2\)` is at its minimum. This is what the least squares method does.
2. `\(\sum e_i = 0\)`. The sum of residuals is 0.
3. `\(\sum Y_i\)` = `\(\sum \hat Y_i\)`. 
4. `\(\sum \hat Y_i e_i\)` = `\(\sum X_i e_i = 0\)`. The sum of residuals weighted by fitted values or levels of X is 0.
5. The fitted regression line goes through `\((\bar X, \bar Y)\)`.

All of these are downstream from the `\(\hat \beta_0, \hat \beta_1\)` we found above.

## Returning to the error variable
Earlier, we stated that at each level of `\(X\)`, `\(Y\)` follows a distribution centered at `\(E(Y_i)\)` with variance `\(\sigma^2\)`. Given that this variance corresponds to the ways in which observed values may deviate about a "true" regression line, it's important to get an idea of _its size_.

Given a sample, we estimate the variance of `\(Y_i\)` the same way we would estimate the variance of a single population but with one difference: we divide by `\(n-2\)`, since two parameters `\(\beta_0, \beta_1\)` are estimated.

Therefore, our variance estimator is:

$$
MSE = \text{Error/Residual mean square}= s^2 = \frac{\sum(Y_i - \hat Y_i)^2}{n-2} = \frac{\sum e_i^2}{n-2}
$$
Along with that, `\(s\)` is our estimator of standard deviation. 

Note that the corollary of `\(\bar X\)` when estimating the variance from a single population is `\(\hat Y_i\)`, which is our "expected value"/"estimated mean" of `\(Y_i\)`.

This estimator is unbiased; `\(E(s^2) = \sigma^2\)`.

## Conclusion 
Insofar, we've learned about a statistical model that acknowledges a) the tendency of `\(Y\)` to vary with `\(X\)` and b) the tendency for `\(Y_i\)` to vary at `\(X_i\)`. 

Given sample data with observations `\((X,Y)\)`, we can fit a simple linear regression model with parameters defined by the least squares method to estimate the expected value of `\(Y\)` at `\(X\)`, and estimate the variance of `\(Y\)` at `\(X\)`. 

However, this is just the tip of the iceberg, because we've yet to add one of the standard assumptions of real-world applications: normality of the error term `\(\varepsilon_i\)`. [Read here.](/overview/simple-linear-regression-with-normality/)



