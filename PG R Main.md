---
tags:
- R
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG R index|Back to index]]

![[rstudio-ide.png]]
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
- We can check the version of a package with `packageVersion("dplyr")`
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
![[PG base-r-cheat-sheet.pdf]]
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
- In R, to check if a value is in an array, we use `%in%`
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
m = 1:10
dim(m) = c(2, 5)
m
m
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
## Subsetting
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
### Partial matching
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
### Removing NA values
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
## Vectorized operations
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
## Control Structures
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
### For loop
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
### While loop
- Honestly, nothing unique enough to be worth mentioning
### Repeat/next/break
- R has a dedicated loop keyword for infinite loops called repeat
- You can break out of these only with `break`
- It's not really used much, as you would normally want to exit out of a loop eventually, and that's achievable with a `while` loop and its condition for example
- `next` and `break` are nothing new
## Functions
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
### The ... argument
- R also has a very cool feature, the `...` argument
	- Unlike the spread operator from JS or the [[PG Go Main#Variadic|variadic]] operator from Go, the `...` argument in R indicates a variable number of arguments that can be passed on to another function
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
## Scope
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
- `ls()` lists all the functions in an environment
	- When used on a function returned by a factory function, the returned objects will all be from the factory function's closure
- `get()` returns the value of an object, provided the name of the object and its closure environment `get("n", environment(cube))`
- Due to R relying on lexical scoping over dynamic scoping provided we have an example like the following:
```R
y = 10

f = function(x) {
	y = 2
	y^2 + g(x)
}

g = function(x) {
	x*y
}
```
- If we were to call `f(3)`, the result will consider `y = 10` not `y = 2`
	- This because lexical scoping prioritizes the environment in which the function was defined, not the one where it was called (known as the parent frame in R)
	- This is why the answer is 34, where in `f()` the `y = 2` so `y^2` is 4, and in `g()` the `y = 10` which will give 4 + 30
- Python actually uses lexical scoping as well
- With lexical scoping, all objects are stored in memory, and all objects always have a pointer to their respective defining environments
	- This introduces a limitation as only physical memory can be used here, not virtual memory, which could be problematic with larger objects
### Optimization
- R has functions for optimizing the values of certain parameters in a function
- These functions expect a parameters array to work on
- These functions are for example `optim()`, `nlm()` and `optimize()`
- It's normally desirable to allow a function to have params that the caller can hold fixed for the optimization to work
```R
make.NegLogLike = function(data, fixed=c(FALSE, FALSE)) {
	params = fixed
	function(p) {
		params[!fixed] = p
		mu = params[1]
		sigma = params[2]
		a = -0.5*length(data) * log(2*pi*sigma^2)
		b = -0.5*sum((data-mu)^2) / (sigma^2)
		-(a + b)
	}
}
```
- The above example show a constructor function (factory function) being used to pass data and a vector of params to an objective function (internal function)
- The function is supposed to find the negative log likelihood of a normal distribution, and optimization functions will attempt to minimize that likelihood by default
- Defining a function in any other environment than the global environment (such as the inside of another function) will return a special tag called environment, which is a pointer to the defining environment `<environment: 0x165b1a4>`
```R
> set.seed(1); normals = rnorm(100, 1, 2)
> nLL = make.NegLegLik(normals)
> nLL
function(p) {
	params[!fixed] = p
	mu = params[1]
	sigma = params[2]
	a = -0.5*length(data) * log(2*pi*sigma^2)
	b = -0.5*sum((data-mu)^2) / (sigma^2)
	-(a + b)
}
<environment: 0x165b1a4>
> ls(environment(nLL))
[1] "data" "fixed" "params"
```
```R
> optim(c(mu = 0, sigma = 1), nLL)$par
	   mu    sigma
 1.218239 1.787343
```
 - Fixing sigma = 2
```R
 > nLL = make.NegLogLik(normals, c(FALSE,2))
 > optimize(nLL, c(-1, 3))$minimum
 [1] 1.217775
```
- Fixing mu = 1
```R
> nLL = make.NegLogLik(normals, c(1, FALSE))
> optimize(nLL, c(1e-6, 10))$minimum
[1] 1.800596
```
- The likelihood can then be plotted
```R
nLL = make.NegLogLik(normals, c(1, FALSE))
x = seq(1.7, 1.9, len=100)
y = sapply(x, nLL)
plot(x, exp(-(y - min(y))), type = "l")

nLL = make.NegLogLik(normals, c(FALSE, 2))
x = seq(0.5, 1.5, len = 100)
y = sapply(x, nLL)
plot(x, exp(-(y - min(y))), type = "l")
```
- The objective function will contain the data, as well as the arguments, thanks to the constructor function
## Dates and times
- R represents dates and times in a special way, using two different classes `POSIXct` and `POSIXlt` for time, and `Date` for dates
- Internally, dates are stored as the number of days since 1970-01-01, and time as the number of seconds since the same date
- Times don't have time attached to them
```R
x = as.Date("1970-01-01")
x
[1] "1970-01-01"
unclass(x)
[1] 0
unclass(as.Date("1970-01-02"))
[1] 1
```
- `POSIXct` stores time as a long integer, while `POSIXlt` stores time as a list alongside other useful information such as the day of the week, of the year, and of the month, as well as the month
- There are also functions that operate on dates and times
	- `weekdays`: give the day of the week
	- `months`: give the month name
	- `quarters`: give the quarter number
- Times like dates can be coerced from a string using `as.POSIXct` and `as.POSIXlt`
```R
> x = Sys.time()
> x
[1] "2026-04-14 18:05:57 EET"
> p = as.POSIXlt(x)
> names(unclass(p))
 [1] "sec"    "min"    "hour"   "mday"   "mon"    "year"   "wday"   "yday"   "isdst"  "zone"   "gmtoff"
> p$sec
[1] 57.75504
```
- `Sys.time()`'s output is already in `POSIXct` format
- `strptime()` is a function that can take a date string and some formatters, then return the resulting datetime object in `POSIXlt` format `strptime(datestring, "%B, %d, %Y, %H:%M")`
- Dates and time support the `+` and `-` mathematical operators, as well as comparisons
- Dates and times even keep track of leap years, daylight savings, and time zones
## Loop functions
- These are functions that have the word "apply" in them
- `lapply`: loop over a list and evaluate a function on each element, kinda like a `.forEach` in JS
- `sapply`: same as `lapply` but try to simplify the result
- `apply`: apply a function over the margins of an array. Useful with matrices and other higher dimension arrays
- `tapply`: apply a function over subsets of a vector
- `mapply`: multivariate version of `lapply`
- `split` can split objects and is useful when used in conjunction with an apply function
### lapply
- `lapply` takes 3 arguments, a list, a function, and other arguments
	- The loop itself is actually done internally in C
	- If supplied with something that's not a list, it will attempt to coerce it into one, or throw an error if it fails
	- It always returns a list regardless of the class of the input
```R
> x = list(a = 1:4, b = rnorm(10), c = rnorm(20,1), d = rnorm(100, 5))
> lapply(x, mean)
$a
[1] 2.5

$b
[1] 0.3474802

$c
[1] 0.937003

$d
[1] 4.957727
```
- Another example using `runif` which generates uniform random variables using a random number generator
	- `runif` generates as many RNGs as the number it's applied on, so 1 for `runif(1)`, 2 for `runif(2)`, and so on
```R
> x = 1:4
> lapply(x, runif)
[[1]]
[1] 0.1621091

[[2]]
[1] 0.475397920 0.001932835

[[3]]
[1] 0.4414591 0.2609297 0.9384137

[[4]]
[1] 0.7158333 0.1630855 0.4761880 0.6902567
```
- `runif` can take some other arguments, and we can supply them as well via `lapply`
```R
> lapply(x, runif, min = 0, max = 10)
[[1]]
[1] 4.608952

[[2]]
[1] 9.551467 7.125401

[[3]]
[1] 3.971479 1.177206 2.401163

[[4]]
[1] 8.636306 4.359764 4.978681 6.919277
```
- The `apply` family can also be used with anonymous functions
```R
> x = list(a = matrix(1:4, 2, 2), b = matrix(1:6, 3, 2))
> lapply(x, function(elt) elt[,1])
$a
[1] 1 2

$b
[1] 1 2 3
```
### sapply
- `sapply` simplifies the results, which means that while `lapply` always returns a list, `sapply` will try to simplify that if possible
- For example, if `lapply` will return a list where each element is of length 1, `sapply` will instead return a vector. If instead every element is a vector of the same length, then a matrix will be returned. If `sapply` can't figure out a simplification, a list will be returned
### apply
- `apply` takes one extra argument, which is a margin to be retained
- It's generally used with higher dimension arrays to apply a function to, for example, the columns or rows of a matrix
```R
> x = matrix(rnorm(200), 20, 10)
> apply(x, 2, mean)
 [1]  0.02449109  0.07583942  0.14146002  0.21051413  0.09442709  0.02218266 -0.15932850  0.09021391
 [9]  0.14723035 -0.22431309
> apply(x, 1, sum)
 [1]  5.6773875  3.2343443  2.0688445 -5.6814233  3.3122708 -0.3792983 -4.4445006 -0.1156158 -2.8256077
[10]  0.4223744  0.1913922  2.4322329 -1.2574098  0.9317553  2.6447572  4.2018323 -1.0060557  1.6188788
[19] -7.5141518  4.9423346
```
- In the above example, we applied mean first on each column of the matrix, then sum on each row
- Had we used `lapply` here, it would have returned a list of 200 elements, and `sapply` would have simplified that to a vector of 200 elements
- For cols and rows though, a much faster method of calculating the sums and means would be to use the dedicated functions:
	- `rowSums`
	- `rowMeans`
	- `colSums`
	- `colMeans`
- These functions are much faster than using apply, especially on large matrices
- Another example with `quantile`
```R
> apply(x, 1, quantile, probs = c(0.25, 0.75))
         [,1]       [,2]       [,3]       [,4]       [,5]       [,6]        [,7]       [,8]       [,9]
25% 0.7153803 -0.6346605 -0.5233306 -0.8343386 -0.3544961 -0.8447185 -0.63733164 -0.2856758 -0.8083212
75% 1.0435839  1.2633594  0.6725798 -0.3130326  1.1609297  0.5731975 -0.02612577  0.5116271  0.2837464
         [,10]      [,11]     [,12]      [,13]      [,14]      [,15]      [,16]      [,17]      [,18]
25% -0.7827622 -0.3885836 0.3025282 -0.7308451 -0.5537525 -0.2782507 0.02738015 -1.0011764 -0.1396592
75%  0.9236771  0.8167648 0.5869685  0.3460132  0.2237042  0.6699093 0.98505861  0.9329413  0.4408073
         [,19]      [,20]
25% -1.3107189 -0.2781763
75% -0.2110138  0.9061585
```
- Another example with an array that holds 10 2x2 matrices, which means it has 3 dimensions
```R
> a = array(rnorm(2 * 2 * 10), c(2, 2, 10))
> apply(a, c(1, 2), mean)
           [,1]       [,2]
[1,] -0.1000849 -0.1717568
[2,]  0.2343975  0.1940927
> rowMeans(a, dims = 2)
           [,1]       [,2]
[1,] -0.1000849 -0.1717568
[2,]  0.2343975  0.1940927
```
- Here we kept the first and second dimensions, and took the mean of all the matrices, which returned a matrix. We also can see how that would be done with `rowMeans`
### mapply
- It applies a function over a set of arguments in parallel
- Results are simplified by default, but you can choose not to simplify them
- The functions used with `mapply` must take multiple args, as element 1 of each function will be passed to each of the args, then 2, then 3 and so on
- This means the number of args of the `mapply` func, must be at least as many as the number of lists that we pass to `mapply`
```R
> mapply(rep, 1:4, 4:1)
[[1]]
[1] 1 1 1 1

[[2]]
[1] 2 2 2

[[3]]
[1] 3 3

[[4]]
[1] 4
```
- In the above example, `rep` takes 2 args, the number to be repeated, and the number of repetitions
- Instead of creating a list with 4 separate calls to `rep`, we use `mapply` and two vectors, `1:4` and `4:1`. The result is, that each element of each vector will be passed to `rep`'s args by index, so 1 and 4, 2 and 3, 3 and 2, then 4 and 1, giving the resulting list of 4 vectors
### tapply
- Used to apply a function on a subset of a vector
- You can choose to NOT simplify the output, which is simplified by default, same as with `mapply`
- Takes a factor variable (categorical variable) to subset a vector, and applies the function on the subset
```R
> x = c(rnorm(10), runif(10), rnorm(10,1))
> x
 [1]  1.22474329 -0.36777511 -1.62840260  0.54322239  1.12749080 -0.41607110  0.39945592  0.90586474
 [9] -0.31548009  0.21295795  0.37984482  0.79926283  0.76851398  0.55482586  0.65426236  0.52139844
[17]  0.28219220  0.72079695  0.02915776  0.35614220 -0.21100346  0.66848058  2.55670488  1.79782156
[25]  1.22266300  2.20423180 -0.56523335  1.33282837  1.71137286  3.21169852
> f = gl(3, 10)
> f
 [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3
Levels: 1 2 3
> tapply(x, f, mean)
        1         2         3 
0.1686006 0.5066397 1.3929565 
```
- The above vector has 3 groups
	- 10 normals
	- 10 uniforms
	- 10 normals with a mean of 1
- With `tapply`, we manage to get the mean of each group, by also supplying a 3 levels factor that we generated
- In another example, we can use `range` to find the min and max of each factor
```R
> tapply(x, f, range)
$`1`
[1] -1.628403  1.224743

$`2`
[1] 0.02915776 0.79926283

$`3`
[1] -0.5652334  3.2116985
```
### split
- `split` isn't a loop function, but it works very nicely with apply functions
- `split` always returns a list
- It takes a factor like `tapply` as well as a vector, and splits the vector by the factor's levels
- We can then use an apply function on any of those individual groups
```R
> x = c(rnorm(10), runif(10), rnorm(10,1))
> x
 [1]  1.22474329 -0.36777511 -1.62840260  0.54322239  1.12749080 -0.41607110  0.39945592  0.90586474
 [9] -0.31548009  0.21295795  0.37984482  0.79926283  0.76851398  0.55482586  0.65426236  0.52139844
[17]  0.28219220  0.72079695  0.02915776  0.35614220 -0.21100346  0.66848058  2.55670488  1.79782156
[25]  1.22266300  2.20423180 -0.56523335  1.33282837  1.71137286  3.21169852
> f = gl(3, 10)
> f
 [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3
Levels: 1 2 3
> split(x, f)
$`1`
 [1]  1.2247433 -0.3677751 -1.6284026  0.5432224  1.1274908 -0.4160711  0.3994559  0.9058647 -0.3154801
[10]  0.2129579

$`2`
 [1] 0.37984482 0.79926283 0.76851398 0.55482586 0.65426236 0.52139844 0.28219220 0.72079695 0.02915776
[10] 0.35614220

$`3`
 [1] -0.2110035  0.6684806  2.5567049  1.7978216  1.2226630  2.2042318 -0.5652334  1.3328284  1.7113729
[10]  3.2116985
```
- These two use cases are the same, and neither is more efficient
```R
> lapply(split(x,f), mean)
$`1`
[1] 0.1686006

$`2`
[1] 0.5066397

$`3`
[1] 1.392956

> tapply(x, f, mean, simplify = FALSE)
$`1`
[1] 0.1686006

$`2`
[1] 0.5066397

$`3`
[1] 1.392956
```
- The nice thing about `split` though, is that it can split some much more complicated objects
```R
> library(datasets)
> head(airquality)
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6
> s = split(airquality, airquality$Month)
> lapply(s, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")]))
$`5`
   Ozone  Solar.R     Wind 
      NA       NA 11.62258 

$`6`
    Ozone   Solar.R      Wind 
       NA 190.16667  10.26667 

$`7`
     Ozone    Solar.R       Wind 
        NA 216.483871   8.941935 

$`8`
   Ozone  Solar.R     Wind 
      NA       NA 8.793548 

$`9`
   Ozone  Solar.R     Wind 
      NA 167.4333  10.1800 
      
# This also works
> tapply(airquality, airquality$Month, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")]))
$`5`
   Ozone  Solar.R     Wind 
      NA       NA 11.62258 

$`6`
    Ozone   Solar.R      Wind 
       NA 190.16667  10.26667 

$`7`
     Ozone    Solar.R       Wind 
        NA 216.483871   8.941935 

$`8`
   Ozone  Solar.R     Wind 
      NA       NA 8.793548 

$`9`
   Ozone  Solar.R     Wind 
      NA 167.4333  10.1800
```
- If we wanted to get the data as a matrix instead
```R
> sapply(s, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")]))
               5         6          7        8        9
Ozone         NA        NA         NA       NA       NA
Solar.R       NA 190.16667 216.483871       NA 167.4333
Wind    11.62258  10.26667   8.941935 8.793548  10.1800
> sapply(s, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")], na.rm = TRUE))
                5         6          7          8         9
Ozone    23.61538  29.44444  59.115385  59.961538  31.44828
Solar.R 181.29630 190.16667 216.483871 171.857143 167.43333
Wind     11.62258  10.26667   8.941935   8.793548  10.18000
```
- This is due to `sapply` simplifying the results, and not splitting the data by factors
#### Splitting on more than one level
- Some times, we would have more than one factor to split by
##### Combining factors (Combinatorics)
```R
> x = rnorm(10)
> f1 = gl(2, 5)
> f2 = gl(5, 2)
> f1
 [1] 1 1 1 1 1 2 2 2 2 2
Levels: 1 2
> f2
 [1] 1 1 2 2 3 3 4 4 5 5
Levels: 1 2 3 4 5
> interaction(f1, f2)
 [1] 1.1 1.1 1.2 1.2 1.3 2.3 2.4 2.4 2.5 2.5
Levels: 1.1 2.1 1.2 2.2 1.3 2.3 1.4 2.4 1.5 2.5
```
- Using `interaction` here, we generated all the possible combinations between both factors
- We can then split using a list of both factors
```R
> str(split(x, list(f1, f2)))
List of 10
 $ 1.1: num [1:2] 1.383 -0.137
 $ 2.1: num(0) 
 $ 1.2: num [1:2] -0.474 -1.26
 $ 2.2: num(0) 
 $ 1.3: num -0.66
 $ 2.3: num 0.461
 $ 1.4: num(0) 
 $ 2.4: num [1:2] 0.271 -0.755
 $ 1.5: num(0) 
 $ 2.5: num [1:2] -2.249 -0.642
 
 # Droping empty levels
 > str(split(x, list(f1, f2), drop = TRUE))
List of 6
 $ 1.1: num [1:2] 1.383 -0.137
 $ 1.2: num [1:2] -0.474 -1.26
 $ 1.3: num -0.66
 $ 2.3: num 0.461
 $ 2.4: num [1:2] 0.271 -0.755
 $ 2.5: num [1:2] -2.249 -0.642
```
- Here, the split function is trying to group similar values from both groups
- What the test factors did was, split the same int vector in two different ways
	- In `f1`, we split the first five numbers into group 1, and the latter 5 into group 2
	- In `f2` each 2 numbers were added to a group, for a total of 5 groups
- When we try to find interactions between them, we basically check which numbers in `f1` intersect with `f2`, by combining 1.1, 2.1, 1.2, 2.2, and so on
- The reason for the empty levels here, is that level 2 in `f1` only has the latter 5 numbers of the vector, while in `f2`, the first 2 and half levels, have the first 5 numbers, so only level 1 of `f1` intersects with those levels
- After the first 5 numbers, it switches, and only level 2 of `f1` is finding intersections now
- If it's still unclear (future me (: ) basically map the first 5 numbers of an int vector to 1 level, and then split them over 3 levels, and do the same for the 2nd 5 numbers
	- Logically, because we went in order, the first 5 numbers filling level 1 will have filled the 2nd factor's 5 groups of 2, as numbers 1:2, then 3:4, then 5, and the same for the 2nd 5
## Debugging tools
- R has built in debugging tools
- The types of diagnostic messages to expect from R are
	- `message`: A generic diagnostic notification
	- `warn`: An indication that something is wrong, but not fatal
	- `error`: An indication something is wrong and fatal
	- `condition`: You're own self declared error (kinda like python's except I suppose)
- R's debugging tools are
	- `traceback`: prints out the function call stack in case of an error
	- `debug`: flags a function for "debug" mode, then you can step through execution one line at a time (basically a debugger)
	- `browser`: suspends the execution of a function wherever it's called, and puts it in debug mode (kinda like a deferred `debug`)
	- `trace`: allows you to insert debugging code into a function at a specific place
	- `recover`: basically a `catch` for R's PANIC, that allows you to handle the thrown error [[PG Go Main#Panic!!!!|Go]] style
```R
> lm(y ~ x)

Error in eval(predvars, data, env) : object 'y' not found

> traceback()
7: eval(predvars, data, env)
6: eval(predvars, data, env)
5: model.frame.default(formula = y ~ x, drop.unused.levels = TRUE)
4: stats::model.frame(formula = y ~ x, drop.unused.levels = TRUE)
3: eval(mf, parent.frame())
2: eval(mf, parent.frame())
1: lm(y ~ x)

# The `...` are not part of the actual output, I was just trying to shorten it a bit
> debug(lm)
> lm(y ~ x)
debugging in: lm(y ~ x)
debug: {
    ret.x <- x
    ret.y <- y
    cl <- match.call()
    mf <- match.call(expand.dots = FALSE)
    m <- match(c("formula", "data", "subset", "weights", "na.action", 
        "offset"), names(mf), 0L)
    ...
}
Browse[1]> 
debug: ret.x <- x
Browse[1]> 
debug: ret.y <- y
Browse[1]> 
debug: cl <- match.call()
Browse[1]> 
debug: mf <- match.call(expand.dots = FALSE)
...
Error in eval(predvars, data, env) : object 'y' not found

# This sets a global recovery option for the current session only
> options(error= recover)
> read.csv("menoexist")
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file 'menoexist': No such file or directory

Enter a frame number, or 0 to exit   

1: read.csv("menoexist")
2: read.table(file = file, header = header, sep = sep, quote = quote, dec = dec, fill = fill, comment.char
3: file(file, "rt")
Selection: 
```
## str
- The `str` function displays the internal structure of an R object in a compact fashion
```R
> x = rnorm(100, 2, 4)
> str(x)
 num [1:100] -8.348 0.538 0.839 4.744 8.924 ...
 
> f = gl(40,10)
> str(f)
 Factor w/ 40 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...
 
 > str(airquality)
'data.frame':	153 obs. of  6 variables:
 $ Ozone  : int  41 36 12 18 NA 28 23 19 8 NA ...
 $ Solar.R: int  190 118 149 313 NA NA 299 99 19 194 ...
 $ Wind   : num  7.4 8 12.6 11.5 14.3 14.9 8.6 13.8 20.1 8.6 ...
 $ Temp   : int  67 72 74 62 56 66 65 59 61 69 ...
 $ Month  : int  5 5 5 5 5 5 5 5 5 5 ...
 $ Day    : int  1 2 3 4 5 6 7 8 9 10 ...
 
> s = split(airquality, airquality$Month)
> str(s)
List of 5
 $ 5:'data.frame':	31 obs. of  6 variables:
  ..$ Ozone  : int [1:31] 41 36 12 18 NA 28 23 19 8 NA ...
  ..$ Solar.R: int [1:31] 190 118 149 313 NA NA 299 99 19 194 ...
  ..$ Wind   : num [1:31] 7.4 8 12.6 11.5 14.3 14.9 8.6 13.8 20.1 8.6 ...
  ..$ Temp   : int [1:31] 67 72 74 62 56 66 65 59 61 69 ...
  ..$ Month  : int [1:31] 5 5 5 5 5 5 5 5 5 5 ...
  ..$ Day    : int [1:31] 1 2 3 4 5 6 7 8 9 10 ...
 $ 6:'data.frame':	30 obs. of  6 variables:
  ..$ Ozone  : int [1:30] NA NA NA NA NA NA 29 NA 71 39 ...
  ..$ Solar.R: int [1:30] 286 287 242 186 220 264 127 273 291 323 ...
  ..$ Wind   : num [1:30] 8.6 9.7 16.1 9.2 8.6 14.3 9.7 6.9 13.8 11.5 ...
  ..$ Temp   : int [1:30] 78 74 67 84 85 79 82 87 90 87 ...
  ..$ Month  : int [1:30] 6 6 6 6 6 6 6 6 6 6 ...
  ..$ Day    : int [1:30] 1 2 3 4 5 6 7 8 9 10 ...
 $ 7:'data.frame':	31 obs. of  6 variables:
  ..$ Ozone  : int [1:31] 135 49 32 NA 64 40 77 97 97 85 ...
  ..$ Solar.R: int [1:31] 269 248 236 101 175 314 276 267 272 175 ...
  ..$ Wind   : num [1:31] 4.1 9.2 9.2 10.9 4.6 10.9 5.1 6.3 5.7 7.4 ...
  ..$ Temp   : int [1:31] 84 85 81 84 83 83 88 92 92 89 ...
  ..$ Month  : int [1:31] 7 7 7 7 7 7 7 7 7 7 ...
  ..$ Day    : int [1:31] 1 2 3 4 5 6 7 8 9 10 ...
 $ 8:'data.frame':	31 obs. of  6 variables:
  ..$ Ozone  : int [1:31] 39 9 16 78 35 66 122 89 110 NA ...
  ..$ Solar.R: int [1:31] 83 24 77 NA NA NA 255 229 207 222 ...
  ..$ Wind   : num [1:31] 6.9 13.8 7.4 6.9 7.4 4.6 4 10.3 8 8.6 ...
  ..$ Temp   : int [1:31] 81 81 82 86 85 87 89 90 90 92 ...
  ..$ Month  : int [1:31] 8 8 8 8 8 8 8 8 8 8 ...
  ..$ Day    : int [1:31] 1 2 3 4 5 6 7 8 9 10 ...
 $ 9:'data.frame':	30 obs. of  6 variables:
  ..$ Ozone  : int [1:30] 96 78 73 91 47 32 20 23 21 24 ...
  ..$ Solar.R: int [1:30] 167 197 183 189 95 92 252 220 230 259 ...
  ..$ Wind   : num [1:30] 6.9 5.1 2.8 4.6 7.4 15.5 10.9 10.3 10.9 9.7 ...
  ..$ Temp   : int [1:30] 91 92 93 93 87 84 80 78 75 73 ...
  ..$ Month  : int [1:30] 9 9 9 9 9 9 9 9 9 9 ...
  ..$ Day    : int [1:30] 1 2 3 4 5 6 7 8 9 10 ...
```
## Simulation
- A good way for practicing R, or testing functions with statistical objectives, is the use of a simulation
- R provides built-in functions for generating probability distributions
	- `rnorm`: generate random Normal variates with a gives mean and standard deviation. 
	- `dnorm`: evaluate the Normal probability density (with a given mean/SD) at a point (or vector of points)
	- `pnorm`: evaluate the cumulative distribution function for a Normal distribution
	- `rpois`: generate random Poisson variates with a given rate
- Some prefixes that we will run by occasionally, as they exist for every kind of distribution generation function
	-  r for random number generations
	- d for density
	- p for cumulative distribution
	- q for quantile function
### seeding
- It's important to set a seed before generating random numbers with `set.seed`
- This ensures reproducible results, as random generation on computers only generates psudo-random numbers, there is no true randomness
### simulating a linear model
- We will use an example with a single predictor, and some random noise
	- $y = B{_0} + B{_1}X + E$
	- $E$ ~ $N$(0,2$^2$), $X$ ~ $N$(0,1$^2$), $B_0$ = 0.5 and $B_1$ = 2
```R
> set.seed(20)
> x = rnorm(100)
> e = rnorm(100, 0, 2)
> y = 0.5 + (2 * x) + e
> summary(y)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-6.4084 -1.5402  0.6789  0.6893  2.9303  6.5052 
> plot(x, y)
```
- What if x is binary?
```R
> x = rbinom(100, 1, 0.5)
> y = 0.5 + (2 * x) + e
> summary(y)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-3.0489  0.1981  1.6548  1.6395  3.1800  6.3324 
```
- This means that we have 100 values, where there is a 50% chance the value will 1
- Now for a generalized linear model where we simulate from a Poisson model, in which:
	- Y ~ Poisson($u$)
	- $log u = B{_0} + B{-1}x$
	- $B_0$  = 0.5 and $B_1$ = 0.3
	- We need to use `rpoise`
```R
> plot(x, y)
> set.seed(1)
> x = rnorm(100)
> log.mu = 0.5 + 0.3 * x
> y = rpois(100, exp(log.mu))
> summary(y)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   0.00    1.00    1.00    1.55    2.00    6.00 
> plot(x, y)
```
### sample
- The `sample` function draws randomly from a set of scalar objects
```R
> set.seed(1)
> sample(1:10, 4)
[1] 9 4 7 1
> sample(1:10, 4)
[1] 2 7 3 6
> sample(letters, 5)
[1] "r" "s" "a" "u" "w"
> sample(1:10)
 [1] 10  6  9  2  1  5  8  4  3  7
> sample(1:10)
 [1]  5 10  2  8  6  1  4  3  9  7
> sample(1:10, replace = TRUE)
 [1]  3  6 10 10  6  4  4 10  9  7
```
## Profiling
- The act of examining how much time R code spends running, with the goal of optimizing it
- We can use `system.time` to calculate the time an R expression took to be evaluated
	- It computes the time in seconds
	- If we run into an error, it will give you the time until the error occurred
	- it returns an object of class `proc_time`
- There are two ways of looking at time when it comes to code
	- **User time:** the time the CPU(s) took to run the expression
	- **Elapsed time:** the real time spent for the expression to run
- User time and elapsed time are usually very close, but they mainly diverge when using a package that can leverage multi-threading, in which case elapsed time will be smaller
- Using an external API where the network could be the actual bottleneck, not the CPU, is an example of when elapsed time would be higher than user time
### Rprof
- Another option are `Rprof` and `summaryRprof`, where `Rprof` is the actual profiller, and `summaryRprof` summarizes the output into something readable
- Note that `system.time` and `Rprof` are not designed to be used together, so don't do that
- `Rprof` keeps track of the function call stack at regular intervals, and tabulates how much time was spent in each function
	- The profiler isn't really useful if your code is already running very fast. You wouldn't exactly need it if it was
- `summaryRprof` is the same, but summarized, duh
- It offers two summary methods
	- `by.total`: divides the time spent in each function by the total run time
	- `by.self`: does the same but first subtracts out time spent in functions about in the call stack
		- This is more interesting as it highlights the amount of time the function ran for, alone, while cutting out the run time of helper functions that it may have called
- Note, any underlying C or Fortran code that R may have used will not be profiled
## Files and directories
- We use `file.exists("directory name")` to check for the existence of a directory, even the the function has "file" in it
- `dir.create("dir name")` is used to create a directory
```R
if (!file.exists("data")) {
	dir.create("data")
}
```
- We can also download files from the internet using `download.file()`
```R
fileUrl = "https://data.baltimorecity.gov/api/views/dz54-2aru/rows.csv?accessType=DOWNLOAD"
download.file(fileUrl, destfile = "./data/cameras.csv", method = "curl")
list.files("./data")
```
- The `curl` method is mainly needed on unix systems (they say mac I assume unix) when dealing with `https` urls
### Excel
- For excel files we use `read.xlsx` from the `xlsx` package
- The function takes a `sheetIndex` argument to specify the sheet with the required data and a `header` argument for keeping header names
- We can also read a subset of the excel file using the `colIndex` and `rowIndex` arguments
- Excel can be written back out of R using `write.xlsx`
- `read.xlsx2` is a faster version of the function, but apparently it can be slightly unstable with subsetting
### XML
- This requires the `xml` package
- The function here is `xmlTreeParse()` which parses in xml into a tree structure
- This function is not a one-hit wonder like the rest of them, this time, the parsed xml will be stored in a variable, and we actually need more functions to process it
- The `xmlRoot()` function will access the root tag (node), which basically stores the whole parsed document in a variable
- `xmlName()` gets the name of a node
- We can also use list accessing syntax to access elements of the parsed node `rootNode[[1]]`
```R
library(XML)
fileUrl = "http://example"
doc = xmlTreeParse(fileUrl, useInternal=TRUE)
rootNode = xmlRoot(doc)
xmlName(rootNode)
```
- The xml package even has xml specific apply commands, one of which is `xmlSApply()` which will programatically extract parts of the file
```R
xmlSApply(rootNode, xmlValue)
```
- xml even brandishes a dedicated language, "XPath" that can be used for parsing xml nodes in R
```
- /node Top level node
- //node Node at any level
- node[@attr-name] Node with an attribute name
- node[@attr-name='bob'] Node with attribute name attr-name='bob'
```
### HTML
- HTML requires the use of `htmlTreeParse()`
```R
fileUrl = "http://example"
doc = htmlTreeParse(fileUrl, useInternal=TRUE)
scores = xpathSApply(doc, "//li[@class='score']", xmlValue)
teams = xpathSApply(doc, "//li[@class='team-name']", xmlValue)
```
### JSON
- The `jsonlite` library is needed here
```R
library(jsonlite)
jsonData = fromJSON("http://example")
names(jsonData)
```
- Reading JSON or any of the other file types in this section should load them into a `data.frame`
- We can also write to JSON
```R
myjson = toJSON(iris, pretty=TRUE)
cat(myjson)
```
# Data Tables
- Data tables are the successors to data frames
- The package is written in C and is very fast
- It's way faster at subsetting, grouping and updating
- It has a slightly different syntax sometimes though
## Creation
```R
DF = data.frame(x=rnorm(9), y=rep(c("a","b","c"), each=3), z=rnorm(9))
DT = data.table(x=rnorm(9), y=rep(c("a","b","c"), each=3), z=rnorm(9))
```
- The `tables`function show how many tables are loaded in memory, and their total size, names, number of rows, cols and keys
## Subsetting
```R
DT[2,]

DT[DT$y="a",]
```
- Subsetting rows and columns here is different
```R
# Using a single index subsets rows instead of columns
DT[c(2,3)]
```
- The column subset function in data tables uses expressions
- An expression in R is a collection of statements enclosed in  curly brackets
- We can use this to pass functions to the data table that we want to run on all the columns
```R
DT[,list(mean(x), sum(z))]
```
- We can even use a walrus operator to quickly add a new column
```R
DT[,W:=z^2]
```
- Note that when adding new data to data frames, R creates a new data frame where the additions are applied. With data tables that's not the case
- Data tables also have some special variables
- `.N` allows you to do a unique total count of a specific column's variables
```R
set.seed(123);
DT = data.table(x=sample(letters[1:3], 1E5, TRUE))
DT[, .N, by=x]

	x     N
1:  a 33387
2:  c 33201
3:  b 33412
```
## Keys
- Data table also allows us to set a key
- This feels a lot like a SQL index, but the difference is that a key just sorts the data in place by the chosen column
```R
DT = data.table(x=rep(c("a","b","c"), each=100), y=rnorm(300))
setkey(DT, x)
DT['a']
Key: <x>
          x           y
     <char>       <num>
  1:      a -1.65808828
  2:      a  0.48353355
  3:      a -0.12598362
  4:      a  1.36050733
  5:      a -0.95224638
  6:      a -0.67258096
  7:      a -0.10470971
  8:      a -0.19415974
  9:      a -0.31749215
 10:      a -1.28466190
 11:      a -0.71499752
 #---rest
```
## Joins
- We even have the ability to do table joins, sure feels like a mini SQL here
```R
DT1 = data.table(x=c("a", "a", "b", "dt1"), y=1:4)
DT2 = data.table(x=c("a", "b", "dt2"), z=5:7)
setkey(DT1, x); setkey(DT2, x)
merge(DT1, DT2)

Key: <x>
        x     y     z
   <char> <int> <int>
1:      a     1     5
2:      a     2     5
3:      b     3     6
```
- What we just did is akin to an inner join
- The merging itself is based on all the possible combinations of matching x values
- This means that for `x ="a"` where DT1 has y = 1, 2 and DT2 has z = 5, we get, 
```
(a,1) x (a,5) -> (a,1,5)
(a,2) x (a,5) -> (a,2,5)
```
- We had only one b for both, and dt1 and dt2 were unique to their respective tables so they get dropped, since an inner join only takes values that exist in both tables
- We could have kept all the rows though if we used the `all` flag
```R
merge(DT1, DT2, all = TRUE)      # full outer join
merge(DT1, DT2, all.x = TRUE)   # left join
merge(DT1, DT2, all.y = TRUE)   # right join
```
## Reading from files
- Needless to say that reading data from disk into a data table is faster than a data frame, with the `fread` function
- It's actually 10 times faster to read with a data table than a data frame