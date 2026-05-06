---
tags:
- DS
MOC: Sciences
---
[[_0000 Home|Home]] | [[_0004 Sciences MOC |Back to Sciences MOC]] | [[SC Datascience index|Back to index]]

# What is data science?
- So the first question you probably need answered going into this course is, “What is Data Science?” - and that is a great question. To different people, this means different things, but at its core, data science is using data to answer questions. This is a pretty broad definition, and that’s because it’s a pretty broad field!
- Data science can involve:
	- Statistics, computer science, mathematics
	- Data cleaning and formatting
	- Data visualization
- A data scientist is broadly defined as someone:
	- “who combines the skills of software programmer, statistician and storyteller slash artist to extract the nuggets of gold hidden under mountains of data”
- What is big data?
- There are a few qualities that characterize big data. 
	- **Volume:** As the name implies, big data involves large datasets - and these large datasets are becoming more and more routine. For example, say you had a question about online video - well, YouTube has approximately 300 hours of video uploaded every minute! You would definitely have a lot of data available to you to analyse, but you can see how this might be a difficult problem to wrangle all of that data!
	- **Velocity:** Data is being generated and collected faster than ever before. In our YouTube example, new data is coming at you every minute! In a completely different example, say you have a question about shipping times or routes. Well, most transport trucks have real time GPS data available - you could in real time analyse the trucks movements… if you have the tools and skills to do so!
	- **Variety:** In the examples I’ve mentioned so far, you have different types of data available to you. In the YouTube example, you could be analysing video or audio, which is a very unstructured data set, or you could have a database of video lengths, views or comments, which is a much more structured dataset to analyse.
![[big_data_qualities.png]]
# What is a data scientist?
- So we’ve talked about what data science is and what sorts of data it deals with, but something else we need to discuss is what exactly a data scientist is.
- The most basic of definitions would be that a data scientist is somebody who uses data to answer questions. But more importantly to you, what skills does a data scientist embody?
###### Drew Conway’s Venn diagram of data science
![[data_scientists_qualities.png]]
- Data science is the intersection of three sectors - Substantive expertise, hacking skills, and math and statistics.
- **Substansive Expertise:** We know that we use data science to answer questions - so first, we need to have enough expertise in the area that we want to ask about in order to formulate our questions and to know what sorts of data are appropriate to answer that question.
- **Hacking Skills:** Once we have our question and appropriate data, we know from the sorts of data that data science works with, often times it needs to undergo significant cleaning and formatting - and this often takes computer programming slash “hacking” skills. 
- **Math and Statistics:** Finally, once we have our data, we need to analyse it, and this often takes math and stats knowledge.
## Data science in action!
- One great example of data science in action is from 2009, in which researchers at Google analysed 50 million commonly searched terms over a five year period, and compared them against CDC data on flu outbreaks. Their goal was to see if certain searches coincided with outbreaks of the flu. One of the benefits of data science and using big data is that it can identify correlations; in this case, they identified 45 words that had a strong correlation with the CDC flu outbreak data. With this data, they have been able to predict flu outbreaks based solely off of common Google searches! Without this mass amounts of data, these 45 words could not have been predicted beforehand.

# What is Data?
![[what_is_data.png]]
- Remember that data is secondary in importance to the question you ask
# The Data Science Process
## The Parts of a Data Science Project
- Every Data Science Project starts with a question that is to be answered with data.
- That means that forming the question is an important first step in the process. 
- The second step is finding or generating the data you’re going to use to answer that question. 
- With the question solidified and data in hand, the data are then analyzed, first by exploring the data and then often by modeling the data, which means using some statistical or machine learning techniques to analyze the data and answer your question. 
- After drawing conclusions from this analysis, the project has to be communicated to others. 
- Sometimes this is a report you send to your boss or team at work. Other times it’s a blog post. Often it’s a presentation to a group of colleagues. 
- Regardless, a data science project almost always involves some form of communication of the projects’ findings. We’ll walk through these steps using a data science project example below.
# Types of data analysis
- **Descriptive:** 
	- Goal: Describe or summarize a set of data
	- Early analysis when new data is received
	- Generate simple summaries about the samples and measurements
	- Not for generalizing the results of the analysis to a larger population or making conclusions
- **Exploratory:**
	- Goal: Examine the data and find relationships that weren't previously known
	- Explore how different variables might be related
	- Useful for discovering new connections
	- Help to formulate hypotheses and drive the design of future studies and data collection
	- However, correlation does not imply causation
