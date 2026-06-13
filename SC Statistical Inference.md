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
- The probability of two independent events A and B occurring is the product of their probabilities 
- $P(A$&$B) = P(A) * P(B)$ 
- where P means probability, and also note that 
- $P(A$&$B)$ and $P(A∩B)$ 
- are the same thing, just using different syntax
- In the case of a fair die that would be 1/6 * 1/6 = 1/36
- There are two ways we can ask for this kind of probability
	- We can say, "what's the probability of getting the same number twice?" in which case, the first toss will give us a number, which one? we don't care, and the probability of getting a number in general is obviously 1, since we will always get a number from tossing the die. The 2nd part is getting the probability of getting that "specific" number again, and that's the keyword here, "specific", because now the probability becomes 1/6
	- The 2nd statement is "what's the probability of getting 4 twice?" because now, both tosses revolve around a specific number, so each toss has a 1/6 probability, and based on the previous rule, that the total probability is the product, which is 1/36
#### Union
- Also, the probability of the union of any two sets of outcomes that have nothing in common (mutually exclusive) is the sum of their respective probabilities
	- This means that if I have 2 possible outcomes of rolling the die, such as getting a even number or an odd one, these two outcomes are mutually exclusive, since a number can't be both even and odd
	- The union of the two possible outcomes, is the probability of getting any of the 3 even number + the probability of getting any of the 3 odd numbers, giving us the formula 
	- $P(A U B) = P(A) + P(B)$
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
- So the probability of getting a number of either set is now the sum of the probabilities of the first set + the 2nd set, minus the intersection, giving us 
- $P(A U B) = P(A) + P(B) - P(A ∩ B) or (P(A) * P(B))$ 
- since if we don't do this, we will double count the probability of 6, giving us a higher probability than reality
- Note that 
- $P(A∩B) = P(A) * P(B)$ 
- is for independent events, but if A is a subset of B then the intersection will be equal to the probability of A
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
- In the equation for this function, capital X represents either 0 or 1, which is the random variable itself, or the idea of the coin flip, and lower case x is a placeholder that we plug a number into 
- $P(X) = (1/2)^x (1/2)^{1-x} for X = 0,1$ 
- which represents the actual observed data after the coin has been tossed
- For 
- $P(0) = (1/2)^0 (1/2)^{1-0}$ or $P(1) = (1/2)^1 (1/2)^{1-1}$ 
- we get 1/2 in the case of a fair coin
- For an unfair coin where the probability isn't going to be half, we go with 
- $P(X) = ϴ^x 1-ϴ^{1-x}$ 
- where $P(1) = ϴ$ and $P(0) = 1 - ϴ$
- This is useful for modelling the prevalence of something, as it's not dissimilar from the flip of a biased coin with a success probability of theta
- In simpler terms, we can think of this as the distribution of two outcomes, success or failure
- Note, that uppercase X and lowercase x both use the same values, 0,1 but they mean different things. Uppercase X is the variable name (heads, tails) while lowercase x is the value (0,1), but statisticians are annoying and decided to name heads and tails 0 and 1 to confuse everyone
## Probability density functions
- While PMF is meant for discrete values, PDF is meant for continuous ones
- A PDF must be larger than or equal to zero everywhere, and the area under it must be equal to one
- Areas under a PDF correspond to probabilities for that random variable
- Say we want to find the probability that 20 to 80% of calls in a call center gets addressed
- That probability can be represented as follows
![[Pasted image 20260602001428.png]]
- The PDF is the formula for just the height of the triangle, which is basically the slope * the base since the slope is rise/run, or height/width, the PDF is = to the rise or if we just know the slope, then it's slope * base (slope = 1/2 * base that's 2 = 1 for example)
- It's important to note that there is a distinction between the values 0.2 and 0.8 as "percentage" calls answered, and the actual probability percentage of being in that range
- Note that the rules we defined are both respected too, as the values are higher than or equal to 0, and the area under is equal to 1
- So what's the probability that 75% of the calls get addressed?
- We need to get the height at that probability times half of the base, again, only up to that probability, so the height turned out to be 1.5, and the base 0.75/2
- We can use `pbeta` to calculate this density, as it's an instance of a beta distribution, although that's a little overkill here
```R
pbeta(0.75, 2, 1)
[1] 0.5625
```
- So the probability here is 56% of addressing 75% of the calls
- Again, that 56% is the 56 percentile, and is = to 56% of the area under the triangle which are different percentages than 75% as a just a value (it's confusing because the god damn course has to keep naming everything the same, like the variable X having a value x)
## Cumulative distribution function
- The CDF of a random variable X returns the probability that the random variable is less than or equal to the value x $F(x) = P(X <= x)$
- The CDF is the formula for the area of the triangle
- The CDF works for both discrete and continuous values
- The PDF is actually a derivative for continuous values of the CDF, which means integrating the PDF yields the CDF
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
- Mathematically speaking, the percentile = the probability = the area because the area has to be a maximum of 1
- There is also a notion of long and wide triangles, so the CDF can be $F(X) = X^2$  for long triangles where Y is larger than X or it can be $F(X) = X^2/4$ for wide triangles where X is larger than Y, due to the value of the slope in the equation $1/2 * base * (slope * height)$. The above functions assume a slope of 2 and 1/2 respectively, but these values are not set in stone of course, and the most important thing is for the area to be equal to 1 in the end
## Conditional probability
- This is the chance of observing an event, given that another event is true
- We already saw an example of this when we went over the chance of rolling a dice and getting an odd number
- The difference though between the previous similar example we saw, and conditional probability is the following
	- What we asked before was what is the probability of an odd number outcome? which was 1/3 because half the population is odd numbers
	- Now we're asking something slightly different, it's given that we rolled an odd number, so that event is confirmed true, what's the probability said odd number is 3? considering we just limited the population to 50%, the chance would be 1/3 again
- The mathematical representation of this is the following $P(A|B)$ or in plain words, the probability of an event A occurring given that B occurred
- The formula itself is 
- $P(A|B) = P(A$&$B)/P(B)$ 
- $P(B|A) = P(B$&$A)/P(A)$ 
- is the opposite and is equivalent to 
- $P(A|B) * P(B)/P(A)$ 
- because we can represent 
- $P(A|B) = P(A$&$B)/P(B)$  
- as 
- $P(A$&$B) = P(A|B) * P(B)$
- and since $P(A$&$B)$ is the same as $P(B$&$A)$ we can do the above substitution
	- This is a form of Bayes's Rule which relates the two conditional probabilities
- This can be useful for when we don't know $P(A)$ but we know the conditional probability of it occurring if B occurs, or if B doesn't occur, which i represented as 
- $P(A|$~$B)$ 
- where ~B is B complement, or just B not occurring/not B
- This ends up giving us 
- $P(A) = P(A|B) * P(B) + P(A|$~$B) * P($~$B)$
- Now we can substitute this into Bayes's formula getting this final monster 
- $P(B|A) = P(A|B) * P(B) / ( P(A|B) * P(B) + P(A|$~$B) * P($~$B) )$
### HIV example
- Any test with a margin for error has the two following properties, specificity, and sensitivity, where specificity is how specific the result is to whatever it is we're measuring, and sensitivity, is how well we can measure the thing
- For example, if a test kit has a 100% chance of showing if a patient has HIV, then it's highly sensitive, but if it also give a positive result if the patient has the flu, then it's not exactly specific
- We'll assume that we know the accuracy rate of a positive case, which is basically the sensitivity rate, and that of a negative case, which is basically specificity
	- That's because the test is sensitive if we have a positive result for someone who actually has HIV, and specific if we get a negative result for someone who has no HIV
- Lets say that `D` is the event itself and `+` is a positive result while `-` is a negative result
- This means that we know $P(+|D)$ (sensitivity) and $P(-|$~$D)$ (specificity)
- So what if a person has a positive test result but come from a population with only 0.1% of the population has HIV due to a prevalence of 0.001. What are the chances that he actually has it?
- Now given Bayes's formula, we have
- $P(D|+) = P(+|D) * P(D) / ( P(+|D) * P(D) + P(+|$~$D) * P($~$D) )$
- The prevalence of HIV in the population is $P(D)$, and obviously the compliment $P($~D$)$ is 1-$P(D)$ and same goes for $P(+|$~$D)$ = 1-$P(-|$~$D)$ and the test sensitivity is 99.7% with a specificity of 98.5%
- So $P(D|+) = 0.997 * 0.001 / ( 0.997 * 0.001 + 0.015 * 0.999 )$ is 0.062382681, which is 6.2%
- $P(D|+)$ is called the positive predictive value and $P(-|$~$D)$ is the negative predictive value, which is the probability the patient doesn't have the disease given a negative test result
#### The proof and importance of knowing the prevalence
- So why 6.2%? Consider this. The prevalence was 0.001, which means that only 0.1% of the population have HIV
- For a population of 1,000,000, and a test sensitivity rate of 99.7% and specificity of 98.5%, this means that we have 1000 truly sick people, and 999,000 healthy ones, at least HIV wise
- In the first group with the sick ones
	- Sensitivity of 99.7% means 997 are actually positive
	- 3 people are false negatives
- For the second group
	- Specificity is 98.5% means 984015 are truly negative
	- 14985 are false positives
- So the error rate is the sum of all false outcomes, which is 14985 + 3 = 14988, and the error rate is 14988/1,000,000 = 0.014988 or 1.4988%
	- This is the expected chance of getting a false outcome in general
- So, looking now at everyone who tested positive, which is 997 + 14985 = 15982, the possibility you actually have HIV is 997 / 15982 = 0.062382681 or 6.2%
	- This also means that once you have tested positive and are part of the positive group, the error rate jumps up significantly, all the way to 93.8% due to the low prevalence
- The diagnostic likelihood ratio of a positive test $DLR_+$, is the ratio of the two positive conditional probabilities, one given the presence of disease and the other given the absence. 
- Specifically, $DLR_+ = P(+|D) / P(+|$~$D)$
- Similarly, the $DLR_-$ is defined as the ratio $P(-|D) / P(-|$~$D)$
- Generally speaking both sensitivity and specificity are accuracy rates, that would normally be close to 1, since no one would take an inaccurate test, and their complements, would then be close to 0
- Also note that $P(+|$~$D)$ is the specificity's complement just like $P(-|D)$ is the sensitivity's compliment
## Independent identically distributed
- Random variables are considered to be IID if they are independent, meaning that they are statistically unrelated, and identically distributed, if they've been drawn from the same population distribution
- IID random variables are the default model for random samples, and many statistical theories assume IID variables, so we usually assume our samples are random and variables are IID

PROTON_LOG=1 PROTON_NO_ESYNC=1 PROTON_NO_FSYNC=1 UPLAY_DISABLE_OVERLAY=1 DXVK_ASYNC=0 taskset -c 0-3 %command% 
