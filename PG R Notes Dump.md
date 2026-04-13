---
tags:
- R
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG R index|Back to index]]

# R packages
- For  R, there are two main repositories for packages
	- [CRAN (Comprehensive R Archive Network)](https://cran.r-project.org/web/packages/): R’s main repository (>12,100 packages available!) 
	- [BioConductor](https://bioconductor.org/packages/release/BiocViews.html#___Software): A repository mainly for bioinformatic-focused packages
## Finding the right pacakge
- CRAN offers 35 different topic, called themes, that we can explore in its [Task view](https://cran.r-project.org/web/views/)
- We can also look for packages on [RDocumentation](https://www.rdocumentation.org
## Installation
- To install a package, we use the `install.packages()` function with the name of the package as a string being the parameter
	- We can also install multiple packages by using an array (R's character vector)
- For bioconductor, we use `BioCManager::install()`, there used to be `biocLite` too, but that's depricated
- To install from github, devtools are required, so first `install.packages("devtools")`, then we can `install_github("author/package")`
- Installed packages can be checked using `installed.packages()` or `library()`
## Loading/unloading and updating
- Once done with the installation, a package must also be loaded using the `library` command, `library(ggplot2)`
	- You can also use a function from a package without needing to load it via `package::function`
- To update packages we have `old.packages()`, to check which packages are outdated, and `update.packages()`, to update them
- We can also just `install.packages("packagename")` which would update the package if it's already there
- To unload a package, simply use `detach("package:ggplot2", unload=TRUE)`
## Remove package
- Remove a package with `remove.packages()`
## Version and sessionInfo
- You can also see the R version and session info with `version`, `sessionInfo()`
	- Knowing this is useful for many things, including the fact that some packages may require specific R versions
## Help and vignettes
- You can see the R help menu on a package in 2 ways
	- The `help()` function
	- Typing `?`in front of the function name
	- Typing `??` shows all help pages related to a packge/function
- Some packages also have vignettes, which are extended help files, kinda like a wiki for the a package and its functions, complete with examples too (although the help menu for most packages and functions is already kinda like that and is very comprehensive)
	- Vignettes open html files in the browser
- To find all the available vignettes `browseVignettes()` or for a specific package `browseVignettes("ggplot2")`
# R basics
- R is an interpreted language that's based on the S language
- Being interpreted, it shares some similarities with python, like being able to print a variable by just typing it
```R
> x = 10
> x
[1] 10
```
- An integer sequence in R (basically a range) can be very easily created by
```R
> x = 1:15
> x
[1] 1 2 3 4 5 6 7 8 9 10
[11] 11 12 13 14 15
```
- This gave us an integer vector (R's vectors are single type arrays)
## Objects
- R has 5 basic object classes
	- Char
	- numeric
	- int
	- complex
	- logical (basically boolean)
- R also has vectors, which can only contain objects of the same class, and lists, which support objects of different classes
- We can create an empty vector with the `vector()` function
### Numbers
- By default, any defined number in R will be numeric (double precision real number). To get an integer instead, you define the number with an `L` suffix `5L`
- We can represent infinity with `Inf`
- R also has `NaN` for not a number
### Attributes
- R objects can have attributes, but they don't always have them
	- names, dimnames
	- dimensions (e.g. matrices, arrays)
	- class
	- length
	- other user-defined attributes/metadata
- These attributes are accessible and settable/modifiable using `attributes()`
### Vectors
- Created with `vector()` or `c()`
```R
> x = c(0.5, 0.6) ## numeric
> x = c(TRUE, FALSE) ## logical
> x = c(T, F) ## logical
> x = c("a", "b", "c") ## character
> x = 9:29 ## integer
> x = c(1+0i, 2+4i) ## complex

## or

> x = vector("numeric", length = 10) ## creates a vector of the default 0 value
```
- Vectors support one type at a time, but you can still try to create a vector of mixed objects. R will coerce the objects to be of the same type
	- The coercion is based on which type is the least common denominator, meaning, the one that can't be converted to as many other types
```R
> y = c(1.7, "a") ## character
> y = c(TRUE, 2) ## numeric
> y = c("a", TRUE) ## character
```
- You can also do explicit coercion (casting in other languages)
```R
> x = 0:6 ## that's an int vector
> class(x)
[1] "integer"
> as.numeric(x) ## now it's numeric
```
- A nonsensical coercion will result in NAs though
```R
> x = c("a", "b", "c")
> as.numeric(x)
[1] NA NA NA
```
### Lists
- These are kinda like a javascript object with how they print out
```R
> x = list(1, "a", TRUE, 1+4i)
> x
[[1]]
[1] 1
[[2]]
[1] "a"
[[3]]
[1] TRUE
[[4]]
[1] 1+4i
```
### Matrices
- These are vectors, with a dimension attribute
- The dimension attribute is itself an integer vector of length two, where the first number is the number of rows, and 2nd number for the columns
- To create a matrix
#### matrix()
```R
> m = matrix(nrow = 2, ncol = 3)
> m
	 [,1] [,2] [,3]
[1,]  NA   NA   NA
[2,]  NA   NA   NA
> dim(m)
[1] 2 3
> attributes(m)
$dim
[1] 2 3
```
- `dim()` shows the dimensions of the matrix, and `attributes()` shows the attributes
- Matrices are created column-wise, and creating a matrix using a column vector will fill it column-wise first
```R
> m = matrix(1:6, nrow = 2, ncol = 3)
> m
	 [,1] [,2] [,3]
[1,]   1    3    5
[2,]   2    4    6
```
#### Create matrix through vector
- Another way to create a matrix is to create a vector first, then give it a dimension attribute
```R
> m = 1:10
> dim(m) = c(2, 5)
> m
> m
	 [,1] [,2] [,3] [,4] [,5]
[1,]   1    3    5    7    9
[2,]   2    4    6    8   10
```
#### Create matrix via cbind and and rbind
- Finally, we can create a matrix using the `cbind()` (column bind) and `rbind()` (row bind) functions
```R
> x = 1:3
> y = 10:12
> cbind(x, y)
	 x   y
[1,] 1  10
[2,] 2  11
[3,] 3  12

> rbind(x, y)
	[,1] [,2] [,3]
x     1    2    3
y    10   11   12
```
- Each one of these function creates a new matrix, where `cbind()` goes column first, and `rbind()` goes row first
### Factors
- A factor is yet another type of vector, that's used to represent categorical data
- They can be ordered or unordered
- It's like an int vector where each int is labelled
- Factors are treated specially by modelling functions such as `lm()` (linear model) and `glm()` (generalized linear model)
- Personal though: Factors, or at least ordered factors are basically enums. It's a sequential int array where each int is labelled, like 1 = low, 2 = medium, 3 = high
#### Creation
- To create a factor
```R
> x = factor(c("yes", "yes", "no", "yes", "no"))
> x
[1] yes yes no yes no
Levels: no yes
> table(x)
x
no yes
 2   3
 > unclass(x)
[1] 2 2 1 2 1
attr(,"levels")
[1] "no" "yes"
```
- The `table()` function in the above example gave a frequency count for the levels
- `unclass()` strips the class from a vector, returning it in its most basic form (an int vector in this case)
	- `unclass()` also called `attr()` on its argument `x` using its only attribute
	- It seems `unclass()` only works if the object has 1 class, as it didn't work on another matrix object, which has 2 classes (matrix and array), and instead just printed the matrix itself
- `attr()` is similar to `attributes()` but the difference is that `attributes()` deals with the full list of attributes for an object, while `attr()` returns or sets a specific attribute
#### Setting the levels
- The order of the factor's levels can be set as follows
```R
> x = factor(c("yes", "yes", "no", "yes", "no"), levels = c("yes", "no"))
> x
[1] yes yes no yes no
Levels: yes no
```
- The baseline level is the first level in the factor, which is determined by alphabetical order if we leave it up to R to decide
- Determining the correct baseline level is important as it affect modeling functions
### NA
- `is.na()` and `is.nan()` can be used to figured out if an object has NA or NAN values
- NAN is a type of NA, and NA values actually have a class, so there is a distinction between int NA, char NA and so on
- Calling `is.na()` on a vector for example would return a logical vector (trues and falses) indicating for each value of the vector if it's NA or not
### Data Frames
- These are used to store tabular data
- It's a special type of list (not vector) where every element has the same length
- That's because we think of each element as a column, and length as the number of rows under said column
- Since it's a type of list, data frames (DFs) can store objects of variable classes, unlike matrices which must store objects of the same class
- They also have a special attribute called `row.names`
	- These are useful for annotating the data (it's basically IDs, like primary keys)
#### Creation
- To create a data frame, you normally use `read.table()` or `read.csv()`, which will read data into a data frame
- They can also be converted to a matrix using `data.matrix()`
	- This will coerce objects to be of the same type
```R
> x = data.frame(foo = 1:4, bar = c(T, T, F, F))
> x
	foo   bar
1     1  TRUE
2     2  TRUE
3     3  FALSE
4     4  FALSE
> nrow(x)
[1] 4
> ncol(x)
[1] 2
```
### Names
- R objects can have names
- For example a vector `x = 1:3` doesn't have a name, if we call `names(x)` we will get a `NULL`, however, we can give it names `names(x) = c("foo", "bar", "norf")`, then `names(x)` will be `"foo"  "bar"  "norf"`
```R
# printing the named x
> x
foo bar norf
  1  2     3
```
- For matrices, the names are called dimnames, for the dimensions `dimnames(m) = list(c("a", "b"), c("c", "d"))`
## Reading and Writing Data
### Read
- We already mentioned `read.table()` and `read.csv()`
	- Takes a file name or connection
	- Header, just tells R if the file has a header
	- Sep, the how are columns separated
	- colClasses, a char vector to indicate what the class of each column is
	- nrows, optional, for the number of rows
	- comment.char, a char to indicate to R what char indicates a comment in the file to read
	- skip, how many lines to skip from the beginning
	- stringsAsFactors, whether or not to code character variables as factors, defaults to true
	- R automatically does most of these things, like skiping lines that start with `#` or figuring out the number of rows in the file, the type of variable in each column, and so on, but telling R these things in advance makes it faster and more efficient
	- `read.csv()` is the same as `read.table()` but its default separator is `,` is all. It also always specifies header to be true
- We also have `readLines()` which returns a char vector from any type of file
- `source()` reads code in code files (it has an inverse function called `dump()`)
- `dget()` similar to `source()` but it reads objects from R files (its inverse is `dput()`)
- `load()` reads in saved workspaces (which are saved in binary)
- `unserialize()` reads single R objects that in binary form
### Write
- The inverse of the above functions are:
	- `write.table()`
	- `writeLines()`
	- `dump()`
	- `dput()`
	- `save()`
	- `serialize()`
### Reading larger datasets
- Since R doesn't use virtual memory, you should calculate in advance the amount of required RAM for loading a dataset
- If you don't have comments in the file, set `comment.char = ""`
- Apparently specifying colClasses in advance also makes R run way faster, often twice as fast
- To figure out your col classes, if you don't already know them
```R
> initial = read.table("datatable.txt", nrows = 100)
> classes = sapply(initial, class)
> tabAll = read.table("datatable.txt", colClasses = classes)
```
- Setting nrows in advance doesn't help R run much faster, but it does help with memory usage (to know that, maybe use `wc` on the file first, unix rules)
#### Calculating Memory Requirements
- As an example, we assume a data frame that will have 1,500,000 rows, and 120 columns, which are all numeric
	- Thanks to C we know that on a 64-bit system an int is 8 bytes, so that's 1,500,000 * 120 * 8 = 1,440,000,00
	- Dividing by 2^20 we get 1,373.29 MB, which is roughly 1.34 GB
### Textual Formats
- dumping and dputing output an editable textual format that is potentially recoverable in case of corruption
- They also save metadata about the classes of the objects in a data frame unlike normal writing functions, which means we don't have to specify this data again when using reading functions
- They work better with version control
- They do however take more space to store
```R
> y = data.frame(a=1, b="a")
> dput(y)
structure(list(a = 1, b = "a"), class = "data.frame", row.names = c(NA, 
-1L))
```
### Connections
- A connection interface is how R connects to the outside world
- Functions like `read.table()` rely on those connections behind the scenes
- `gzfile()` and `bzfile()` can open connections to compressed gzip and bzip2 files respectively
- `url()` opens a connection to a webpage
- `file()` opens a connection to any other file
	- `file(open = "")` is where you pass flags like `r, w, a` for read write and append (like python), and we can even pass `rb, wb, ab` for doing the same in binary mode (meant mainly for windows it seems)
- Conventionally, to open a connection
```R
> con = file("foo.txt", "r")
> data = read.csv(con)
> close(con)

# That's the same as
> data = read.csv("foo.txt")
```
- Functions like `read.csv()` do these steps under the hood
- Establishing a connection can be useful for reading lines of a file for example
### Subsetting
- When you subset an object in R with the single bracket operator`[`, it always return an object of the same class
	- The single bracket operator is used to select more than one element of the object
- The double bracket operator `[[` is used to select one element and the class of the returned object
- The dollar sign operator `$` extracts elements or lists of data by name with similar semantics to the double bracket operator
```R
> x = c("a", "b", "c", "c", "d", "a")
> x[1]
[1] "a"
> x[2]
[1] "b"
> x[1:4]
[1] "a" "b" "c" "c"
> x[x > "a"]
[1] "b" "c" "c" "c"
> u = x > "a"
> u
[1] FALSE TRUE TRUE TRUE TRUE FALSE
> x[u]
[1] "b" "c" "c" "d"
```
- If we had a list of lists, we can subset it to get a value from the nested list in two ways
	- `x[[c(1, 3]]` or `x[[1]][[3]]`
- Matrices are two dimensional, so we can subset it with `x[1, 2]`
	- Leaving an index blank in a matrix index, means you want the entire row, or entire column
	- `x[1,]` or `x[,1]`
- When a single element is retrieved from a matrix, it's returned as a vector of length 1 (this is an exception to the norm of `[` ), as R by default drops the dimensionallity of the matrix
	- This behavior is controllable though, and can be turned off with `x[1, 2, drop = FALSE]`
- Same goes for subsetting a single column or row, where R will return a vector, not a matrix (which makes sense since you want the element at those coordinates, not a new matrix)
#### Partial matching
- R supports partial matching when accessing objects
```R
> x = list(aardvark = 1:5)
> x$a
[1] 1 2 3 4 5
> x [["a"]]
NULL
> x[["a", exact = FALSE]]
[1] 1 2 3 4 5
```
- The `$` operator supports this by default, the `[[` operator doesn't, so we would have to specify it
#### Removing NA values
```R
> x = c(1, 2, NA, 4, NA, 5)
> bad = is.na(x)
> x [!bad]
[1] 1 2 4 5
```
- The approach here is to generate a logical vector of all the elements that are NA, then use it's inverse, to get back only the non-NA values
```R
> x = c(1, 2, NA, 4, NA, 5)
> y = c("a", "b", NA, "d", NA, "f")
> good = complete.cases(x, y)
> good
[1] TRUE TRUE FALSE TRUE FALSE TRUE
> x[good]
[1] 1 2 4 5
> y[good]
[1] "a" "b" "d" "f"
```
- `complete.cases()` is a function that takes two objects, and returns `TRUE` for positions that don't have NA values in both objects
```R
> airquality[1:6, ]
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6
> good = complete.cases(airquality)
> airquality[good, ][1:6, ]
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
7    23     299  8.6   65     5   7
8    19      99 13.8   59     5   8
```
#### Vectorized operations
- R supports the ability to add two vectors together, where each element from each vector will be added/multiplied/divided together based on their index
```R
> x = 1:4; y = 6:9
> x + y
> [1] 7 9 11 13
> x = 2
[1] FALSE FALSE TRUE TRUE
> x >= 2
[1] FALSE TRUE TRUE TRUE
> y == 8
[1] FALSE FALSE TRUE FALSE
> x * y
[1] 6 14 24 36
> x / y
[1] 0.1666667 0.2857143 0.3750000 0.4444444
```
- It's even possible to do this with matrices, where the opperatios are carried out coordinate wise
- R also has a special operator `%*%` which is for true matrix multiplications (don't know much about matrices to know what that means)
### Control Structures
#### If Else
- R has a similar `if else` syntax to the C syntax
```R
if(x > 3) {
	y = 10
} else {
y = 0
}
```
- R has a unique method of assignment with `if else` though, as you can set the entire `if else` block as a value for a variable (well, the variable will get the resulting value only ofc)
```R
y = if(x > 3) {
	10
} else {
	0
}
```
#### For loop
- This one is more like python's syntax
```R
for(i in 1:10) {
	print(i)
}
```
- Some more examples
```R
x = c("a", "b", "c", "d")

for(i in 1:4) {
	print(x[i])
}

for(i in seq_along(x)) {
	print(x[i])
}

for(letter in x) {
	print(letter)
}

for(i in 1:4) print(x[i])
```
- Note how `seq_along()` was used on x to generate a sequential int vector equal to the size of x. Basically the same as doing a range that's `1:length(x)`
#### While loop
- Honestly, nothing unique enough to be worth mentioning
#### Repeat/next/break
- R has a dedicated loop keyword for infinite loops called repeat
- You can break out of these only with `break`
- It's not really used much, as you would normally want to exit out of a loop eventually, and that's achievable with a `while` loop and its condition for example
- `next` and `break` are nothing new
### Functions
- Functions in R return the last evaluated expression automatically
```R
add2 = function(x, y) {
	x + y
}
```
- You can still use the `return()` function in R to return values whenever you want
```R
getHigherThan = function(comp, x) {
        higher = x[x > comp]
        
        if (length(higher) == 0) {
                return("No number in the vector was higher than the comparator")
        }
        higher
}
```
- R also support having a default value for a parameter in the function signature just like python
- Arguments in R are called formals, and the `formals()`function can be used to return a list of all the formal arguments of a function
- We can also use `args()` which just prints the function signature. Both functions will also show the default values of all the args when applicable
- When setting the value of a function, R supports the partial matching, so you can write part of the argument's name and R will accept it
- R also supports lazy evaluations, so you don't have to make arguments optional in any way, as you can ignore a function's arguments if they're not needed, even if they have no default value, and R will allow it
```R
# This is allowed
f = function(a, b) {
	a^2
}
```
#### The ... argument
- R also has a very cool feature, the `...` argument
	- Unlike the spread operator from JS or the [[PG Go note dump#Variadic|variadic]] operator from Go, the `...` argument in R indicates a variable number of arguments that can be passed on to another function
	- It's useful for when you want to extend another function, and you don't want to copy the full argument list of the first, since R's functions can have some very long argument lists
```R
# Here, myplot is used to change the default plot type of `plot()`, and the remaining args are passed as is using `...`
myplot = function(x, y, type = 'l', ...) {
	plot(x, y, type = type, ...)
}
```
- This is also very useful for generic functions
```R
> mean
function (x, ...)
UseMethod("mean")
```
- And when the number or arguments isn't known in advance
```R
> testfunc = function(...) {
+ sum(...)/length(c(...))
+ }
> testfunc(1,2,3,4,5)
[1] 3
```
- Another example are `paste()` and `cat()`, which function similarly, as they both concatenate strings together, but `cat()` can also print the output to a file unlike `pate()`
```R
>  args(paste)
function (..., sep = " ", collapse = NULL, recycle0 = FALSE) 
> args(cat)
function (..., file = "", sep = " ", fill = FALSE, labels = NULL, 
    append = FALSE) 
```
- The catch with `...` though, is that any argument that comes after it must be names explicitly, despite R supporting partial matching and positional arguments
### Scope
- R relies on namespaces to separate its various symbols
- If you assign a value to a variable, R searches its packages in order until it finds the value of that variable when it's called, and the global environment is always the one that will be searched first, while the base package is always last
- The order of the search is as follows:
```R
> search()
 [1] ".GlobalEnv"        "tools:rstudio"     "package:stats"     "package:graphics"  "package:grDevices"
 [6] "package:utils"     "package:datasets"  "package:methods"   "Autoloads"         "package:base" 
```
- R also separates functions from non-functions into separate namespaces
- We can also configure which packages get loaded on startup
- Loading a new package automatically puts it in position 2 of the search list, shifting everything else down the list
- Scoping is the main difference between R and S, as R uses lexical scoping
- Lexical scoping basically refers to R's ability to use a variable defined in the global scope inside a function even if it's not explicitly passed to it as an argument. Which is not dissimilar from most other languages
- A closure in R is the association between a function and its environment in which it was defined
- When a free variable (global scope variable) is used within a function in R, the variable is searched for first in the global environment, and if not found, then in the variable's parent environments in a top down style
- R supports higher-order functions, and as such, you can define and return function inside of R, who's scope, will be the body of the parent function
```R
make.power = function(n) {
	pow = function(x) {
		x^n
	}
	pow
}
```