- **Inferential:**
	- Goal: Use a relatively small sample of data to say something about the population at large
	- Provide your estimate of the variable for the population and provide your uncertainty about your estimate
	- Ability to accurately infer information about the larger population depends heavily on sampling scheme
- **Predictive:**
	- Goal: Use current and historical data to make predictions about future data
	- Accuracy in predictions is dependent on measuring the right variables
	- Many ways to build up prediction models with some being better or worse for specific cases
		- More data and a simple model generally performs well at predicting future outcomes
	- Just because one variable may predict another, it does not mean that one causes the other
- **Causal:**
	- Goal: See what happens to one variable when we manipulate another variable
	- Gold standard in data analysis
	- Often applied to the results of randomized studies that were designed to identify causation
	- Usually analysed in aggregate, and observed relationships are usually average effects
- **Mechanistic:**
	- Goal: Understand the exact changes in variables that lead to exact changes in other variables
	- Applied to simple situations or those that are nicely modeled by deterministic equations
	- Commonly applied to physical or engineering sciences
		- Biological sciences are far too noisy to use mechanistic analysis
	- Often, the only noise in the data is measurement error
# Experimental design
- Before collecting any data, as we established, the first thing you do is
	- Formulate your question
- Then you 
	- Design your experiment, and the best setup possible to gather the data you need
- After then you
	- Identify problems and sources of error in your design
- And finally, you
	- Collect the data
- Doing the wrong analysis, can lead to the wrong conclusion
## Hypothesis
- The hypothesis is what you believe should be the expected outcome of your experiment
- You will also have a variable amount of dependent, and independent variables
- An **independent variable**, or **factor**,  is the variable that you manipulate yourself, as it doesn't depend on other variables being measured
- A **dependent variable** however, is the variable that's expected to change as a result of changing an independent variable
	- These two variable classes are displayed on a cartesian plot, with the independent variables being the "X" and the dependent ones, the "Y"
![[dependent_and_independent_variables.png]]
- An example of a hypothesis with both variable types would be
![[example_hypothesis.png]]
- In the above example, we're making a hypothesis that an increase in shoe size corresponds to an increase in literacy
- In this example we decided to use 100 individuals with varying shoe sizes, to measure their literacy
	- There are techniques for deciding on the suitable sample size for an experiment
### Confounder
- A problem that we identified here, was the possibility of a **confounder**
	- A confounder is an extraneous (unrelated/irrelevant) variable that may affect the relationship between the independent and dependent variables, such as age, which also affect shoe size, and literacy, which means that any relationship we see between shoe size and literacy, could in fact be due to age
	- We can adjust for the counfounder in this case would be to measure each individual's age as well, and measure its effect on literacy, or, fix all the participants ages, such that we eliminate the confounder all together
- In an experiment where we want to measure the effect of an external factor, such as a drug, we normally have a control group that the drug isn't administered to, that we use as a baseline to measure the effect of the drug on the treatment group
	- A confounding effect here can be caused by the fact that the test subjects know which group they belong to (placebo effect)
	- A way to control the confounder here, would be to keep subject blind to which group they're part of
- We don't normally know what variable would be a confounder beforehand, therefore, to avoid having one group be enriched by a confounder, we can rely on randomization, where we randomly assign individuals to each group, which should distribute confounder roughly equally between both groups
### Replication
- An experiment shouldn't be conducted on only one set of subjects, as the results may have occurred by chance
- Instead, a proper experiment should have replicas. Meaning, that we should repeat the experiment with different test subjects
- Replication also allows us to measure variability in the results more accurately, making the differences we see in the data more significant
![[benefit_of_replication.png]]
### P-Value
- The p-value is the probability that a significant result was observed by chance
- Generally, a p-value of 0.05, or 5% is considered significant
- One thing to be aware of, is p-hacking
	- If you were to conduct an experiment 20 times, even if your hypothesis is wrong, there is still a 5% chance you would observe a significant result by chance (the opposite of the p-value). That was called the alpha-value I think
- The idea here is that if look hard enough for a dataset that shows you what you want to see, and do enough experiments to get there, you can trick yourself into finding significance, where there isn't any
- It's actually possible to manipulate data into finding whatever relationship that you want
- It's important to remain objective and unbiased to avoid such a possibility