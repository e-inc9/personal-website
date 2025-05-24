---
title: "Finding Coefficients for Linear Regression"
output: html_document
date: '2025-05-19'
categories:
  - Proofs/derivations
---

## Finding coefficients for linear regression

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

\[
`\begin{align*}
 \sum 2(Y_i - \hat \beta_0  - \hat \beta_1X_i) \cdot -1 &= 0 \\ 
 \sum (Y_i - \hat \beta_0  - \hat \beta_1X_i) &= 0 \\  
 \sum (Y_i) - n \hat \beta_0  - \hat \beta_1 \sum (X_i) &= 0 \\  
n \bar Y - n \hat \beta_0  - \hat \beta_1 n \bar X &= 0 \\  
\bar Y -  \hat \beta_0  - \hat \beta_1  \bar X &= 0 \\ 
\end{align*}`
\]

Solving for `\(\hat \beta_0\)`, we get:

$$
\hat \beta_0 = \bar Y - \hat \beta_1 \bar X
$$

## Optimal `\(\hat \beta_1\)`

Using `\(\frac{dh}{d \hat \beta_1} = \sum 2(Y_i - \hat \beta_0 - \hat \beta_1X_i) \cdot  - X_i\)`:

\[
`\begin{aligned}
\sum 2(Y_i - \hat \beta_0 - \hat \beta_1X_i) \cdot - X_i &= 0 \\
\sum (X_iY_i - \hat \beta_0 X_i - \hat \beta_1X_i^2) &= 0 \\
\sum (X_iY_i) - \hat \beta_0 \cdot n \bar X - \hat \beta_1 \sum (X_i^2) &= 0 \\
\end{aligned}`
\]

At this stage, we plug in `\(\hat \beta_0\)`, which we found above, into the expression:

\[
`\begin{aligned}
\sum (X_iY_i) - (\bar Y - \hat \beta_1 \bar X) \cdot n \bar X - \hat \beta_1 \sum X_i^2 &= 0 \\
\sum (X_iY_i) - (n \bar X \bar Y - n \hat \beta_1 \bar X^2) - \hat \beta_1 \sum X_i^2 &= 0 \\
\sum (X_iY_i) - n \bar X \bar Y + n \hat \beta_1 \bar X^2 - \hat \beta_1 \sum X_i^2 &= 0 \\
\sum (X_iY_i) - n \bar X \bar Y + \hat \beta_1 \left[ n \bar X^2 - \sum X_i^2 \right] &= 0 \\
\end{aligned}`
\]

Solving for the above, we arrive at:

$$
\hat \beta_1 = \frac{- \sum (X_iY_i) - n \bar X \bar Y}{n \bar X^2 - \sum X_i^2 }
$$

We apply the `\(-1\)` in the numerator to the denominator, which gives us the expression for `\(\hat \beta_0\)` in its conventional form:

$$
\hat \beta_1 = \frac{\sum (X_iY_i) - n \bar X \bar Y}{ \sum X_i^2 - n \bar X^2 }
$$

## Abbreviated form of squared sums

We usually denote:

- `\(\sum (X_iY_i) - n \bar X \bar Y\)` by `\(S_{xy}\)`  
- `\(\sum X_i^2 - n \bar X ^2\)` by `\(S_{xx}\)`  
- `\(\sum Y_i^2 - n \bar Y^2\)` by `\(S_{yy}\)`

Then:

$$
\hat \beta_1 = \frac{S_{xy}}{S_{xx}}
$$
