# Lab 8


### T-test

For this question, we will use classical t-tests and Bayesian analogues.

#### 1.

Use the OK Cupid dataset and test the following claim, the mean height
OK Cupid respondents reporting their body type as `athletic` is
different than 70 inches.

``` r
library(tidyverse)
okc <- read_csv('http://www.math.montana.edu/ahoegh/teaching/stat408/datasets/OKCupid_profiles_clean.csv')
```

1.  (4 points)

Use a t-test `t.test()` to answer this research question. Print
statistical output **AND** summarize your results in writing.

2.  (8 points)

Use an equivalent Bayesian model specified in JAGS to answer this
research question.

1.  Write out your statistical model and priors.
2.  Assume that your ROPE is 1/4 inch. Print statistical output, create
    a data visualization, **AND** summarize your results in writing.

#### 2.

Now consider whether there is a height difference between OK Cupid
respondents self-reporting their body type as “athletic” and those
self-reporting their body type as “fit”

1.  (4 points)

Use a t-test `t.test()` to answer this research question. Print
statistical output **AND** summarize your results in writing.

2.  (8 points)

Use an equivalent Bayesian model specified in JAGS to answer this
research question.

1.  Write out your statistical model and priors.
2.  Assume that your ROPE is 1/4 inch. Print statistical output, create
    a data visualization, **AND** summarize your results in writing.
