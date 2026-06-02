---
tags:
- DS
MOC: Sciences
---
[[_0000 Home|Home]] | [[_0004 Sciences MOC |Back to Sciences MOC]] | [[SC Datascience index|Back to index]]

# Statistical Inference
- This is the principal that allows us to infer new knowledge from our data that's based on a population
- It's also the process of drawing conclusions "about a population using noisy statistical data where uncertainty must be accounted for". 
- In other words, statistical inference lets scientists formulate conclusions from data and quantify the uncertainty arising from using incomplete data.
- A statistic (singular) is a number computed from a sample of data. 
- We use statistics to infer information about a population. 
- A random variable is an outcome from an experiment.
- Deterministic processes, such as computing means or variances, applied to random variables, produce additional random variables which have their own distributions. It's important to keep straight which distributions you're talking about.
- There are two broad flavors of inference. The first is frequency, which uses "long run proportion of times an event occurs in independent, identically distributed repetitions
- This means that we're defining the parameters of a population based on the percent probability that a specific outcome will take place from repeating an an action a certain number of times (frequency)
- The second is Bayesian in which the probability estimate for a hypothesis is updated as additional evidence is acquired
## Probability
- This is the foundation of statistical analysis
### Rules of probability
- Given a random experiment (say rolling a die) a probability measure is a population quantity that summarizes the randomness
	- What this means, is that while rolling a die, at first, may seem totally random, as we have no idea what outcome to expect or why, we can use probability measures to summarize the expected outcomes and patterns in the population of die rolls
	- For example, we can say that theoretically in a fair die, there is a 1/6 chance to get any of the 6 faces
	- There is also a 50% chance of an even or an odd number to be the outcome
	- As well, there is a 2/6 chance of getting a multiple of 3 (3 or 6)
	- By doing that, we used probability to summarize the randomness into possibilities
	- However, this still assumes a fair die, but empirical data from actually rolling the die, may reveal that the die is loaded, and favors specific faces on each roll, changing the theoretical truth of the data
- Probability calculus follows a set of rules based on the previous explanation
- Every possible outcome should be assigned a number between 0 and 1, so the sum of the probabilities of all the possible events (e), is 1
- The probability something happens to begin with, which in this case, that the die is rolled at all is 1
#### Independent events
- The probability of two independent events A and B occurring is the product of their probabilities $P(A$&$B) = P(A) * P(B)$
- In the case of a fair die that would be 1/6 * 1/6 = 1/36
- 
#### Union
- Also, the probability of the union of any two sets of outcomes that have nothing in common (mutually exclusive) is the sum of their respective probabilities
	- This means that if I have 2 possible outcomes of rolling the die, such as getting a even number or an odd one, these two outcomes are mutually exclusive, since a number can't be both even and odd
	- The union of the two possible outcomes, is the probability of getting any of the 3 even number + the probability of getting any of the 3 odd numbers, giving us the formula $P(A U B) = P(A) + P(B)$
	- This formula explains that  $P(A)$  is the probability of an even number outcome, which include 3 possible faces, so 1/6 + 1/6 + 1/6 which is 3/6 or 0.5 (50%), and the same goes for $P(B)$, in which case the probability of this union, is the full 100%
	- This is because this union covers all possible probabilities, but we could just have easily looked at the probability of 2 different outcomes, which is getting 1 or 2, or getting 3 or 4
	- In this case we use the same formula for 2 mutually exclusive sets, and see that the probability is 4/6 = 0.66 (66%)
- Intuitively, we should be aware that the probability of something happening, is 1 minus the probability of the opposite, which is true for mutually exclusive cases, such as the probability of an even number being 1 - 0.5 (probability of an odd number) = 0.5
- If we have an event A, which implies the occurrence of an event B, then the probability of A occurring is less that that of B occurring
	- This is because by this definition, A becomes a subset of B
	- ![[Pasted image 20260527101442.png]]
	- This for example means that if we get the number 2, then this implies we have an even number, but we can have an even number if we get 4 or 6 too
	- So if the probability of an even number is B and that of 2 is A this means that $P(B) > P(A)$
#### Intersection
- What about the union of the probability we get a number that's a multiple of 2, or a one that's a multiple of 3?
- Here we have an intersection, because $P(A)$ becomes the probability of 2, 4 or 6 and $P(B)$ is 3 or 6, so the 6 is shared
- So the probability of getting a number of either set is now the sum of the probabilities of the first set + the 2nd set, minus the intersection, giving us $P(A U B) = P(A) + P(B) - P(A ∩ B)$ since if we don't do this, we will double count the probability of 6, giving us a higher probability than reality
## Probability mass functions
- When talking about probabilities associated with distributions, like the normal distribution represented with a bell curve, we're talking about population quantities, not statements about the data
- Now, we want to collect data that will be used to estimate properties of the population
### Random variables
- A numerical outcome of an experiment which comes in one of two forms
	- Discrete: Basically categorical data
		- These are modeled using the Poisson function
	- Continuous: Numerical range of data
