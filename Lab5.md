# Lab 5


Suppose you have been hired as a statistical consultant by a NBA
expansion team that will be moving to Big Sky, MT. Your goal is to help
identify basketball players to bring to Montana.

Specifically, the team has one more spot to fill and is looking to add a
free throw shooting specialist. Here are two options:

- Bugs Bunny. Bugs Bunny is a shooting guard that was 2 for 7 on free
  throws last year.
- Tasmanian Devil. Tasmanian Devil is a center that was 25 for 40 on
  free throws last year.

## 1.

Use a binomial model, along with uniform prior of the probability
parameter, to model the free throw shooting for Bugs Bunny and Tasmanian
Devil.

### a (4 points)

Plot and summarize the posterior distribution for each player.

### b (4 points)

Using the posteriors above, compute the probability that Bugs Bunny has
a higher shooting percentage ($\theta$ parameter in the binary setting).

#### c (2 points)

If both players had 25 free throws, who do you think will make more free
throws? Why?

## 2.

Now assume that we know that shooting guards, like Bugs Bunny, tend to
make about 80 percent of free throws and centers, like Tasmanian Devil,
tend to make about 60 percent of free throws. This information can be
imparted by more informative priors: Use Beta(40,10) for Bugs Bunny and
Beta(30,20) for Tasmanian Devil as the prior distributions.

### a (4 points)

Plot and summarize the posterior distribution for each player.

### b (4 points)

Using the posteriors above, compute the probability that Bugs Bunny has
a higher shooting percentage ($\theta$ parameter in the binary setting).

### c (2 points)

If both players had 25 free throws, who do you think will make more free
throws? Why?

### 3. (4 points)

Reflect on the results from question a and question b. Which analysis,
and associated prior, do you prefer, why?
