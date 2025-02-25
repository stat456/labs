# Lab 6


For this lab we will again use the Seattle Housing dataset
<http://www.math.montana.edu/ahoegh/teaching/stat408/datasets/SeattleHousing.csv>.
Assume your goal is to model housing prices in Seattle using a normal
distribution.

### Q1.

#### a. (4 pts)

Write out the sampling model for this dataset, define all variables. For
simplicity, feel free to stick with a normal distribution.

#### b. (4 pts)

State and justify prior distributions for $\mu$ and $\sigma^2$. Plot
both of these prior distributions.

#### c. (4 pts)

Using the priors above fit the model in JAGS. Include summaries of your
output.

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
Seattle <- read_csv('http://www.math.montana.edu/ahoegh/teaching/stat408/datasets/SeattleHousing.csv')
```

    Rows: 869 Columns: 14
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    dbl (14): price, bedrooms, bathrooms, sqft_living, sqft_lot, floors, waterfr...

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

#### d. (4 pts)

Print and interpret an HDI for $\mu$.

#### e. (2 pts)

Calculate the $Pr[\mu > 650,000]$.

#### f. (2 pts)

Calculate the probability of an individual house, say $y^{*}$ costing
more than $\$650,000.$

This calculation uses a posterior predictive distribution. For this
distribution we compute the following interval by sampling
$$p(y^*|y) = \int p(y^*|\theta) p(\theta|y) d\theta$$ where
$p(\theta|y)$ is the posterior distribution, and $y^*$ is a new data
point that follows the same sampling model as the previously observed
data, $y$.

In practice, this is done by:

- taking posterior samples from $p(\theta|y)$, using JAGS in this case
- including those posterior samples in the sampling model to simulate a
  distribution for a new data point.

#### g. (4 pts)

Summarize the results from part d so that your aunt who is a realtor in
Billings can understand.
