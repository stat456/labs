# Lab 11

This lab will present the
framework for a hierarchical model in the normal setting. 

``` r
library(rjags)
library(knitr)
library(tidyverse)
```


Consider a hierarchical normal model

$$y_{i[j]} \sim N(\mu_i, \sigma^2)$$ where $y_{i[j]}$ denotes that the
$j^{th}$ response is in the $i^{th}$ group and $\mu_i$ is the mean for
group $i$.

$$\mu_i \sim N(\gamma, \tau^2)$$

#### 1. (4 points)

Simulate and plot 10 group-level means ($\mu_{i}$’s).

#### 2. (4 points)

Now simulate between 2 and 10 responses for each group and plot these as
well.

#### 3. (4 points)

Run the following JAGS code for a Bayesian ANOVA using your data. Does
this model framework the model defined above? Why or why not (hint where
are the individual estimates shrunk toward)?

``` r
modelString = "
   model {
   #Likelihood
    for (i in 1:n) {
      y[i]~dnorm(mean[i],1 / sigma ^2)
      mean[i] <- mu[x[i]]
    }

   #Priors and derivatives
   for (i in 1:ngroups) {
     mu[i] ~ dnorm(0, .001) #prior
   }
   sigma.group <- sd(mu[])
   sigma ~ dunif(0, 10)
 
   #Group mean posteriors (derivatives)
   for (i in 1:ngroups) {
     Group.means[i] <- mu[i]
   }
  }
"

writeLines(modelString, con = "anovaModel.txt")
```

#### 4. (4 points)

To fit a Bayesian hierarchical model for the data generation mechanism
defined above, what parameter require prior distributions? Specify
reasonable prior distributions for these values.

#### 5. (4 points)

Now fit and interpret a hierarchical model. What values are the $\mu$
shrunk toward?

``` r
hierString = "
  model {# Likelihood
    for(i in 1:n){
    y[i] ~ dnorm(mu_vec[i], 1/sigma^2)
    mu_vec[i] <- mu[group[i]]
}

for(j in 1:J){
  mu[j] ~ dnorm(gamma, 1/tau^2)
}

# Priors
gamma ~ dnorm(0,0.001)
sigma ~ dunif(0,10)
tau ~ dunif(0,10)
}"

writeLines(hierString, con = "hierModel.txt")

dataList <- list(y = data_tib$yvals, 
                             group = data_tib$group, 
                             n = nrow(data_tib),
     J = length(levels(data_tib$group)))

jags.hier<- jags.model( file = "hierModel.txt", 
                         data = dataList, 
                         n.chains = 3, 
                         n.adapt = 5000)
update(jags.hier, 10000)

num.mcmc <- 10000
codaSamplesHier <- coda.samples( jags.hier, 
                             variable.names = c('mu', 'gamma'), 
                             n.iter = num.mcmc)

summary(codaSamplesHier)
```

#### 6. (4 points)

Compare the estimates for each group between the approach on parts 3 and
5.
