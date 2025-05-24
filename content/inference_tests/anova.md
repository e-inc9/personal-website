---
title: "ANOVA (in progress)"
date: "2025-04-26"
output: html_document
categories:
  - Inference/tests
---



# Intro
ANOVA stands for analysis of variance. The goal of ANOVA is, for a variable measured on more than one group of data, to **use the variance of our measurements** to determine whether the population mean of those groups is different between at least one pair of groups.

\[ H_0: \mu_1 = \mu_2 = ... =\mu_n \]
\[ H_a: \mu_i \ne \mu_j \text{ for } i \ne j \]

You might wonder why we are analyzing _variance_ to get at a truth involving means -- I certainly did. The best way to intuit _why_ is through a simple exercise. 

Examine the points in graph (a), where each type of point corresponds to a sample from Group 1, Group 2, or Group 3. Judging by the sample mean within each cluster (which is somewhere at the middle), would you say that the population means of Group 1, Group 2, and Group 3 are different? Most would say **yes**.

Now examine the points in graph (b). Would you come to the same conclusion as you did before? Most would say **no**. But what if I told you that in both graphs, the sample mean is in the exact same place on both graphs? **You would still say no!** 


![](../pics/ANOVA_example.png)<!-- -->

At this point, one should realize that on an intuitive level, their read on the equality of the population means was based not on the sample means, but on the _clustering_, or spread, or **variance**,  of the data they observed:

 - In graph (a), the sample data for each group were clustered in a way that made a difference emerge between groups. One group's sample mean was likely to fall within each cluster, and thus be farther from another group's sample mean than the clusters were to each other.

 - In graph (b), the sample data for each group were interspersed with each other, making it difficult to tell where the sample means even fell, let alone whether they were different from each other! However, even if we did know where the sample means fell, the spread of the points in each sample ought to make us second guess whether the sample means we saw were even _close_ to the real population mean. And if we second guess the sample mean, then by extension we second guess any observed separation or lack thereof.
 
Unconsciously or subconsciously, we relied on variance in our assessment of the above graphs. Statistics draws from this intuition in creating a formalized ratio of variance called the `\(F\)`-statistic. This `\(F\)`-statistic falls along a known probability distribution, which then allows us to evaluate the equality of population means in probabilistic terms. 
 
We build up to the `\(F\)`-statistic by first starting between-group variance and within-group variance.

# Between and within-group variance.
The F-statistic is a ratio of mean between-group variance over within-group variance. Before actually discussing this statistic, we ought to first familiarize ourselves with the two types of variance. 

## Between group variance
_also known as sum of squares treatment,SSTr(eatment),SSB(etween)..._

Between group variance is a measure of how spread out our sample data is between groups. Refer once more to graph (a). The "clouds" of data for the groups are visually separate. We'd say that compared to graph (b), (a) displays a high between-group variance. For `\(k\)` groups and `\(n_i\)` observations within a group, between-group variance is given by:
\[SSB =\sum_{i=1}^{k} n_i \cdot (\bar X_i - \bar X_{..})^2 \]

`\(\bar{X_i}\)` is the sample mean within each group. `\(\bar{X_{..}}\)` is the grand mean -- meaning, the mean of all data points in the sample, across all groups. The intuition is that the more separation there is between the clusters of group sample data, the farther the sample mean of those clusters are from the grand mean  and thus the greater this sum of squared distances "between" groups.

A **key framing** of between group variance is that it is the variance _explained_ by hypothesized differences in the groups. It is the variance for which it would be plausible to say "hey, this might be because of differences in group means".  

## Within group variance
_also known as the sum of squares error, SSE(rror), SSW(ithin)..._

Within group variance is a measure of how spread out or sample data is within each group. Again, in graph (a), within group variance is very small. This contributes to the separation observed when looking at group sample data. In graph (b), this within group variance is very large, and so no real separation exists.
For `\(k\)` groups and `\(n_i\)` observations within a group, within-group variance is given by:
\[SSE = \sum_{i=1}^k \sum_{j=1}^{n_i} (X_{ij} - \bar{X_i})^2    \]

The best way to understood within-group variance, in my opinion, is as a required complement to between-group variance. Between-group variance may tell us that group sample means are far from the grand mean, but it does not tell us the scatter of the observations that led to the observed group variance. Knowing this scatter is paramount in determining whether the between group variance we observe is meaningful -- in other words, whether it's truly due to differences in the groups.

In the below visual example, between group variance is the same in both graphs. However, the scatter within each group in (c) lends more credence to group means being different compared to the scatter in (d). The group means are different in both graphs, but you can't deny that the scatter in (c) is more convicing of such a difference.

<img src="/inference_tests/anova_files/figure-html/unnamed-chunk-2-1.png" width="672" /><img src="/inference_tests/anova_files/figure-html/unnamed-chunk-2-2.png" width="672" />


A **key framing** of within group variance is that it is the variance _unexplained_ by differences in the groups; meaning that it's plausible that the variance observed would persist regardless of whether `\(H_0\)` or `\(H_a\)` were true.

## Total Variance 
One 

