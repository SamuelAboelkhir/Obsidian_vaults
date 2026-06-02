---
tags:
- DS
MOC: Sciences
---
[[_0000 Home|Home]] | [[_0004 Sciences MOC |Back to Sciences MOC]] | [[SC Datascience index|Back to index]]

# Reproducible research
## What is it?
- The goal of reproducible research is to provide others with the means to reproduce your findings based on the analytical data, and code that you used
- Reproducibility is a middle ground between the golden standard of replicable results, and nothing
## Why do we need it?
- The main point here, is that you have done an analysis on some data, and you want others to be able to take the same data, and nitpick your approach, see if they can see the same conclusion as you or not. Ti's is the scientific way. So you provide your peers and other interested parties with the same set of inputs and tools that you used for them to either reach the same output, or refine it, correcting any mistakes you may have made along the way, and reaching the proper conclusion
- It's a validation of your work in essence, and without this, your work doesn't get as much credibility, since no one can prove your findings are real
## How to create reproducible research?
- The first thing to keep in mind is, you have to script everything, that's the first golden rule of reproducible research
### The steps of data analysis
#### Define the question
- This is the most critical step of any analysis
- Defining a proper question is literally the most powerful method of dimension reduction, as it immediately lets you zoom in on the most relevant variables to what you're trying to answer
- But sometimes you might just want to look at a data set and see what's inside, without having any specific question in mind, you're just exploring. Yet, if you can at least narrow down your interest to a specific question, that will still help you be focused, simplify your problem, and actually reach some kind of meaningful results
- It's important to remember that the proper order of things is that, first, you need to have a specific kind of science that you're pursuing, to which end you get the appropriate data, and then you apply statistical methods to reach some conclusion
	- Going the other way around where you apply statistical methods development to raw data without any sense of science might still lead you to something interesting, but it may not be reproducible, or even meaningful
- So a proper data analysis is based on some scientific context, with some general question being investigated to narrow down the dimensionality of the problem, then we apply statistical methods to the data
#### Define the ideal data set
- Say you want to know if you can detect emails that are spam automatically in your email
- You must make this question concrete for it to be a data analysis problem: "Can I use quantitative characteristics of the emails to classify them as SPAM/HAM?"
- So what would those quantitative characteristics be? and what would the ideal data set for answering this question look like?
- Depending on our goal, and the kind of statement we am to make about the data, the data set can be:
	- **Descriptive:** This means the whole population is used as the data set, no sampling
	- **Exploratory:** A random sample with many variables measured, just for exploring
	- **Inferential:** The right population, randomly sampled. You need to be careful about the sampling mechanism, and the definition of the population that you're sampling from, as an inferential analysis aims to draw conclusions about the greater population from the sample
	- **Predictive:** A training and test data sets from the same population. This is catered more towards machine learning
	- **Causal:** Data from a randomized study. Making a causal statement means that you want to say, that if you modify a specific component, something else happens in the data. So cause and effect, where some study was performed to discover some sort of correlation, and inducing some external stimuli, should cause some sort of change
	- **Mechanistic:** Data about all components of the system that we're trying to describe
