---
tags:
- DS
- SC
MOC: Technology
---
[[_0000 Home|Home]] | [[_0004 Sciences MOC |Back to Sciences MOC]] | [[SC Datascience index|Back to index]]
# What you should have
- You should have 4 things when you finish tidying the data
	- The raw data
	- A tidy data set
	- A code book describing each variable and its value in the tidy data set
	- An explicit and exact recipe you used to go from step 1 till the end
# Attributes of raw data
- The rawest possible form of the data
- No software was ran on the data
- No manipulation was done on the numbers in the set
- Nothing is summarized
# Attribute of tidy data
- One variable per column
- Each observation in its own row
- One table per type of variable
- Links between multiple tables
- Variable names should be mentioned in a row at the top of the file
- Human readable names
- Saved as one file per table
# The code book
- Includes info about the variables
- Info about the summary of choices made
- Info about the experimental study design that was used
- Normally a word or text file
- Should have a section called "study design" where you explain how the data was collected
- There should be a section called code book that describes each variable and its units
# The instructions list
- Ideally a script in a programming language
- Takes the raw data as input
- Outputs the tidy data
- Doesn't require parameters
- In case running the script as is isn't possible, provide usage instructions