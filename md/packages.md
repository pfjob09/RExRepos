---
license: Creative Commons BY-SA
author: Daniel Wollschlaeger
title: "Add-on packages"
categories: [RBasics]
rerCat: R_Basics
tags: [Packages, CRAN]
---

Add-on packages
=========================

TODO
-------------------------

 - link to workspace for `search()`

Install required packages
-------------------------

[`coin`](http://cran.r-project.org/package=coin)


```r
wants <- c("coin")
has   <- wants %in% rownames(installed.packages())
if(any(!has)) install.packages(wants[!has])
```

Useful online resources
-------------------------

 * [Comprehensive R Archive Network (CRAN)](http://cran.r-project.org/web/packages/): These mirror servers are the primary hosts for contributed R add-on packages
 * [CRAN Task Views](http://cran.r-project.org/web/views/): An organized and maintained list of packages useful for a specific data analysis task (e.g., multivariate statistics or psychometric models)
 * [BioConductor](http://www.bioconductor.org/): A host for add-on packages especially useful for genomic data analysis
 * [r4stats package overview](http://r4stats.com/articles/add-ons/): A list of popular R packages and how their functionality compares to SPSS/SAS

Install and update add-on packages
-------------------------

### Information about installed packages


```r
.libPaths()
```

```
[1] "/home/dw/R/x86_64-pc-linux-gnu-library/3.1"
[2] "/usr/local/lib/R/site-library"             
[3] "/usr/lib/R/site-library"                   
[4] "/usr/lib/R/library"                        
```


```r
installed.packages()
# not shown (very long output)
```


```r
rownames(installed.packages())
```

```
  [1] "acepack"         "AICcmodavg"      "binom"          
  [4] "bitops"          "boot"            "brew"           
  [7] "car"             "cluster"         "codetools"      
 [10] "coin"            "colorspace"      "CompQuadForm"   
 [13] "DBI"             "DEoptimR"        "DescTools"      
 [16] "devtools"        "dichromat"       "digest"         
 [19] "e1071"           "effects"         "ellipse"        
 [22] "energy"          "epitools"        "evaluate"       
 [25] "expm"            "formatR"         "Formula"        
 [28] "gdata"           "GGally"          "ggplot2"        
 [31] "glmnet"          "GPArotation"     "gtable"         
 [34] "gtools"          "hexbin"          "highr"          
 [37] "Hmisc"           "httr"            "ICS"            
 [40] "ICSNP"           "irr"             "jsonlite"       
 [43] "KernSmooth"      "knitr"           "labeling"       
 [46] "latticeExtra"    "leaps"           "lme4"           
 [49] "lmtest"          "lpSolve"         "manipulate"     
 [52] "markdown"        "MASS"            "maxLik"         
 [55] "mediation"       "memoise"         "mgcv"           
 [58] "mime"            "minqa"           "miscTools"      
 [61] "mlogit"          "modeest"         "modeltools"     
 [64] "multcomp"        "multilevel"      "munsell"        
 [67] "mvoutlier"       "mvtnorm"         "nlme"           
 [70] "nloptr"          "nortest"         "ordinal"        
 [73] "pbkrtest"        "pcaPP"           "permute"        
 [76] "perturb"         "phia"            "plotrix"        
 [79] "pls"             "plyr"            "polspline"      
 [82] "polycor"         "pracma"          "pROC"           
 [85] "proto"           "pscl"            "psych"          
 [88] "quadprog"        "QuantPsyc"       "quantreg"       
 [91] "RColorBrewer"    "Rcpp"            "RcppArmadillo"  
 [94] "RcppEigen"       "RCurl"           "reshape"        
 [97] "reshape2"        "rgl"             "rms"            
[100] "robCompositions" "robustbase"      "rockchalk"      
[103] "rrcov"           "RSQLite"         "rstudio"        
[106] "rstudioapi"      "sandwich"        "scales"         
[109] "sets"            "sfsmisc"         "sgeostat"       
[112] "shotGroups"      "sos"             "SparseM"        
[115] "statmod"         "stringr"         "survey"         
[118] "TH.data"         "tseries"         "ucminf"         
[121] "unmarked"        "vegan"           "VGAM"           
[124] "whisker"         "zoo"             "base"           
[127] "boot"            "class"           "cluster"        
[130] "codetools"       "compiler"        "datasets"       
[133] "foreign"         "graphics"        "grDevices"      
[136] "grid"            "KernSmooth"      "lattice"        
[139] "MASS"            "Matrix"          "methods"        
[142] "mgcv"            "nlme"            "nnet"           
[145] "parallel"        "rpart"           "spatial"        
[148] "splines"         "stats"           "stats4"         
[151] "survival"        "tcltk"           "tools"          
[154] "utils"          
```


```r
library()
# not shown (opens separate window)
```

### Install and remove add-on packages


```r
setRepositories("http://cran.ma.imperial.ac.uk/")
install.packages(c("coin", "car"))
install.packages("coin", repos="http://cran.at.r-project.org/")
update.packages()
# remove.packages("coin")
# not shown
```

See `install_github()` from package [`devtools`](http://cran.r-project.org/package=devtools) for installing from [GitHub](http://github.com/) repositories.

### Information provided by add-on packages


```r
help(package=coin)
vignette()
vignette("coin_implementation")
# not shown (opens separate window)
```

Use add-on packages
-------------------------

### Load add-on package

`library()` throws an error if the package cannot be found


```r
library(coin)
```

Get a return value `TRUE` or `FALSE` that indicates whether package could be loaded


```r
library(coin, logical.return=TRUE)
```

```
[1] TRUE
```

`require()` gives a warning if the package cannot be found


```r
require(doesNotExist)
```

```
Warning in library(package, lib.loc = lib.loc, character.only = TRUE,
logical.return = TRUE, : there is no package called 'doesNotExist'
```

Explicitly state which package a function is from - if multiple packages export a function with the same name.


```r
base::mean(c(1, 3, 4))
```

```
[1] 2.666667
```

### Know which packages are currently loaded


```r
sessionInfo()
```

```
R version 3.1.1 (2014-07-10)
Platform: x86_64-pc-linux-gnu (64-bit)

locale:
 [1] LC_CTYPE=de_DE.UTF-8       LC_NUMERIC=C              
 [3] LC_TIME=de_DE.UTF-8        LC_COLLATE=de_DE.UTF-8    
 [5] LC_MONETARY=de_DE.UTF-8    LC_MESSAGES=de_DE.UTF-8   
 [7] LC_PAPER=de_DE.UTF-8       LC_NAME=C                 
 [9] LC_ADDRESS=C               LC_TELEPHONE=C            
[11] LC_MEASUREMENT=de_DE.UTF-8 LC_IDENTIFICATION=C       

attached base packages:
[1] splines   methods   stats     graphics  grDevices utils     datasets 
[8] base     

other attached packages:
[1] coin_1.0-24     survival_2.37-7 stringr_0.6.2   knitr_1.7      

loaded via a namespace (and not attached):
[1] evaluate_0.5.5    formatR_1.0       modeltools_0.2-21 mvtnorm_1.0-0    
[5] stats4_3.1.1      tools_3.1.1      
```

```r
search()
```

```
 [1] ".GlobalEnv"        "package:coin"      "package:survival" 
 [4] "package:splines"   "package:methods"   "package:stringr"  
 [7] "package:knitr"     "package:stats"     "package:graphics" 
[10] "package:grDevices" "package:utils"     "package:datasets" 
[13] "Autoloads"         "package:base"     
```

### Un-load a package


```r
detach(package:coin)
```

Data sets from add-on packages
-------------------------


```r
data(package="coin")
data(jobsatisfaction, package="coin")
# not shown (opens separate window)
```

Detach (automatically) loaded packages (if possible)
-------------------------


```r
try(detach(package:coin))
try(detach(package:survival))
try(detach(package:splines))
```

Get the article source from GitHub
----------------------------------------------

[R markdown](https://github.com/dwoll/RExRepos/raw/master/Rmd/packages.Rmd) - [markdown](https://github.com/dwoll/RExRepos/raw/master/md/packages.md) - [R code](https://github.com/dwoll/RExRepos/raw/master/R/packages.R) - [all posts](https://github.com/dwoll/RExRepos/)