- When it comes to spam, we know all the emails we can need are on google's data centers
#### Determine what data you can access
- Obviously though, we can't access all the emails on google's data centers, so we need an alternative
- Our options here could be to look for some freely accessible data on the internet
- Maybe we can buy data from a provider, making sure to respect the terms of use of said data
- If the data just doesn't exist, here we may have to generate it ourselves somehow
- In the case of spam emails, there are actually spam datasets out there that we can use
#### Obtain the data
- So first, we can try to obtain the raw data, and we must always **reference** the source of where it came from
- Polite emails go a long way in case we need to obtain the data from an online source, who has the data and never made it public
- Always record the URL and time of access, as you will only have a snapshot of the data at a specific point in time, after which, the data set is subject to change, or it may not be available all together
#### Clean the data
- Now, following the steps from [[SC Getting and Cleaning Data]], the next step would be to tidy up our data, processing the raw set into a tidy set
- Sometimes the data can be pre-processed, in which case we need to know how it was processed, and where it even comes from
- If the data comes from a survey for example, then how was the sampling done? Is it from an observational study? maybe from some experiment?
- The data may still need to be reformatted depending on how we want to model it, and the sorts of analysis we want to perform, and to just make it more manageable
- All of these pre-processing steps, again, must be documented in a script, that can be re-run by us or anyone else
- Sometimes, the data may not be good enough to answer our question, in which case, either new data is required, or maybe the question needs to be changed, but regardless, you just don't carry on with unsuitable data, as it will lead to wrong results
- Here is the SPAM dataset from the `kernlab` package
```R
library(kernlab)
data(spam)
str(spam[, 1:5])
'data.frame':	4601 obs. of  5 variables:
 $ make   : num  0 0.21 0.06 0 0 0 0 0 0.15 0.06 ...
 $ address: num  0.64 0.28 0 0 0 0 0 0 0 0.12 ...
 $ all    : num  0.64 0.5 0.71 0 0 0 0 0 0.46 0.77 ...
 $ num3d  : num  0 0 0 0 0 0 0 0 0 0 ...
 $ our    : num  0.32 0.14 1.23 0.63 0.63 1.85 1.92 1.88 0.61 0.19 ...
```
#### Exploratory data analysis
- So, the type of analysis we wanna make here, is a predictive one, where we will split the data into a training set and a test set, and see if we can predict if an email is spam or not based on its characteristics
- So in the first part of test we will build our model, and then we will use another part of the set, that's independent of the first, to test if our model is actually good at predicting spam
- To do so, we will take half of the data, to act as our training set, and the rest will be the test set
```R
set.seed(3435)
trainIndicator = rbinom(4601, size = 1, prob = 0.5)
table(trainIndicator)
trainIndicator
   0    1 
2314 2287
trainSpam = spam[trainIndicator == 1,]
testSpam = spam[trainIndicator == 0,]
```
- Now lets explore the data, see if there are NAs, some summaries, create some plots, and do some clustering maybe
```R
names(trainSpam)
 [1] "make"              "address"           "all"               "num3d"            
 [5] "our"               "over"              "remove"            "internet"         
 [9] "order"             "mail"              "receive"           "will"             
[13] "people"            "report"            "addresses"         "free"             
[17] "business"          "email"             "you"               "credit"           
[21] "your"              "font"              "num000"            "money"            
[25] "hp"                "hpl"               "george"            "num650"           
[29] "lab"               "labs"              "telnet"            "num857"           
[33] "data"              "num415"            "num85"             "technology"       
[37] "num1999"           "parts"             "pm"                "direct"           
[41] "cs"                "meeting"           "original"          "project"          
[45] "re"                "edu"               "table"             "conference"       
[49] "charSemicolon"     "charRoundbracket"  "charSquarebracket" "charExclamation"  
[53] "charDollar"        "charHash"          "capitalAve"        "capitalLong"      
[57] "capitalTotal"      "type"
head(trainSpam)
   make address  all num3d  our over remove internet order mail receive will people
1  0.00    0.64 0.64     0 0.32 0.00   0.00        0  0.00 0.00    0.00 0.64   0.00
7  0.00    0.00 0.00     0 1.92 0.00   0.00        0  0.00 0.64    0.96 1.28   0.00
9  0.15    0.00 0.46     0 0.61 0.00   0.30        0  0.92 0.76    0.76 0.92   0.00
12 0.00    0.00 0.25     0 0.38 0.25   0.25        0  0.00 0.00    0.12 0.12   0.12
14 0.00    0.00 0.00     0 0.90 0.00   0.90        0  0.00 0.90    0.90 0.00   0.90
16 0.00    0.42 0.42     0 1.27 0.00   0.42        0  0.00 1.27    0.00 0.00   0.00
```
- So looking at the columns and first few rows, we can see that the set's variables are a bunch of names, and rows have counts, so this seems to be the frequency of each of these words in the emails
```R
table(trainSpam$type)
nonspam    spam 
   1381     906
```
- The training set also has 1381 nonspam and 906 spam emails
- Now lets look at some different characteristics between spam and nonspam
```R
plot(trainSpam$capitalAve ~ trainSpam$type)
```
![[Pasted image 20260517230439.png]]
- So this variable `capitalAve` is the frequency of capital letters in an email, and we see that spam emails tend have more of them, but the data is so skewed in either case, that plot isn't very visible
- In these cases, taking the log of the data to reduce the scale of the numbers a bit helps group the data points a bit more making the plots more descriptive
- Also, as the data has a bunch of zeroes, we will be incrementing all the values by 1. Although, this is only fine in this case since we're exploring the data, normally we wouldn't want to just go increment zeroes like that
```R
plot(log10(trainSpam$capitalAve + 1) ~ trainSpam$type)
```
![[Pasted image 20260517230941.png]]
- Much better, now we can actually see the medians and inner quartiles of the boxplots
- Next, lets look at the relationships between our predictors, using a pairwise plot
```R
plot(log10(trainSpam[, 1:4]+1))
```
![[Pasted image 20260517231544.png]]
- So we see that some of the predictors are correlated, but not all of them
- Now lets try to cluster the data
```R
hCluster = hclust(dist((t(trainSpam[, 1:57]))))
plot(hCluster)
```
![[Pasted image 20260517232217.png]]
- The cluster wasn't really of much help on the first attempt, although it did separate this `capitalTotal` predictor, but clustering algorithms are sensitive to skweness, so we will try again after we have done some transformations to the predictor space
- Particularly, we will use our favorite transformation for removing skews, which is taking the log again and adding +1 to avoid trying to log any zeroes
```R
hClusterUpdated = hclust(dist((t(log10(trainSpam[, 1:57] + 1)))))
```
![[Pasted image 20260517232642.png]]
- Now we have more clusters to work with. `capitaleAve` is a cluster all on its own now, and there is another cluster with the words `you` `your` and `will` and a bunch other clusters with words ambiguously clumping together
#### Statistical prediction/modeling
- Now the new part, statistical modeling
- A statistical model should be informed by the results of the exploratory analysis
- The exact methods to use depend on the question of interest, so again, the right question is the corner stone of the analysis
- While doing statistical analysis, always take into account the fact that the data has been processed or transformed, if they have been
- The sources of uncertainty and the measure of uncertainty should also be reported
- As for our model
```R
trainSpam$numType = as.numeric(trainSpam$type) - 1
costFunction = function(x, y) sum(x != (y > 0.5))
cvError = rep(NA, 55)
library(55)
library(boot)
for (i in 1:55) {
    lmFormula = reformulate(names(trainSpam)[i], response = "numType")
    glmFit = glm(lmFormula, family = "binomial", data = trainSpam)
    cvError[i] = cv.glm(trainSpam, glmFit, costFunction, 2)$delta[2]
}

## Which predictor has minimum cross-validated error?
names(trainSpam)[which.min(cvError)]
[1] "charDollar"
```
- In this model, we go over each variable of the data set, and we try to fit a generalized linear model, which is a logistic regression in this case, to see if we can predict whether or not an email is spam using just a single variable
- We used the `reformulate` function to create the formula, where the response is the type of email, with one variable from the dataset at a time
- We cycled through them using a for-loop building a linear regression model
- We then calculated the cross-validated error rate of predicting spam emails from a single variable
- The cross-validated error rates were stored in this `cvError` variable which we checked for the smallest value, and it turns out it was under the variable `charDollar` which indicates the number of dollar signs in the email
- We still need to measure our uncertainty though, so we will take the best model out of our set of 55 predictors, and we will refit the model, giving us a valid linear regression model that we can use to make predictions
- Due to the fact that linear regression models don't give binary predictions, but rather a probability that an email is spam or not, we need to also take our continuous probability, ranging between 0 and 1, and determine at what point do we consider an email spam
- Lets use a cutoff of 0.5 or 50% for this example, meaning that if the probability is above 50%, the email is considered spam
```R
## Use the best model from the group
predictionModel = glm(numType ~ charDollar, family = "binomial", data = trainSpam)

## Get predictions on the test set
predictionTest = predict(predictionModel, testSpam)
predictedSpam = rep("nonspam", dim(testSpam)[1])

## Classify as `spam` for those with prob > 0.5
predictedSpam[predictionModel$fitted > 0.5] = "spam"
```
- Now lets see how our model did, by comparing our predictions to the real types
```R
table(predictedSpam, testSpam$type)
             
predictedSpam nonspam spam
      nonspam    1346  458
      spam         61  449
      
## Error rate
(61 + 458)/(1346 + 458 + 61 + 449)
[1] 0.2242869
```
- From this classification table we can see that we misclassified 519 emails, with an error rate of ~22%, since we classified 61 emails as nonspam, although they were spam, and 458 emails as spam, while they were not spam
#### Interpret results
- It's time to interpret our analysis, and an important thing to take care of here, is the use of appropriate language, that doesn't go beyond what the analysis actually shows
- This means that we want to use words correlated with the type of analysis that we did, such as:
	- describes
	- correlates with/associated with
	- leads to/causes
	- predicts
