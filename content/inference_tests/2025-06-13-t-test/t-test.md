---
title: T-test (population mean)
author: ''
date: '2025-06-13'
slug: t-test
categories:
  - Inference/tests
tags: []
---
<script src="/rmarkdown-libs/kePrint/kePrint.js"></script>
<link href="/rmarkdown-libs/lightable/lightable.css" rel="stylesheet" />
<script src="/rmarkdown-libs/kePrint/kePrint.js"></script>
<link href="/rmarkdown-libs/lightable/lightable.css" rel="stylesheet" />


The t-test can be used to make inferences regarding a population mean when we have a sample from said population. 

The assumptions are that

1) The sample mean follows an approximately normal distribution. This requires either `\(n \ge 30\)` or the population to be normally distributed, either of which are ensures `\(\bar X \sim N\)` (approximately).
2) The numerator and denominator are independent, which can be assumed for a sample drawn from a normal population.

The statistic used is 

\[ T = \frac{\bar X - \mu}{\sqrt{ \frac{S^2}{n}}}\]

Where `\(\bar X\)` is the sample mean, `\(\mu\)` is the theorized population mean, `\(\sqrt{ \frac{S^2}{n}}\)` is our estimated standard error. 

This statistic follows a [t-distribution](/distrib/2025-05-27-t-distribution/t-dist/). The proof itself is actually quite involved (hint: the numerator and denominator we're looking at -- `\(\bar X - \mu\)`, `\(\sqrt{S^2 \over n}\)` -- aren't `\(\sim Z, \sim \chi^2\)`, respectively).

In the real world, where we don't _know_ whether a population is normally distributed, we can't be certain that this statistic follows a t-distribution. Yet we need not worry. Theoretically, if `\(n\)` is sufficiently large, the entire statistic becomes approximately standard normal (Slutsky's theorem). We can then verify this empirically: if we run simulations of t-tests when the population is non-normal and normal, the performances are relatively similar if the population's distribution is *symmetric* and *behaves well*.
 
# Power tables

We evaluate the ability of the t-test to reject the null hypothesis when it is false in a series of experiments where the population distribution is non-normal and the sample size varies. Each cell of the table represents the proportion of rejections out of 10,000 samples taken.

Let
\[ H_0: \mu = 5\]
\[ H_a: \mu \ge 5  \]

In actuality, `\(\mu = 7\)` for each of the below distributions.

We see that with the exception of the Cauchy and exponential distributions, the t-test's power on non-normal populations is comparable to its power on normal populations. The Cauchy is symmetrical, but it has no mean, so sample means will be volatile; some entering the rejection region and some not. The exponential distribution is extremely skewed compared to other distributions (even already skewed distributions like the chi-squared distribution). 


<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:right;"> 10 </th>
   <th style="text-align:right;"> 20 </th>
   <th style="text-align:right;"> 30 </th>
   <th style="text-align:right;"> 40 </th>
   <th style="text-align:right;"> 50 </th>
   <th style="text-align:right;"> 60 </th>
   <th style="text-align:right;"> 70 </th>
   <th style="text-align:right;"> 80 </th>
   <th style="text-align:right;"> 90 </th>
   <th style="text-align:right;"> 100 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Normal </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> t_shifted_7 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cauchy </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.58 </td>
   <td style="text-align:right;"> 0.52 </td>
   <td style="text-align:right;"> 0.54 </td>
   <td style="text-align:right;"> 0.59 </td>
   <td style="text-align:right;"> 0.44 </td>
   <td style="text-align:right;"> 0.50 </td>
   <td style="text-align:right;"> 0.60 </td>
   <td style="text-align:right;"> 0.57 </td>
   <td style="text-align:right;"> 0.49 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LogNormal </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Exponential </td>
   <td style="text-align:right;"> 0.07 </td>
   <td style="text-align:right;"> 0.27 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> 0.66 </td>
   <td style="text-align:right;"> 0.63 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 0.77 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:right;"> 0.88 </td>
   <td style="text-align:right;"> 0.95 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uniform </td>
   <td style="text-align:right;"> 0.42 </td>
   <td style="text-align:right;"> 0.65 </td>
   <td style="text-align:right;"> 0.83 </td>
   <td style="text-align:right;"> 0.95 </td>
   <td style="text-align:right;"> 0.98 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laplace </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ChiSq </td>
   <td style="text-align:right;"> 0.78 </td>
   <td style="text-align:right;"> 0.98 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Triangular </td>
   <td style="text-align:right;"> 0.66 </td>
   <td style="text-align:right;"> 0.90 </td>
   <td style="text-align:right;"> 0.98 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bimodal </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
</tbody>
</table>

# Type I error table

We evaluate the tendency of the t-test to reject the null hypothesis when it is true. 

Let
\[ H_0: \mu = 7\]
\[ H_a: \mu \ge 7  \]

and `\(\mu = 7\)` for each of the below distributions. 

We see that Type I error rates are relatively similar across distributions (with the exception of exponential). Generally, Type I errors are more robust to violation of assumptions. That's an asymmetry to memorize. 

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:right;"> 10 </th>
   <th style="text-align:right;"> 20 </th>
   <th style="text-align:right;"> 30 </th>
   <th style="text-align:right;"> 40 </th>
   <th style="text-align:right;"> 50 </th>
   <th style="text-align:right;"> 60 </th>
   <th style="text-align:right;"> 70 </th>
   <th style="text-align:right;"> 80 </th>
   <th style="text-align:right;"> 90 </th>
   <th style="text-align:right;"> 100 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Normal </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.08 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> t_shifted_7 </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.07 </td>
   <td style="text-align:right;"> 0.05 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.07 </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.08 </td>
   <td style="text-align:right;"> 0.05 </td>
   <td style="text-align:right;"> 0.09 </td>
   <td style="text-align:right;"> 0.04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cauchy </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LogNormal </td>
   <td style="text-align:right;"> 0.07 </td>
   <td style="text-align:right;"> 0.13 </td>
   <td style="text-align:right;"> 0.10 </td>
   <td style="text-align:right;"> 0.21 </td>
   <td style="text-align:right;"> 0.23 </td>
   <td style="text-align:right;"> 0.23 </td>
   <td style="text-align:right;"> 0.26 </td>
   <td style="text-align:right;"> 0.27 </td>
   <td style="text-align:right;"> 0.29 </td>
   <td style="text-align:right;"> 0.36 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Exponential </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.05 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uniform </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.07 </td>
   <td style="text-align:right;"> 0.08 </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.09 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laplace </td>
   <td style="text-align:right;"> 0.09 </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.08 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.08 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.06 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ChiSq </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.05 </td>
   <td style="text-align:right;"> 0.02 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.05 </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Triangular </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 0.07 </td>
   <td style="text-align:right;"> 0.08 </td>
   <td style="text-align:right;"> 0.06 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bimodal </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
</tbody>
</table>

