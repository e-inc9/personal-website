---
title: "Intro to matrix math for multivariate stats"
date: '2025-05-16'
categories:
  - General
output: html_document
---




## Why is matrix math important to multivariate data?

Consider that for observations within a sample, we can record multiple variables on a single observation. For example, for 10  students sampled from a class, we can record their height, weight, and average GPA. We can represent our findings with a matrix, where a row is an observation, and each column is a variable.

```
##       Height_cm Weight_kg  GPA
##  [1,]       164        75 2.29
##  [2,]       168        68 2.83
##  [3,]       186        68 2.83
##  [4,]       171        66 2.74
##  [5,]       171        61 2.30
##  [6,]       187        79 2.28
##  [7,]       175        69 2.47
##  [8,]       157        49 2.93
##  [9,]       163        71 2.53
## [10,]       166        61 3.72
```

In matrix-math speak, we can denote our matrix by `\(\mathbf{Y}\)`. We can express `\(\mathbf{Y}\)` as 40 `\(y_{ij}\)`'s, where `\(y_{ij}\)` is the `\(i\)`th observation's `\(j\)`th variable.
\[
\mathbf{Y} = 
\begin{bmatrix}
y_{11} & y_{12} & y_{13} \\
y_{21} & y_{22} & y_{23}  \\
\vdots & \vdots & \vdots \\
y_{i1} & y_{i2} & y_{i3} \\
\vdots & \vdots & \vdots \\
y_{10,1} & y_{10,2} & y_{10,3} 
\end{bmatrix}
\]

We can also express `\(\mathbf{Y}\)` as a collection of 10  `\(\mathbf{y_i'}\)` observation row vectors...
\[
\mathbf{Y} = 
\begin{bmatrix}
\mathbf{y_1}'\\
\mathbf{y_2}'\\
\vdots \\
\mathbf{y_{10}}'\\
\end{bmatrix}
\]

...where each observation row vector `\(\mathbf{y_i}\)` is then made up of variable values on that observation only.