- Basically, we just need to think carefully about our choice of words, and make sure it gives the correct interpretation as to what our findings and conclusions actually are
- It's good to give an explanation of our choices, such as why we believe certain models predict better than others in this case, and if there are any coefficients that need interpretation, we should explain them
- It's also very useful to bring in the measures of uncertainty, as a way to calibrate our interpretations of the final results
- For example, in our small analysis, we believe that the fraction of characters that are dollar signs can be used to predict if an email is spam
- We may as well have decided that any email with more than 6.6% dollar signs should be classified as spam, since our prediction says that more dollar signs always means more spam
#### Challenge results
- Now, before allowing other people to criticize our work, we should be the first ones to challenge our own interpretations of the data
- We need to question all the steps that we took, starting from the initial question itself, all the way to the conclusions
- Maybe the question wasn't a valid one to begin with, or maybe the data source wasn't suitable, or the processing was wrong in some way
- Are the measures of uncertainty appropriate?
- Why is this model the best one for this problem?
- How did we choose the predictors to include in the model?
- Are there alternative analyses that we could have performed?
	- We don't have to go ahead and perform the alternative analyses, but it may be useful to at least mention them as possibilities
#### Synthesize/write up results
- Now we need to write the results
- The final report's structure is important, as it needs to be able to tell a coherent story for the reader
- We normally want to lead with the question, as it lays down the framework of the whole analysis, and if the readers understand the question, they can start understanding the context, and the manner in which we operated
- Only include the analyses that are relevant to the story, and were needed to address a challenge
	- However, do keep the analyses readily available as someone may challenge you asking why you haven't performed a certain analysis, in which case you can show him that you did, but it wasn't really relevant, or was problematic for some reason
