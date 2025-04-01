# Lab 9


For this question, we will use a historical data set with NCAA
tournament results.

``` r
library(rjags)
library(knitr)
library(tidyverse)
ncaa <- read_csv('https://raw.githubusercontent.com/stat456/labs/main/Lab9_data.csv') %>%
  filter(Seed.Diff != 0) %>%
  mutate(Seed.Diff = -1 * Seed.Diff,
         SAG.Diff = -1 * SAG.Diff)
```

#### 1. (4 points)

Create two figures to explore the impact of `Seed.Diff` and `SAG.Diff`
on `Score.Diff.` Add a smoother line to approximate the relationship.

#### 2. (4 points)

Write a short caption to accompany each figure.

#### 3. (4 points)

Deviance Information Criteria (DIC) is a Bayesian analog to AIC.
Consider the two model below, which do you prefer and why?

``` r
# Model String
modelString = "model {
  for ( i in 1:N ) {
    y[i] ~ dnorm(beta0 + beta1 * x[i], 1/sigma^2) # sampling model
  }
  beta0 ~ dnorm(M0,1/S0^2)
  beta1 ~ dnorm(M1, 1 / S1^2)
  sigma ~ dunif(0,C)
} "
writeLines( modelString, con='NORMmodel.txt')
```

``` r
# Runs JAGS Model: Seeds
seeds_model <- jags.model( file = "NORMmodel.txt", 
                           data =  list(y = ncaa$Score.Diff, 
                                        x = ncaa$Seed.Diff,
                                        N = nrow(ncaa), 
                                        M0 = 0, 
                                        S0 = .001,
                                        M1 = 1, 
                                        S1 = 3, 
                                        C = 100),
                         n.chains = 2, n.adapt = 1000)
```

    Compiling model graph
       Resolving undeclared variables
       Allocating nodes
    Graph information:
       Observed stochastic nodes: 1182
       Unobserved stochastic nodes: 3
       Total graph size: 2410

    Initializing model

``` r
update(seeds_model, n.iter = 1000)

seeds_coda <- coda.samples(seeds_model, 
                          variable.names = c('beta0','beta1', 'sigma'), 
                          n.iter = 5000)

summary(seeds_coda)
```


    Iterations = 2001:7000
    Thinning interval = 1 
    Number of chains = 2 
    Sample size per chain = 5000 

    1. Empirical mean and standard deviation for each variable,
       plus standard error of the mean:

               Mean       SD  Naive SE Time-series SE
    beta0 5.309e-06 0.001015 1.015e-05      1.015e-05
    beta1 1.123e+00 0.044333 4.433e-04      4.433e-04
    sigma 1.170e+01 0.243835 2.438e-03      3.062e-03

    2. Quantiles for each variable:

               2.5%        25%       50%       75%     97.5%
    beta0 -0.001951 -0.0006835 7.328e-06 6.831e-04  0.002038
    beta1  1.038831  1.0924590 1.123e+00 1.153e+00  1.211206
    sigma 11.230631 11.5319542 1.170e+01 1.187e+01 12.186572

``` r
dic_seed <- dic.samples(seeds_model, 5000)
dic_seed
```

    Mean deviance:  9166 
    penalty 2.022 
    Penalized deviance: 9168 

``` r
# Runs JAGS Model: Sagarin
sag_model <- jags.model( file = "NORMmodel.txt", 
                           data =  list(y = ncaa$Score.Diff, 
                                        x = ncaa$SAG.Diff,
                                        N = nrow(ncaa), 
                                        M0 = 0, 
                                        S0 = .001,
                                        M1 = .5, 
                                        S1 = 3, 
                                        C = 100),
                         n.chains = 2, n.adapt = 1000)
```

    Compiling model graph
       Resolving undeclared variables
       Allocating nodes
    Graph information:
       Observed stochastic nodes: 1182
       Unobserved stochastic nodes: 3
       Total graph size: 2826

    Initializing model

``` r
update(sag_model, n.iter = 1000)

sag_coda <- coda.samples(sag_model, 
                          variable.names = c('beta0','beta1', 'sigma'), 
                          n.iter = 5000)

summary(sag_coda)
```


    Iterations = 2001:7000
    Thinning interval = 1 
    Number of chains = 2 
    Sample size per chain = 5000 

    1. Empirical mean and standard deviation for each variable,
       plus standard error of the mean:

                Mean       SD  Naive SE Time-series SE
    beta0 -9.631e-06 0.001005 1.005e-05      1.005e-05
    beta1  1.266e-01 0.004642 4.642e-05      4.716e-05
    sigma  1.134e+01 0.235838 2.358e-03      3.088e-03

    2. Quantiles for each variable:

               2.5%        25%        50%       75%     97.5%
    beta0 -0.001965 -0.0006967 -1.186e-05 6.668e-04  0.001942
    beta1  0.117597  0.1234482  1.266e-01 1.297e-01  0.135717
    sigma 10.889123 11.1845187  1.134e+01 1.150e+01 11.828145

``` r
dic_sag <- dic.samples(sag_model, 5000)
dic_sag
```

    Mean deviance:  9093 
    penalty 2.04 
    Penalized deviance: 9095 

``` r
diffdic(dic_seed, dic_sag)
```

    Difference: 72.60869
    Sample standard error: 23.96242

#### 4. (4 points)

Also consider a model that includes both Sagarin and the seeds.
Additionally, you can also consider interactions and/or non-linearity
terms as well as the information on the highest seed to create a more
sophisticated model. Choose the best one based on DIC.

#### 5. (4 points)

Write out the formal model you’ve selected in part 4, including all
priors.

#### 6. (4 points)

Interpret your parameters in the model and summarize your findings.
Assume you are telling your parents how statistics can be useful for
filling out an NCAA bracket.

#### 7. (4 points)

Construct a posterior predictive distribution to calculate the winning
probability of the following games (in 2023 when the Cats were in the
tournament), Note your model is likely set to predict the point spread
for the higher seed:

1.  Montana State (Seed: 14, Sagarin: 133) vs. Kansas State (Seed: 3,
    Sagarin: 18)
2.  Purdue (Seed: 1, Sagarin: 9) vs. Farleigh Dickinson (Seed: 16,
    Sagarin: 310)
3.  Arizona (Seed: 2, Sagarin: 10) vs. Princeton (Seed 15:, Sagarin:
    118)
4.  Connecticut (Seed: 4, Sagarin: 4) vs. Iona (Seed: 13, Sagarin: 86)

Create a visualization of the posterior predictive distributions for
each of these 4 scenarios and include the upset probabilities on the
figure.
