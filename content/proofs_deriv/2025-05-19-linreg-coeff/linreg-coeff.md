---
title: "Deriving the coefficients for linear regression (OLS)"
output: html_document
date: '2025-05-19'
categories:
  - Proofs/derivations
header-includes:
  - \usepackage{amsmath}
---

We hereby describe the ordinary least squares method. 
## Finding coefficients for linear regression (OLS)

Recall that given a sample with observations `\(Y_1, Y_2, \dotsc Y_n\)`, the sum of squared differences is given by:

$$  
\sum_{i=1}^{n} (Y_i - \hat Y_i)^2 = \sum_{i=1}^n(Y_i - (\hat \beta_0 + \hat \beta_1 X_i))^2  
$$

We denote this sum by `\(h(\hat \beta_0, \hat \beta_1)\)`.

We use the first derivative to find the coefficients that minimize this sum. Since this function is convex, we know the first derivative is a minimum.

Note that the derivative of a summation over `\(f(x)\)` is the sum of `\(f'(x)\)`:

$$
\frac{d}{dx} \left( \sum_{i=1}^n f_i(x) \right) = \sum_{i=1}^n \frac{d}{dx} f_i(x)
$$

## Optimal `\(\hat \beta_0\)`

Using `\(\frac{dh}{d \hat \beta_0} = \sum 2(Y_i - \hat \beta_0  - \hat \beta_1X_i) \cdot -1\)`:

$$ \begin{aligned}
 \sum 2(Y_i - \hat \beta_0  - \hat \beta_1X_i) \cdot -1 &= 0 \\ 
 \sum (Y_i - \hat \beta_0  - \hat \beta_1X_i) &= 0 \\  
 \sum (Y_i) - n \hat \beta_0  - \hat \beta_1 \sum (X_i) &= 0 \\  
n \bar Y - n \hat \beta_0  - \hat \beta_1 n \bar X &= 0 \\  
\bar Y -  \hat \beta_0  - \hat \beta_1  \bar X &= 0 \\ 
\end{aligned} $$

Solving for `\(\hat \beta_0\)`, we get:

$$
\hat \beta_0 = \bar Y - \hat \beta_1 \bar X
$$

[The distribution of this estimator](/proofs_deriv/2025/05/23/distribution-of-hat-beta_0/).

## Optimal `\(\hat \beta_1\)`

Using `\(\frac{dh}{d \hat \beta_1} = \sum 2(Y_i - \hat \beta_0 - \hat \beta_1X_i) \cdot  - X_i\)`:

`$$\begin{aligned}
\sum 2(Y_i - \hat \beta_0 - \hat \beta_1X_i) \cdot - X_i &= 0 \\
\sum (X_iY_i - \hat \beta_0 X_i - \hat \beta_1X_i^2) &= 0 \\
\sum (X_iY_i) - \hat \beta_0 \cdot n \bar X - \hat \beta_1 \sum (X_i^2) &= 0 \\
\end{aligned}$$`

At this stage, we plug in `\(\hat \beta_0\)`, which we found above, into the expression:

$$ \begin{aligned}
\sum (X_iY_i) - (\bar Y - \hat \beta_1 \bar X) \cdot n \bar X - \hat \beta_1 \sum X_i^2 &= 0 \\
\sum (X_iY_i) - (n \bar X \bar Y - n \hat \beta_1 \bar X^2) - \hat \beta_1 \sum X_i^2 &= 0 \\
\sum (X_iY_i) - n \bar X \bar Y + n \hat \beta_1 \bar X^2 - \hat \beta_1 \sum X_i^2 &= 0 \\
\sum (X_iY_i) - n \bar X \bar Y + \hat \beta_1 \left[ n \bar X^2 - \sum X_i^2 \right] &= 0 \\
\end{aligned}$$`

Solving for the above, we arrive at:

$$
\hat \beta_1 = \frac{- \sum (X_iY_i) - n \bar X \bar Y}{n \bar X^2 - \sum X_i^2 }
$$

We apply the `\(-1\)` in the numerator to the denominator, which gives us the expression for `\(\hat \beta_0\)` in its conventional form:

$$
\hat \beta_1 = \frac{\sum (X_iY_i) - n \bar X \bar Y}{ \sum X_i^2 - n \bar X^2 }
$$
[The distribution of this estimator](/proofs_deriv/2025/05/19/distribution-of-hat-beta_1).
## Abbreviated form of squared sums

We usually denote:

- `\(\sum (X_iY_i) - n \bar X \bar Y\)` by `\(S_{xy}\)`  
- `\(\sum X_i^2 - n \bar X ^2\)` by `\(S_{xx}\)`  
- `\(\sum Y_i^2 - n \bar Y^2\)` by `\(S_{yy}\)`

Then:

$$
\hat \beta_1 = \frac{S_{xy}}{S_{xx}}
$$

## Important note

Note that this method of taking the derivative `\(\sum_{i=1}^{n} (Y_i - \hat Y_i)^2\)` does _NOT_ just apply to when `\(\hat Y_i = \hat \beta_0 + \hat \beta_1 X_i\)`. It can  be used for any model that is linear in the parameters, such as: 

\[ \begin{aligned}
&Y = \beta_0 + \beta_1 X \\
&Y = \beta_0 + \beta_1 X + \beta_2 X^2 \\
&Y = \beta_0 + \beta_1 \ln(X) \\
&Y = \beta_0 + \beta_1 e^{X} \\[10pt]
\end{aligned} \]

As an example, we could use the OLS method on `\(\hat Y_i = \hat \beta_1 X_i\)`, when we wish to fit a line through the origin. It will still give us the single coefficient `\(\hat \beta_1\)` that minimizes the distance between `\(Y_i\)`  and `\(\hat Y_i = \hat \beta_1 X_i\)`. 

However, in this instance, there are consequences: `\(\sum e_i \ne 0\)`, which has other effects such as `\(SSTO \ne SSE + SSR\)`, which means that we cannot interpret `\(R^2\)`, for example, as a proportion of variance explained.
