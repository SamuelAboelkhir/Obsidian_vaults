---
tags:
- Programming-Language
- R
- PG
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[PG R index|Back to index]]
# Reading files
- You can read files via a variety of commands, but the most common is `read.table([filePath], sep=[separator], header= TRUE)`
- Read files are stored in RAM
- You can tell R how to handle quotes with `quote=`
	- `quote=""` means to remove quotes
- You can tell R what value to give NA values with `na.strings`
- You can determine how many rows you want to read with `nrows`
- You can skip a number of lines before starting to read with `skip`
# data.table
- The successor to `data.frame`
- Also references rows then columns in order `DT[row,column]`
- All functions that deal with data.frame can deal with data.table
- Much faster since it's written in C
- Creating a data.table is similar to a data.frame
	- data.frame: `DF = data.frame(x=rnorm(9), y=rep(c("a","b","c"), each=3), z=rnorm(9))`
	- data.table: `DT = data.table(x=rnorm(9), y=rep(c("a","b","c"), each=3), z=rnorm(9))`
- Subsetting
	- Subset rows
		- `DT[2,`: outputs row 2
		- `DT[DT$y="a",]`: Accesses the y column and returns all rows where y = "a"
		- `DT[c(2,3)]`: Returns the 2nd and 3rd rows
		- `DT[,c(2,3)]`: Can't use the same logic to subset columns
			- You can instead path a list of functions to perform on the columns by name
			- `DT[,list(mean(x), sum(z))]`
- You can new columns with `DT[,[columnName]:=[value]]`