\[
\mathbf{y_1'} = 
\begin{bmatrix}
y_{11} &
y_{12} &
y_{13} &
y_{14}
\end{bmatrix}
\]

Lastly, we can express `\(\mathbf{Y}\)` as a collection r of 4 `\(\mathbf{y}_{(j)}\)` variable column vectors...
\[
\mathbf{Y} = 
\begin{bmatrix}
\mathbf{y}_{(1)} & \mathbf{y}_{(2)} & \mathbf{y}_{(3)} & \mathbf{y}_{(4)}
\end{bmatrix}
\]
... where each variable column vector `\(\mathbf{y}_{(j)}\)` contains the measurements for observations `\(1\)` through `\(n\)`.

\[
\mathbf{y_{(1)}} = 
\begin{bmatrix}
y_{11} \\
y_{21} \\
y_{31} \\
\vdots \\
y_{10,1}
\end{bmatrix}
\]

Note that a bolded uppercase letter represents a matrix, a bolded lowercase letter represents a column vector, and a bolded lowercase letter + tick mark represents a row vector. A non-bolded lowercase letter represents a single value, or a scalar. `\(\mathbf{y}_i\)` denotes the ith observation vector, while `\(\mathbf{y}_{(j)}\)` denotes the jth variable vector. 

Representing multivariate data from a sample with matrix notation allows us to use matrix operations to express  various statistics such as the sample mean and sample variance.

<hr>

## Calculating the sample mean
Recall that in the univariate case (where we only observe one variable on each observation), we calculate the sample mean by averaging: `\(\sum y_i\)`. 

In the multivariate case, we perform the same averaging operation for each variable to calculate the sample mean row vector `\(\mathbf{\bar y'}\)`. Let there be `\(p\)` variables and `\(n\)` observations.

\[
\mathbf{\bar y'} = 
\begin{bmatrix}
\bar{y}_1 & \bar{y}_2 & \cdots & \bar{y}_p
\end{bmatrix}
\]
We can express this mean vector as the scaled multiplication of a `\(1 \times n\)`  identity row vector `\(\mathbf{j'}\)` and sample matrix `\(\mathbf{Y}\)`: 

\[
\bar{\mathbf{y}} = \frac{1}{n} \mathbf{j'} \mathbf{Y} = \frac{1}{n}
\begin{bmatrix}
1 & 1 & \dotsc & 1
\end{bmatrix}
\cdot
\begin{bmatrix}
\mathbf{y}^{(1)} & \mathbf{y}^{(2)} & \dotsc & \mathbf{y}^{(p)}
\end{bmatrix} =
\begin{bmatrix}
\frac{1}{n} \sum_{i=1}^{n} y_{i1} &
\frac{1}{n} \sum_{i=1}^{n} y_{i2} &
\cdots &
\frac{1}{n} \sum_{i=1}^{n} y_{ip}
\end{bmatrix}
\]

### Example - Sample mean
Consider the following sample matrix `\(\mathbf{Y}\)`:

```
##       y1   y2   y3
##  [1,] 35  3.5 2.80
##  [2,] 35  4.9 2.70
##  [3,] 40 30.0 4.38
##  [4,] 10  2.8 3.21
##  [5,]  6  2.7 2.73
##  [6,] 20  2.8 2.81
##  [7,] 35  4.6 2.88
##  [8,] 35 10.9 2.90
##  [9,] 35  8.0 3.28
## [10,] 30  1.6 3.20
```
We calculate the mean using the above math and check it with R's sample means function.

``` r
j_row <- matrix(1, nrow=1, ncol= nrow(Y))

ybar_vec <- (1/nrow(Y)) %*% j_row %*% Y; ybar_vec
```

```
##        y1   y2    y3
## [1,] 28.1 7.18 3.089
```

``` r
colMeans(Y)
```

```
##     y1     y2     y3 
## 28.100  7.180  3.089
```

<hr>

## Calculating the sample covariance matrix
Univariate variance, the scalar `\(\sigma^2\)`, is analogous to multivariate covariance matrix `\(\mathbf{\Sigma}\)`. Whereas univariate `\(\sigma^2\)` is the sum of squared distance `\(\sum (x_i - \bar{x})^2\)` from a sample mean for a single variable, in the multivariate case we must compute the sum of products of distance between all possible combinations of two variables, including a given variable with itself: `\(\sum (x_i - \bar{x})(y_i - \bar{y})\)`. This results in the `\(p \times p\)`  matrix `\(\mathbf{\Sigma}\)`,
\[
\mathbf{\mathbf{\Sigma}} = (s_{jk}) =
\begin{pmatrix}
s_{11} & s_{12} & \cdots & s_{1p} \\
s_{21} & s_{22} & \cdots & s_{2p} \\
\vdots & \vdots & \ddots & \vdots \\
s_{p1} & s_{p2} & \cdots & s_{pp}
\end{pmatrix}
\]

Where `\(s_{ab}\)` is the covariance between the ath and bth variable. Naturally, this results in computing the variance for all variables 1 through `\(p\)` along the diagonal of `\(\mathbf{\Sigma}\)`.

From `\(s_{ab}\)` expressed in vector terms, we can work up to a matrix understanding of `\(\mathbf{\Sigma}\)`. We first emphasize that the products of distance are summed over observations. Thus, we can express `\(\mathbf{\Sigma}\)` as the scaled sum of multiple matrices `\(\mathbf{\Sigma_1}, \mathbf{\Sigma_2},... \mathbf{\Sigma_n}\)`, where

\[ 
\mathbf{\Sigma_i} =  (\mathbf{y_i} - \mathbf{\bar{y}}) (\mathbf{y_i} - \mathbf{\bar{y}})' =
\begin{pmatrix}
y_{i1} - \bar{y}_1 \\
y_{i2} - \bar{y}_2 \\
\vdots \\
y_{ip} - \bar{y}_p \\
\end{pmatrix}
\begin{pmatrix}
y_{i1} - \bar{y}_1 & y_{i2} - \bar{y}_2 & \cdots & y_{ip} - \bar{y}_p
\end{pmatrix}
\]

When you scale the sum of all `\(\mathbf{\Sigma_i}'s\)`  by `\(\frac{1}{n-1}\)`, you get the sample covariance matrix `\(\mathbf{\Sigma}\)`. This allows us to calculate the covariance using the matrix alone.
\[ \mathbf{\Sigma}= 
\frac{1}{n-1}\sum_{i=1}^{n} (\mathbf{y_i} - \mathbf{\bar{y}}) (\mathbf{y_i} - \mathbf{\bar{y}})' = 
\frac{1}{n-1}\left( \sum_{i=1}^n  \mathbf{y_i y_i'} \right) - n\mathbf{\bar y \bar y'} 
\]

Using matrix math, the above can be expressed with the original matrix `\(\mathbf{Y}\)`:
\[\mathbf{\Sigma} = \mathbf{Y'Y}- \mathbf{Y'} \frac{\mathbf{j}}{1} \cdot \frac{\mathbf{j'}}{n} \mathbf{Y} = \mathbf{Y'}(\mathbf{I} - \frac{\mathbf{J}}{n})\mathbf{Y} \]

### Example - Sample covariance matrix 
Take the matrix above and compute it manually. Check with R. Answers should match:

```
##            y1        y2        y3
## y1 140.544444 49.680000 1.9412222
## y2  49.680000 72.248444 3.6760889
## y3   1.941222  3.676089 0.2501211
```

Using the vector method:

``` r
Y <- data.frame(Y)
ybar_vec <- colMeans(Y)
n <- nrow(Y)

Sigma <- matrix(0, nrow=ncol(Y), ncol=ncol(Y))
for (i in 1:n){
  dist <- as.matrix(Y[i,] - ybar_vec)
  Sigma_n <- t(dist) %*% dist
  Sigma <- Sigma + Sigma_n
}

print(Sigma/(n-1))
```

```
##            y1        y2        y3
## y1 140.544444 49.680000 1.9412222
## y2  49.680000 72.248444 3.6760889
## y3   1.941222  3.676089 0.2501211
```

Using the matrix method:

``` r
Y<- as.matrix(Y)
ybar_vec <- as.matrix(ybar_vec)
Sigma <-(1/(n-1)) * ( t(Y)%*%Y - n * ybar_vec %*% t(ybar_vec)); Sigma
```

```
##            y1        y2        y3
## y1 140.544444 49.680000 1.9412222
## y2  49.680000 72.248444 3.6760889
## y3   1.941222  3.676089 0.2501211
```

Using more intuitive method (courtesy of ChatGPT) where the distance of each variable measurement from the respective sample mean is subtracted out:

``` r
Y <- as.matrix(Y)
Y_centered <- sweep(Y,2,colMeans(Y)) #centers 
Sigma <- (1/(n-1)) * ( t(Y_centered) %*% Y_centered ); Sigma
```

```
##            y1        y2        y3
## y1 140.544444 49.680000 1.9412222
## y2  49.680000 72.248444 3.6760889
## y3   1.941222  3.676089 0.2501211
```

