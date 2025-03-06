# MCMC Lab


Recreate the traveling politician example with seven islands that have
the following population totals.

``` r
population.totals <- c(123,942,234,553,890,1458,99)
library(knitr)
kable(data.frame(island = as.character(1:7), pop = population.totals))
```

| island |  pop |
|:-------|-----:|
| 1      |  123 |
| 2      |  942 |
| 3      |  234 |
| 4      |  553 |
| 5      |  890 |
| 6      | 1458 |
| 7      |   99 |

### 1. (5 points)

Let your politician walk for 100 steps. Do you think this is an adequate
amount of time for the politician to spend time proportional to the
population of each island? why or why not?

### 2. (5 points)

Let your politician walk for 100,000 steps and recreate the 4 plots from
lecture:

1.  relative population of islands
2.  relative time spent in each island
3.  first 15 steps
4.  first 100 steps.

### 3. (4 points)

Do you believe 100,000 steps is sufficient for the politician to spend
time proportional to the population of each island? why or why not?

### 4. (6 points)

Assume your politician starts on island 1 on day 1, given this
algorithm, compute the following probabilities, where $l(i)$ is the
politician location on day $i$.

- $Pr[l(1) = 1]$
- $Pr[l(2) = 1]$
- $Pr[l(2) = 2]$
- $Pr[l(3) = 2]$
- $Pr[l(3) = 5]$
- finally take a guess at $Pr[l(100,000) = 3]$