- A probability mass function (PMF) evaluated at a specific value corresponds to the probability that a random variable takes that value
	- It must always be larger than or equal to 0
	- The sum of the possible values must add up to 1
- So basically, this is a function that takes a value as input and returns the probability a random variable will be equal to this value as the output, such as assigning 1/6 as the probability for any face of a die after a die roll
- Example of these functions are
	- The binomial function for binary values, such as flipping a coin
	- The Poisson function for modelling counts
- The most famous PMF is the binomial one, with the most famous example being flipping a coin, which results in a specific distribution known as the Bernoulli distribution
#### Bernoulli distribution
- In this example, tails = 0 and heads = 1
- In the equation for this function, capital X represents either 0 or 1, which is the random variable itself, or the idea of the coin flip, and lower case x is a placeholder that we plug a number into $P(X) = (1/2)^x (1/2)^{1-x} for X = 0,1$ which represents the actual observed data after the coin has been tossed
- For $P(0) = (1/2)^0 (1/2)^{1-0}$ or $P(1) = (1/2)^1 (1/2)^{1-1}$ we get 1/2 in the case of a fair coin
- For an unfair coin where the probability isn't going to be half, we go with $P(X) = ϴ^x 1-ϴ^{1-x}$ where $P(1) = ϴ$ and $P(0) = 1 - ϴ$
- This is useful for modelling the prevalence of something, as it's not dissimilar from the flip of a biased coin with a success probability of theta
- In simpler terms, we can think of this as the distribution of two outcomes, success or failure
- Note, that uppercase X and lowercase x both use the same values, 0,1 but they mean different things. Uppercase X is the variable name (heads, tails) while lowercase x is the value (0,1), but statisticians are annoying and decided to name heads and tails 0 and 1 to confuse everyone
## Probability density functions
- While PMF is meant for discrete values, PDF is meant for continuous ones
- A PDF must be larger than or equal to zero everywhere, and the area under it must be equal to one
- Areas under a PDF correspond to probabilities for that random variable
- Say we want to find the probability that 20 to 60% of calls in a call center gets addressed
- That probability can be represented as follows
![[Pasted image 20260602001428.png]]
- Where the height is $f(x) = 2x$ for 0<x<1 and 0 otherwise and the base is $f(x) = 1/2(x)$ so the PDF is base x height or $1/2(x) * (2x)$ which is  $x^2$
- Note that the rules we defined are both respected too, as the values are higher than or equal to 0, and the area under this right angled triangle is 1/2 the base * the height of the triangle, which is 1/2 * 2 = 1
- So what's the probability that 75% of the calls get addressed?
- We need to get the height at that probability times half of the base, again, only up to that probability, so the height turned out to be 1.5, and the base 0.75/2
- We can use `pbeta` to calculate this density, as it's an instance of a beta distribution, although that's a little overkill here
```R
pbeta(0.75, 2, 1)
[1] 0.5625
```
- So the probability here is 56% of addressing 75% of the calls
## Cumulative distribution function
- The CDF of a random variable X returns the probability that the random variable is less than or equal to the value x $F(x) = P(X <= x)$
- The CDF works for both discrete and continuous values
- `pbeta` actually returns a CDF
![[Pasted image 20260602005536.png]]
- From the image we can see how the PDF is a distribution, and it has a peak, while the CDF is truly cumulative
- The idea is still that, maybe landing in the far right of the PDF is as unlikely as the far left, as they're both extremes, but the CDF doesn't look at the likelyhood of the far right, it looks at the likelyhood of the any point you give it, or anything before it
- Due to this, figuring of the probability of landing in a range between 0.2 and 0.8, is actually calculated by getting the CDF of both values, and subtracting them from each other
```R
pbeta(0.8, 2, 1) - pbeta(0.2, 2, 1)
[1] 0.6
```
## Survival function
- This is just 1 - F(x), so it's the inverse of the cumulative function, it's the probability of being greater than a value instead of equal or less than said value $S(X) = P(X > x)$
- It's important to note that these probabilities, help us build models for making estimations about a population, from a sample, given the properties of the population
- Looking back at `pbeta` and considering `qbeta` for quantiles. These two are inverse functions
```R
pbeta(0.75, 2, 1)
[1] 0.5625
qbeta(0.5625, 2, 1)
[1] 0.75
```
- What we're saying with these two functions is, "What is the probability that I can address 75% or less of all incoming calls?" or in other words, the probability of landing in the 75th percentile, and the answer was 56%
- The inverse question is "What is the percentage of calls that I'm sure I can address with a 56% certainty?" and that is 75%. So we're basically saying, that we're 56% certain, we will be in the 75th percentile