- Order the analyses according to the narrative of the story, not necessarily chronologically
	- That's because the chronological order isn't necessarily coherent, and may be quite scattered, hurting the narrative and the reader's ability to follow along
- And of course, include pretty figures that helps the reader visualize the data
	- Not the quick ones done during exploration, but rather well made aesthetically appealing, and descriptive ones
#### Create reproducible code
- Now, it's finally time to create the actual reproducible code
- Ideally, this is something you make as you're performing the analysis, and documenting what you're doing along the way
- Tools like markdown (obviously) and knitter are very good for this purpose
- They also help you preserve the code and any written summaries that you have in a single document
- The creation of this document as you go is how you make sure that your analysis is reproducible, as everything you did step by step from start to finish with the code you used to do it is now documented, and Rmarkdown using knitter even allows you to run the code and see the results within the document, so you don't have to document the result itself, just the code needed to produce it
- This is the standard for big data analysis
## Organizing a data analysis
- The key files/folders in a data analysis
	- Data
		- Raw: can be added to git, but if it's too big then can skip
		- Processed: should be named according to the script that generated it
	- Figures
		- Exploratory: no need for these to be pretty, they don't make it to the final report
		- Final: prettified versions of the relevant exploratory figures, maybe new ones too. Need to be descriptive and clear
	- R code
		- Raw/unused scripts: like the figures, no need for these to be super clean, but cleanliness still helps
		- Final: commented, clean, clear, well explained, meant to be read by others
		- R Markdown: optional, great for generating reproducible reports, code snippits can be embedded in them, then knitted into PDF, HTML, etc. Part of the concept of "literate programming"
	- Text
		- README: not needed if an R Markdown, or some equivalent file exists
		- Text of analysis/report: basically a scientific paper or summary. Not much explanation needed, this is literally a scientific paper, like a review paper or a research paper, and should be structured following those sorts of principles
## Communicating results
- People are busy, especially managers and decision makers
- You should break down the results of an analysis by levels of granularity
- You shouldn't bombard people with too much information, focus on a summary of the main findings, and attach a more detailed analysis with your results
- The report should be broken up like a paper
	- Title
	- Abstract
	- Body
	- Supplementary materials / The gory details
	- Code / Data
- If you're sending an email, then in the body:
	- The body should include a brief description of the problem
	- Context as to what was discussed as a reminder
	- A call to action if action is needed, with suggested options
	- If questions need answering, try to make them yes or no questions
- The email can also have attachments
	- Code
	- Figures
	- Don't overdo it though
- Links to supplementary materials
	- More code, software, data
	- A github repo or project website
## Reproducible research checklist
+ Are we doing good science?
+ Was any part of this analysis done by hand?
- If so, are those parts precisely document?
- Does the documentation match reality?
+ Have we taught a computer to do as much as possible (i.e. coded)?
+ Are we using a version control system?
+ Have we documented our software environment?
+ Have we saved any output that we cannot reconstruct from original data + code?
+ How far back in the analysis pipeline can we go before our results are no longer (automatically) reproducible?
## Replication and Reproducibility
### Replication
+ Focuses on the validity of the scientific claim
+ "Is this claim true?”
	+ The claim may be true if we can replicate it with different investigators, lab equipment, who are all collecting new data, that reaches the same conclusion
+ The ultimate standard for strengthening scientific evidence
+ New investigators, data, analytical methods, laboratories, instruments, etc.
+ Particularly important in studies that can impact broad policy or regulatory decisions
### Reproducibility
+ Focuses on the validity of the data analysis
+ "Can we trust this analysis?"
+ Arguably a minimum standard for any scientific study
+ New investigators, same data, same methods
+ Important when replication is impossible