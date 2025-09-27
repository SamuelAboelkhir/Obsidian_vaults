---
tags:
- DS
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[DS Datascience index|Back to index]]
- Acquired from [https://www.datacamp.com/cheat-sheet/introduction-to-probability-](https://www.datacamp.com/cheat-sheet/introduction-to-probability-)
![](https://media.datacamp.com/cms/29e70f8c7ee70f086f40a3eadbd3ad66.png)

Have this cheat sheet at your fingertips

[Download PDF](https://images.datacamp.com/image/upload/v1680264475/Marketing/Blog/Probability_Cheat_Sheet.pdf)

## Definitions

The following pieces of jargon frequently occur when discussing probability.

- **Event**: A thing that you can observe whether it happens or not.
- **Probability**: The chance that an event happens, on a scale from 0 (cannot happen) to 1 (always happens). Denoted P(event).
- **Probability universe**: The probability space where all the events you are considering can either happen or not happen.
- **Mutually exclusive events: If one event happens, then the other event cannot happen (e.g., you cannot roll a dice that is both 5 and 1).**
- **Independent events**: If one event happens, it does not affect the probability that the other event happens (e.g., the weather does not affect the outcome of a dice roll).
- **Dependent events**: If one event happens, it changes the probability that the other event happens (e.g., the weather affects traffic outcomes).
- **Conjunctive probability** (a.k.a. **joint probability**): The probability that all events happen.
- **Disjunctive probability**: The probability that at least one event happens.
- **Conditional probability**: The probability that one event happens, given another event happened.

## Complement Rule: Probability of events not happening

- **Definition:** The complement of A is the probability that event A does not happen. It is denoted A' or A c.
- **Formula:** P(A') = 1 - P(A)
- **Example:** The probability of basketball player Stephen Curry successfully shooting a three-pointer is 0.43. The complement, the probability that he misses, is 1 - 0.43 = 0.57.

## Odds: Probability of the event happening versus not happening

- Definition: The odds of event A happening is the probability that the event happens divided by the probability that the event doesn't happen.
- Formula: Odds(A) = P(A) / P(A') = P(A) / 1 - P(A)
- Example: The odds of basketball player Stephen Curry successfully shooting a three-pointer is the probability that he scores divided by the probability that he misses, 0.43 / 0.57 = 0.75.

## Multiplication Rules: Probability of both events happening

### Mutually exclusive events

- **Definition:** The probability of two mutually exclusive events happening is zero.
- **Formula:** P(A ∩ B) = 0
- **Example:** If the probability of it being sunny at midday is 0.3 and the probability of it raining at midday is 0.4, the probability of it being sunny and rainy is 0, since these events are mutually exclusive.

### Independent events

- **Definition:** The probability of two independent events happening is the product of the probabilities of each event.
- **Formula:** P(A ∩ B) = P(A) \* P(B)
- **Example:** If the probability of it being sunny at midday is 0.3 and the probability of your favorite soccer team winning their game today is 0.6, the then probability of it being sunny at midday and your favorite soccer team winning their game today is 0.3 \* 0.6 = 0.18.

### The conjunctive fallacy

- **Definition:** The probability of both events happening is always less than or equal to the probability of one event happening. That is P(A ∩ B) <= P(A), and P(A ∩ B) <= P(B). The conjunctive fallacy is when you don't think carefully about probabilities and estimate that probability of both events happening is greater than the probability of one of the events.
- **Example:** A famous example known as 'The Linda problem" comes from a 1980s research experiment. A fictional person was described:

*Linda is 31 years old, single, outspoken, and very bright. She majored in philosophy. As a student, she was deeply concerned with issues of discrimination and social justice and also participated in anti-nuclear demonstrations.*

Participants had to choose which statement had a higher probability of being true:  

1. Linda is a bank teller.
2. Linda is a bank teller and is active in the feminist movement.

Many participants chose fell for the conjunctive fallacy and chose option 2, even though it must be less likely than option 1 using the multiplication rule.

## Addition Rules: Probability of at least one event happening

### Mutually exclusive events

- **Definition:** The probability of at least one mutually exclusive event happening is the sum of the probabilities of each event happening.
- **Formula:** P(A ∪ B) = P(A) + P(B)
- **Example:** If the probability of it being sunny at midday is 0.3 and the probability of it raining at midday is 0.4, the probability of it being sunny or rainy is 0.3 + 0.4 = 0.7, since these events are mutually exclusive.

### Independent events

- **Definition:** The probability of at least one mutually exclusive event happening is the sum of the probabilities of each event happening minus the probability of both events happening.
- **Formula:** P(A ∪ B) = P(A) + P(B) - P(A ∩ B)
- **Example:** If the probability of it being sunny at midday is 0.3 and the probability of your favorite soccer team winning their game today is 0.6, the then probability of it being sunny at midday or your favorite soccer team winning their game today is 0.3 + 0.6 - (0.3 \* 0.6) = 0.72.

### The disjunctive fallacy

- **Definition:** The probability of at least one event happening is always greater than or equal to the probability of one event happening. That is P(A ∪ B) >= P(A), and P(A ∪ B) >= P(B). The disjunctive fallacy is when you don't think carefully about probabilities and estimate that the probability of at least one event happening is less than the probability of one of the events.
- **Example:** Returning to the "Linda problem," consider having to rank these two statements in order of probability:
1. Linda is a bank teller.
2. Linda is a bank teller and is active in the feminist movement.

The disjunctive fallacy would be to think that choice 1 had a higher probability of being true, even though that is impossible because of the additive rule of probabilities.

## Bayes Rule: Probability of an event happening given another event happened

- **Definition:** For dependent events, the probability of event B happening, given that event A happened, is equal to the probability that both events happen divided by the probability that event A happens. Equivalently, it is equal to the probability that event A happens given that event B happened times the probability of event B happening divided by the probability that event A happens.
- **Formula:** P(B|A) = P(A ∩ B) / P(A) = P(A|B) \* P(B) / P(A)
- **Example:** Suppose it's a cloudy morning, and you want to know the probability of rain today. If the probability of it raining that day given a cloudy morning is 0.6, and the probability of it raining on any day is 0.1. The probability of it being cloudy on any day is 0.3, then the probability of it raining, given a cloudy morning, is 0.6 \* 0.1 / 0.3 = 0.2. That is, if you have a cloudy morning, it is twice as likely to rain than if you didn't have a cloudy morning, due to the dependence of the events.

Topics

Related

![Excel Cheat Sheet.png](https://media.datacamp.com/legacy/v1674225279/Excel_Cheat_Sheet_218f6c7f57.png?w=750)Excel Formulas Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/getting-started-with-excel-cheat-sheet)

Learn the basics of Excel with our quick and easy cheat sheet. Have the basics of formulas, operators, math functions and more at your fingertips.

Richie Cotton

![](https://media.datacamp.com/cms/descriptive-statistics---updated.png?w=750)Descriptive Statistics Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/descriptive-statistics-cheat-sheet)

In this descriptive statistics cheat sheet, you'll learn about the most common statistical techniques for descriptive analytics.

Richie Cotton

Python for Data Science - A Cheat Sheet for Beginners

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/python-for-data-science-a-cheat-sheet-for-beginners)

This handy one-page reference presents the Python basics that you need to do data science

Karlijn Willems

Probability Distributions in Python Tutorial

Tutorial

[View original](https://www.datacamp.com/tutorial/probability-distributions-python)

In this tutorial, you'll learn about and how to code in Python the probability distributions commonly referenced in machine learning literature.

DataCamp Team

Poker Probability and Statistics with Python

Tutorial

[View original](https://www.datacamp.com/tutorial/statistics-python-tutorial-probability-1)

Tackle probability and statistics in Python: learn more about combinations and permutations, dependent and independent events, and expected value.

Daniel Poston

![](https://media.datacamp.com/legacy/v1694029223/image_16508ea001.jpg?w=750)Priceless Probability Puzzles in Python

code-along

[View original](https://www.datacamp.com/code-along/priceless-probability-puzzles-python)

Use Python to tackle some logic and probability puzzles

Justin Saddlemyer