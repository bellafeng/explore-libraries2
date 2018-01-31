01\_explore-libraries\_jenny.R
================
gfeng
Wed Jan 31 14:10:09 2018

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "C:/Users/gfeng/Documents/R/R-3.4.2/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "C:/Users/gfeng/DOCUME~1/R/R-34~1.2/library"

``` r
library(fs)
```

    ## Warning: package 'fs' was built under R version 3.4.3

``` r
path_real(.Library)
```

    ## C:/Users/gfeng/Documents/R/R-3.4.2/library

Installed packages

``` r
library(tidyverse)
```

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 223

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##                                      LibPath    Priority     n
    ##                                        <chr>       <chr> <int>
    ## 1 C:/Users/gfeng/Documents/R/R-3.4.2/library        base    14
    ## 2 C:/Users/gfeng/Documents/R/R-3.4.2/library recommended    15
    ## 3 C:/Users/gfeng/Documents/R/R-3.4.2/library        <NA>   194

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n       prop
    ##              <chr> <int>      <dbl>
    ## 1               no   101 0.45291480
    ## 2              yes   114 0.51121076
    ## 3             <NA>     8 0.03587444

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 4 x 3
    ##   Built     n        prop
    ##   <chr> <int>       <dbl>
    ## 1 3.4.0     1 0.004484305
    ## 2 3.4.1    31 0.139013453
    ## 3 3.4.2   157 0.704035874
    ## 4 3.4.3    34 0.152466368

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##   [1] "aRxiv"                "assertthat"           "backports"           
    ##   [4] "base64enc"            "BBmisc"               "BH"                  
    ##   [7] "bibtex"               "bindr"                "bindrcpp"            
    ##  [10] "bit"                  "bit64"                "bitops"              
    ##  [13] "blogdown"             "bookdown"             "broom"               
    ##  [16] "caret"                "caTools"              "cellranger"          
    ##  [19] "checkmate"            "cli"                  "clipr"               
    ##  [22] "clisymbols"           "colorspace"           "combinat"            
    ##  [25] "crayon"               "crminer"              "crosstalk"           
    ##  [28] "crul"                 "curl"                 "CVST"                
    ##  [31] "data.table"           "DBI"                  "ddalpha"             
    ##  [34] "DEoptimR"             "desc"                 "devtools"            
    ##  [37] "dichromat"            "digest"               "dimRed"              
    ##  [40] "DoseFinding"          "dplyr"                "DRR"                 
    ##  [43] "e1071"                "enc"                  "evaluate"            
    ##  [46] "farff"                "forcats"              "foreach"             
    ##  [49] "forecast"             "fracdiff"             "fs"                  
    ##  [52] "fulltext"             "gdata"                "ggplot2"             
    ##  [55] "gh"                   "git2r"                "glmnet"              
    ##  [58] "glue"                 "gmodels"              "gower"               
    ##  [61] "gridSVG"              "gtable"               "gtools"              
    ##  [64] "h2o"                  "haven"                "here"                
    ##  [67] "hexbin"               "highr"                "hms"                 
    ##  [70] "hoardr"               "htmltools"            "htmlwidgets"         
    ##  [73] "httpcode"             "httpuv"               "httr"                
    ##  [76] "hunspell"             "ini"                  "ipred"               
    ##  [79] "iterators"            "janeaustenr"          "jsonlite"            
    ##  [82] "kernlab"              "klaR"                 "knitr"               
    ##  [85] "labeling"             "lava"                 "lazyeval"            
    ##  [88] "lime"                 "lmtest"               "lubridate"           
    ##  [91] "magrittr"             "mailR"                "markdown"            
    ##  [94] "memoise"              "microdemic"           "mime"                
    ##  [97] "miniUI"               "mnormt"               "ModelMetrics"        
    ## [100] "modelr"               "munsell"              "mvtnorm"             
    ## [103] "NLP"                  "numDeriv"             "openssl"             
    ## [106] "padr"                 "pander"               "pdftools"            
    ## [109] "PerformanceAnalytics" "pkgconfig"            "plogr"               
    ## [112] "plotly"               "plotROC"              "plyr"                
    ## [115] "prodlim"              "psych"                "purrr"               
    ## [118] "quadprog"             "Quandl"               "quantmod"            
    ## [121] "R.cache"              "R.methodsS3"          "R.oo"                
    ## [124] "R.utils"              "R6"                   "rappdirs"            
    ## [127] "RColorBrewer"         "Rcpp"                 "RcppArmadillo"       
    ## [130] "RcppEigen"            "RcppRoll"             "rcrossref"           
    ## [133] "RCurl"                "readr"                "readxl"              
    ## [136] "recipes"              "rematch"              "rematch2"            
    ## [139] "rentrez"              "reshape2"             "rJava"               
    ## [142] "rjson"                "rlang"                "rmarkdown"           
    ## [145] "robustbase"           "rplos"                "rprojroot"           
    ## [148] "rredis"               "rstudioapi"           "rvest"               
    ## [151] "scales"               "selectr"              "servr"               
    ## [154] "sfsmisc"              "shiny"                "shinythemes"         
    ## [157] "slam"                 "SnowballC"            "solrium"             
    ## [160] "sourcetools"          "stringdist"           "stringi"             
    ## [163] "stringr"              "styler"               "tibble"              
    ## [166] "tidyquant"            "tidyr"                "tidyselect"          
    ## [169] "tidytext"             "tidyverse"            "timeDate"            
    ## [172] "timeSeries"           "timetk"               "tm"                  
    ## [175] "tokenizers"           "translations"         "triebeard"           
    ## [178] "tseries"              "TTR"                  "twitteR"             
    ## [181] "urltools"             "usethis"              "viridisLite"         
    ## [184] "whisker"              "withr"                "wordcloud"           
    ## [187] "XLConnect"            "XLConnectJars"        "XML"                 
    ## [190] "xml2"                 "xtable"               "xts"                 
    ## [193] "yaml"                 "zoo"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n      prop
    ##    <lgl> <int>     <dbl>
    ## 1  FALSE   109 0.4887892
    ## 2   TRUE   114 0.5112108
