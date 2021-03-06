---
license: Creative Commons BY-SA
author: Daniel Wollschlaeger
title: "Association tests and measures for unordered categorical variables"
categories: [Descriptive]
rerCat: Descriptive
tags: [Association]
---




TODO
-------------------------

 - link to correlation, associationOrder, diagCategorical

Install required packages
-------------------------

[`coin`](http://cran.r-project.org/package=coin), [`DescTools`](http://cran.r-project.org/package=DescTools)


```r
wants <- c("coin", "DescTools")
has   <- wants %in% rownames(installed.packages())
if(any(!has)) install.packages(wants[!has])
```

$(2 \times 2)$-tables
-------------------------

### Fisher's exact test


```r
disease <- factor(rep(c("no", "yes"),   c(10, 5)))
diagN   <- rep(c("isHealthy", "isIll"), c( 8, 2))
diagY   <- rep(c("isHealthy", "isIll"), c( 1, 4))
diagT   <- factor(c(diagN, diagY))
contT1  <- table(disease, diagT)
addmargins(contT1)
```

```
       diagT
disease isHealthy isIll Sum
    no          8     2  10
    yes         1     4   5
    Sum         9     6  15
```


```r
fisher.test(contT1, alternative="greater")
```

```

	Fisher's Exact Test for Count Data

data:  contT1
p-value = 0.04695
alternative hypothesis: true odds ratio is greater than 1
95 percent confidence interval:
 1.031491      Inf
sample estimates:
odds ratio 
  12.49706 
```

### Prevalence, sensitivity, specificity, CCR, $F$-score


```r
TN <- c11 <- contT1[1, 1]       ## true negative
TP <- c22 <- contT1[2, 2]       ## true positive / hit
FP <- c12 <- contT1[1, 2]       ## false positive
FN <- c21 <- contT1[2, 1]       ## false negative / miss
```


```r
(prevalence <- sum(contT1[2, ]) / sum(contT1))
```

```
[1] 0.3333333
```


```r
(sensitivity <- recall <- TP / (TP+FN))
```

```
[1] 0.8
```


```r
(specificity <- TN / (TN+FP))
```

```
[1] 0.8
```


```r
(relevance <- precision <- TP / (TP+FP))
```

```
[1] 0.6666667
```

Correct classification rate (CCR)


```r
(CCR <- sum(diag(contT1)) / sum(contT1))
```

```
[1] 0.8
```

$F$-score


```r
(Fval <- 1 / mean(1 / c(precision, recall)))
```

```
[1] 0.7272727
```

### Odds ratio, Yule's $Q$ and risk ratio

#### Odds ratio


```r
library(DescTools)                    # for OddsRatio()
(OR <- OddsRatio(contT1, conf.level=0.95))  # odds ratio
```

```
odds ratio     lwr.ci     upr.ci 
 16.000000   1.092859 234.247896 
```

#### Yule's $Q$


```r
library(DescTools)                    # for YuleQ()
YuleQ(contT1)  ## Yule's Q
```

```
[1] 0.8823529
```

#### Risk ratio


```r
library(DescTools)                    # for RelRisk()
RelRisk(contT1)                       # relative risk
```

```
[1] 4
```


```r
(risk    <- prop.table(contT1, margin=1))
```

```
       diagT
disease isHealthy isIll
    no        0.8   0.2
    yes       0.2   0.8
```

```r
(relRisk <- risk[1, 1] / risk[2, 1])
```

```
[1] 4
```

$(r \times c)$-tables
-------------------------

### $\chi^{2}$-test


```r
set.seed(123)
N        <- 50
smokes   <- factor(sample(c("no", "yes"), N, replace=TRUE))
siblings <- factor(round(abs(rnorm(N, 1, 0.5))))
cTab     <- table(smokes, siblings)
addmargins(cTab)
```

```
      siblings
smokes  0  1  2 Sum
   no   5 16  4  25
   yes  3 19  3  25
   Sum  8 35  7  50
```


```r
chisq.test(cTab)
```

```

	Pearson's Chi-squared test

data:  cTab
X-squared = 0.9, df = 2, p-value = 0.6376
```

Also for higher-order tables

### Measures of association: $\phi$, Cramer's $V$, contingency coefficient


```r
DV1  <- cut(c(100, 76, 56, 99, 50, 62, 36, 69, 55,  17), breaks=3,
            labels=LETTERS[1:3])
DV2  <- cut(c(42,  74, 22, 99, 73, 44, 10, 68, 19, -34), breaks=3,
            labels=LETTERS[1:3])
cTab <- table(DV1, DV2)
addmargins(cTab)
```

```
     DV2
DV1    A  B  C Sum
  A    2  0  0   2
  B    0  3  2   5
  C    0  1  2   3
  Sum  2  4  4  10
```


```r
library(DescTools)
Assocs(cTab)
```

```
                       estimate  lwr.ci  upr.ci
Phi Coeff.               1.0328       -       -
Contingency Coeff.       0.7184       -       -
Cramer V                 0.7303  0.0000  1.0000
Goodman Kruskal Gamma    0.8333  0.4513  1.0000
Kendall Tau-b            0.6350  0.1884  1.0000
Stuart Tau-c             0.6000  0.1151  1.0000
Somers D C|R             0.6452  0.2040  1.0000
Somers D R|C             0.6250  0.1897  1.0000
Pearson Correlation      0.7254  0.1763  0.9302
Spearman Correlation     0.6761  0.0810  0.9159
Lambda C|R               0.5000  0.0000  1.0000
Lambda R|C               0.4000  0.0000  0.8294
Lambda sym               0.4545  0.0591  0.8500
Uncertainty Coeff. C|R   0.4774  0.1492  0.8055
Uncertainty Coeff. R|C   0.4890  0.1519  0.8260
Uncertainty Coeff. sym   0.4831  0.1522  0.8140
Mutual Information       0.7610       -       -
```

Cochran-Mantel-Haenszel test for three-way tables
-------------------------


```r
N    <- 10
myDf <- data.frame(work =factor(sample(c("home", "office"), N, replace=TRUE)),
                   sex  =factor(sample(c("f", "m"),         N, replace=TRUE)),
                   group=factor(sample(c("A", "B"), 10, replace=TRUE)))
tab3 <- xtabs(~ work + sex + group, data=myDf)
```


```r
library(coin)
cmh_test(tab3, distribution=approximate(B=9999))
```

```

	Approximative Generalized Cochran-Mantel-Haenszel Test

data:  sex by
	 work (home, office) 
	 stratified by group
chi-squared = 2.6129, p-value = 0.19
```

Detach (automatically) loaded packages (if possible)
-------------------------


```r
try(detach(package:coin))
try(detach(package:survival))
try(detach(package:splines))
try(detach(package:DescTools))
```

Get the article source from GitHub
----------------------------------------------

[R markdown](https://github.com/dwoll/RExRepos/raw/master/Rmd/association.Rmd) - [markdown](https://github.com/dwoll/RExRepos/raw/master/md/association.md) - [R code](https://github.com/dwoll/RExRepos/raw/master/R/association.R) - [all posts](https://github.com/dwoll/RExRepos/)
