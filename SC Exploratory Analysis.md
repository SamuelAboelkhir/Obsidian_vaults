---
tags:
- DS
MOC: Sciences
---
[[_0000 Home|Home]] | [[_0004 Sciences MOC |Back to Sciences MOC]] | [[SC Datascience index|Back to index]]

# Principles of Analytic Graphics
- A graph is meant to tell a story about the data, and it needs to follow a set of principles to be effective at this task
- Principle 1: Show Comparisons
	- The first principle is comparisons. A hypothesis is always relative to another competing hypothesis, so you should always ask, "compared to what?"
- Principle 2:  Show causality, mechanism, explanation, or structure
	- For example, what caused you to think about this question? or, if I'm saying that medicine A decreases hypertension, then why? why does it decrease hypertension? so I have to elaborate on the graph, maybe the medicine increases vascular dilation by increasing the levels of a certain protein, so I need to add it in the graph labels "protein level"
- Principle 3: Show multivariate data
	- Multivariate data here means more than 2 variables
	- This makes sense because the world is inherently multivariate, and not one single thing can drive a conclusion forward
	- Focusing on a single variable can cloud the truce, or twist it, like how focusing on the concentration on pollutants in the air and mortality rate, shows that as pollution increases, mortality decreases, which doesn't make much sense. Now add the season to the mix, and we see that the results were confounded, and adding the 4 seasons as a factor, got the relation to be positive, as it should be
- Principle 4: Integration of evidence
	- Every piece of evidence that you could use, completes the picture, so integrate everything that you have
- Principle 5: Describe and document your evidence
	- You should add credibility to your evidence by referencing your sources
- Principle 6: Content is king
	- The quality of the data being presented is the ultimate factor in whether or not the presentation succeeds
	- If the data is relevant and has high integrity, you're much more likely to succeed
## Exploratory Graphs
- These help us understand the data's properties, and find patterns within
- They help us suggest modeling strategies, and debug our analysis
- They're not however meant to communicate results yet
- We usually make these quickly, and in high quantities just to gather as much info as we can about our data
- When looking at your exploratory graphs, you still want to be asking questions about the data, you don't mindlessly go through them
- Histograms, boxplots, density plots, and barplots are all examples of simple summaries of the data
### Boxplots
- The box plot helps us visualize the five numbers summary, which is the result of the `summary` function, although in R we get an extra number, which is the mean
- For example, taking a random `rnorm(10000)`
```R
summary(x)
    Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
-3.88301 -0.71939 -0.01656 -0.04264  0.68215  2.87291
quantile(x)
         0%         25%         50%         75%        100% 
-3.88300624 -0.71938977 -0.01655652  0.68215266  2.87290519
boxplot(x, col = "blue")
```
![[Pasted image 20260504113329.png]]
- The boxplot shows the median along side the 1st and 3rd quantiles in the blue box (known as the interquartile range), and the whiskers, which are the top and bottom lines represent Q<sub>0</sub>, and Q<sub>1</sub>
- Past the whiskers are the outliers, which are the minimum and maximum values
- Here is the same boxplot with an `abline`, which helps us visualize a threshold of sorts
```R
abline(h = 1.5)
```
![[Pasted image 20260504114122.png]]
### Histogram
- This is an example histogram for `rnorm(10000)`
```R
x = rnorm(10000)
hist(x, col = "green", breaks = 100)
rug(x)
```
- ![[Pasted image 20260504112819.png]]
- The `rug` is responsible for adding the points that are below the histogram itself. It built upon the already existing plot created with `hist`
	- The rug plots all the points in the dataset under the histogram to help visualize where the data points lie exactly, and help visualize the outliers
- The breaks we added determines the number of bars that the histogram is going to have in the plot, which helps separate the points more, as a low number of bars would concatenate the present bars, like so
![[Pasted image 20260504113236.png]]
- Adding `abline` to the plot at the median (magenta color) and an x value of 1.5
- If we had a threshold of 1.5, and we didn't want our median to exceed it, then doing this can help us see how far away the median actually is from that value
```R
abline(v = 1.5, lwd = 2)
abline(v = median(x), col = "magenta", lwd = 4)
```
![[Pasted image 20260504114346.png]]
### Barplots
- Barplots are great for visualizing categorical data, and seeing how many rows falls under each category
```R
barplot(test)
barplot(test, col = "wheat", main = "Probills by status")
```
![[Pasted image 20260504114649.png]]
- Note that test is a table, meaning each data point is two dimensional with an X and Y value, which can be plotted as we see above
### Scatterplots
- The first 2-d plot that we're covering
- Since we're talking about 2-d data, here is a way of representing 2-d data in a boxplot
```R
boxplot(inhibitionzones ~ extractionmethod + species, data = tidyCollegeData, col = 'red')
```
![[Pasted image 20260504225119.png]]
- We can also do something similar with histograms by plotting multiple of them together
```R
par(mfrow = c(2,1), mar = c(4,4,2,1))
hist(cars$speed, col = "green")
hist(cars$dist, col = "blue")
```
![[Pasted image 20260505023332.png]]
- Finally, scatterplots
```R
with(airquality, plot(Ozone, Temp))
abline(h = 80, lwd = 2, lty = 2)
```
![[Pasted image 20260505024123.png]]
- The above graph indicates the presence of a positive correlation between higher temperatures and higher ozone values
- We can add an extra dimension by adding color
```R
with(airquality, plot(Ozone, Temp, col = Month))
```
![[Pasted image 20260505024356.png]]
- This shows a very logical thing, which is a change is that as temperature increases, the month is changing, or rather, as the months change, temperature increases, but ozone isn't affected so much by the change in month
	- We can see this because the color seems to remain the same horizontally as we move along the X axis, but changes as we move vertically as we move along the Y axis
- Another way of showing this is by using two plots
```R
with(subset(airquality, Month == "5"), plot(Ozone, Temp))
with(subset(airquality, Month == "6"), plot(Ozone, Temp))
```
![[Pasted image 20260505024736.png]]
- Again, the positive correlation between Temp and ozone remains the same, unaffected by the months
## What was the point?
- While I was mainly documenting different ways of doing graphs, I think these examples can still show the main idea, that these quick simple plots (the code wasn't very complicated) are very good for summarizing the data visually, and highlighting broader patterns within it
- Obviously the proper plot needs to be picked relevant to the available data though
## Plotting systems in R
- R has 3 different plotting systems
- Note that if you start a plot with one system, you need to stick to it, as the system can't be mixed
- The different systems use different graphics devices to generate their plots
- When making a plot, you should consider the following
	- Where will the plot be made? on screen? in a file
	- How will it be used?
		- Will be viewed on screen temporarily?
		- Will be presented on a web browser
		- Will it end up in a paper and might get printed?
		- Will it be used in a presentation?
	- Is there a large amount of data going in? or just a few points?
	- Does it need to be dynamically resized?
### Graphics devices
- A graphics device is basically any where you can make a plot appear
- These are things like X11, PDF, PostScript, PNG, the window on the computer, etc... which the base system uses
- Lattice for example uses a different system called grid, which produces what's known as Trellis graphics
- The `?Devices` help menu can show all the available graphics devices
- It's possible to pick a graphics device to generate the plot in
```R
pdf(file = "myplot.pdf")
with(faithful, plot(eruptions, waiting))
title(main = "Old Faithful Geyser data")
dev.off()
```
- It's very important to remember to close the device with `dev.off` otherwise, every plot you generate will keep writing to the pdf file you have opened
#### Vector based formats
- You normally want a pdf for things like line-type graphics, as it resizes well and is very portable
- SVGs are XML-based, and are good for animations and interactivity, as well as web-based plots
	- You don't really want a vector format plot though for plots with a lot of points as they can get very large due to every point having a piece of information tied to it
#### Bitmap formats
- PNGs are good for line drawings and images with solid colors, and are preferred for plots with many points as they can use lossless compression, and doesn't resize well
- JPEG is good for more natural scenes and photographs. It's also good for plots with multiple points
	- It doesn't resize well however, and uses lossy compression so it's not great for line drawings
- It's possible to open multiple graphics devices at the same time, but plotting can be done to only one at a time, which is the active one. `dev.cur` can tell you the active device, and every device gets an integer index
- `dev.set` takes an integer and can change the current active graphics device
- `dev.copy` can copy plots from one device to another, like making a plot on the screen device, and copying it to a file
- `dev.copy2pdf` is a special variant of the copy function specific to pdfs
- Do note that the copy may not look exactly like the original
```R
library(datasets)
with(faithful, plot(eruptions, waiting))
title(main = "Old Faithful Geyser data")
dev.copy(png, file = "geysersplot.png")
dev.off()
```
### The base plotting system
- This one offers the most control, where you can add elements to your plot one by one
- The plot functions are split into creation/generation functions, and annotation functions
- One draw back though is that if you add something, you can't remove it, you'd have to start over to get rid of it
- This is a great system for hand crafting a plot
```R
library(datasets)
data(cars)
with(cars, plot(speed, dist))
```
- You can pick your chosen graphics device, but if you don't the default one will be used
- The `plot` function has many methods that are dependent for the type of data, but if the data has no special attributes, then the default method will be used
	- The default means for example the open circle in scatterplot
- Some of the most important parameters in the base system's functions are
	- `pch`: the plotting symbol (default is the open circle)
	- `lty`: the line type. This one can be dashed, dotted, etc... but the default is solid
	- `lwd`: the line width
	- `col`: the plot's color. This one can be a number, string, or hex
		- The `colors` function gives us a vector of colors by name
	- `xlab`: the x-axis label
	- `ylab`: the y-axis label
- The `par` function is used to specify global graphics parameters that affects all the plots in an R session
	- `par` can take any of the previous parameters, plus the following
	- `las`: the orientation of the axis labels on the plot
	- `bg`: the background color
	- `mar`: the margin size. The margin's params are the 4 sides of the screen, and the order starts at the bottom going clock-wise
	- `oma`: the outer margin size (defaults to 0)
	- `mfrow`: number of plots per row, column (plots will be filled row-wise). Defaults to 1,1 meaning 1 plot per row/col
	- `mfcol`: same as `mfrow` but plots will be filled column-wise
- `plot`: by default makes a scatterplot, but can make other plots based on the class of the object being passed to it
- `lines`: adds lines to the plot, and takes 2 numeric vectors as coordinates, or a 2-column matrix
- `points`: adds points
- `text` adds text labels at the specified x, y coordinates
- `title`: adds annotations to x, y axis labels, title, subtitle, or the the outer margin
- `axis`: adds axis ticks/labels
#### Examples
- The following example shows how we can add a title to a pre-made plot
```R
with(airquality, plot(Wind, Ozone))
title(main = "Ozone and Wind in New York City")
```
![[Pasted image 20260505110442.png]]
- The following example shows how we can change the color on a subset of the plotted data after the plot has been made
```R
with(airquality, plot(Wind, Ozone, main = "Ozone and Wind in New York City"))
with(subset(airquality, Month == 5), points(Wind, Ozone, col = "blue"))
```
![[Pasted image 20260505110630.png]]
- The following is a more complicated example
- `type = "n"` is used to set up an empty plot, which will setup the plot it self with the title, but not actually plot the data
- We then showed the dots representing May's data in blue, and everything else in red
- Then we added a legend to the top right, with `pch = 1` for open circles, and added the colors of the legend and the legend's labels in order
```R
with(airquality, plot(Wind, Ozone, main = "Ozone and Wind in New York City", type = "n"))
with(subset(airquality, Month == 5), points(Wind, Ozone, col = "blue"))
with(subset(airquality, Month != 5), points(Wind, Ozone, col = "red")) 
legend("topright", pch = 1, col = c("blue", "red"), legend = c("May", "Other Months"))
```
![[Pasted image 20260505111021.png]]
- Now, we make a plot where we add a regression line
- The `lm` function was used to model the data, and will be covered later in the statistics course
- The main point is that we were able to add a modeled regression line the plot
```R
with(airquality, plot(Wind, Ozone, main = "Ozone and Wind in New York City", pch = 20))
model = lm(Ozone ~ Wind, airquality)
abline(model, lwd = 2)
```
![[Pasted image 20260505111353.png]]
- The following example is for multiple plots in the same panel
```R
par(mfrow = c(1, 2))
with(airquality, {
	plot(Wind, Ozone, main = "Ozone and Wind")
	plot(Solar.R, Ozone, main = "Ozone and Solar Radiation")
})
```
![[Pasted image 20260505111659.png]]
- We can have even more plots
- To be able to have this global label that covers all the whole panel of plots, we needed to make the outer margin bigger with `oma`
```R
par(mfrow = c(1, 3), mar = c(4, 4, 2, 1), oma = c(0, 0, 2, 0))
> with(airquality, {
    plot(Wind, Ozone, main = "Ozone and Wind")
    plot(Solar.R, Ozone, main = "Ozone and Solar Radiation")
    plot(Temp, Ozone, main = "Ozone and Temperature")
	mtext ("Ozone and Weather in New York City", outer = TRUE)
})
```
![[Pasted image 20260505111809.png]]
### The lattice system
- This system is very different in philosophy from the base system
- Instead of hand crafting the plot, you generate it with a single function
- You do have to pass a lot of arguments to a single plot though
- You also can't add anything to the plot once you're done
- It's good for generating a lot of plots very fast
```R
library(lattice)
state = data.frame(state.x77, region = state.region)
xyplot(Life.Exp ~ Income | region, data = state, layout = c(4,1))
```
- Unlike the base system, with lattice, we don't need to manually use par to adjust our panel to be able to add more plots
- Some of the most used functions with lattice are
	- `xyplot`: for scatterplots
	- `bwplot`: for boxplots (bw stands for box and whiskers)
	- `histogram`: for histograms
	- `stripplot`: like boxplot but with points
	- `dotplot`: plots dots on "violin strings"
	- `splom`: this is a scatterplot matrix (the lecture says "like `pairs` in the base system, as if we covered `pairs`)
	- `levelplot` and `contourplot`: for plotting "image" data
- Lattice functions require a formula for their first argument, which looks something like this
```R
xyplot(y ~ x | f * g, data)
```
- The `f` and `g` are conditioning variables, which are optional, and the `*` indicates an interaction between two variables
- The 2nd argument is the data source itself, and if one isn't provided, variables from scope will be used
#### Examples
```R
xyplot(y ~ x)
xyplot(Ozone ~ Wind, data = airquality
```
![[Pasted image 20260506104642.png]]
```R
airquality = transform(airquality, Month = factor(Month))
xyplot(Ozone ~ Wind | Month, data = airquality, layout = c(5, 1))
```
![[Pasted image 20260506104801.png]]
- `transform` can transform its first argument to a data frame if it's not one already, and then can do a mutation on said data frame, like how we changed the `Month` column to a factor
- While the base system plots to a graphics device directly, lattice instead returns an object of the class trellis, and then the print methods of lattice do the plotting job'
- The command line auto prints trellis objects which gives the impression of automatically writing to the graphics device
- This also means that the trellis object can be saved in the workspace
```R
p = xyplot(Ozone ~ Wind, data = airquality)
print(p)
```
#### Panel functions
- Lattice also has panel functions which controls what happens inside each panel of the plot
- It's also possible to write custom ones
```R
set.seed(10)
x = rnorm(100)
f = rep(0:1, each = 50)
y = x + f - f * x + rnorm(100, sd =0.5)
f = factor(f, labels = c("Group 1", "Group 2"))
xyplot(y ~ x | f, layout = c(2,1))
```
![[Pasted image 20260506224012.png]]
```R
xyplot(y ~ x | f, panel = function(x, y, ...) {
	panel.xyplot(x, y, ...)
	panel.abline(h = median(y), lty = 2)
})
```
![[Pasted image 20260506224118.png]]
```R
xyplot(y ~ x | f, panel = function(x, y, ...) {
	panel.xyplot(x, y, ...)
	panel.lmline(x, y, col = 2)
})
```
![[Pasted image 20260506224803.png]]

### The ggplot system
- A middle ground system between both systems, and pretty much the most comprehensive and beloved system
- Can increment a plot like the base system, but abstracts some of the very fine tuning required by the base system like lattice
- This system has a bunch of defaults, so it can be used like lattice, but also allows you to control every facet of the plot
```R
library(ggplot2)
data(mpg)
qplot(displ, hwy, data = mpg)
```
- It's nice to know that the `example` function can take any other function as an argument, and show examples of how to use it
	- It does this by running all the code in a function's "examples" section from R's online help topics
- ggplot implements what's known as the Grammer of Graphics "by Leland Wilkinson"
- The philosophy here, is that we compose a sentence in essence, describing each component of the plot
- This system builds upon R's base grid system
- Based on the course, `qplot` is the workhorse of ggplot2, however, it has sense been deprecated
- `qplot` stands for quick plot, and is meant to be similar to the base system's `plot`, and the reason for its deprecation, was to encourage users to use the main ggplot2 core function `ggplot` which can compose some much more complicated graphics
- Regardless, plots in ggplot2 are generally made up of 
	- aesthetics `aes` which include things like size, shape, and color
	- `geom_` which is where you indicate if you're plotting lines, or points, etc...
	- `facets` for conditional plots. Can be used to create multiple plots based on a factor in the data
	- `stats` statistical transformations such as binning, quantiles, and smoothing
	- `scales` the scale an aesthetic map uses, such as male = red, female = blue
	- `coordinate system`
#### Examples
- I'll mainly stick to `ggplot` since `qplot` is depricated
```R
ggplot(mpg, aes(displ, hwy, colour = drv)) + geom_point()
```
![[Pasted image 20260507012610.png]]
- Adding a smoother which shows the 95% confidence intervals
```R
ggplot(mpg, aes(displ, hwy)) + geom_point() + geom_smooth()
`geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```
![[Pasted image 20260507013214.png]]
- Creating a histogram
```R
ggplot(mpg, aes(hwy, fill = drv)) + geom_histogram()
`stat_bin()` using `bins = 30`. Pick better value `binwidth`.
```
![[Pasted image 20260507013449.png]]
- Using facets to generate multiple plots in the panel
- The variable on the left hand side of the tilde represents on the rows, and the one on the right is for the columns. If we use a `.` when we don't have a variable that specifies how many rows/cols we want, and using a factor variable will place as many rows/cols as there are levels to the factor
```R
ggplot(mpg, aes(displ, hwy)) + geom_point() + facet_grid(. ~ drv)
```
![[Pasted image 20260507013736.png]]
```R
ggplot(mpg, aes(hwy)) + geom_histogram(binwidth = 2) + facet_grid(drv ~ .)
```
![[Pasted image 20260507014051.png]]
## Hierarchical clustering
- Clustering basically groups related data points visually, and makes patterns in the data clearer
- This means that we need to be able to
	- Define what close is
	- Group the data
	- Visualize our grouping
	- Interpret our grouping
- To understand this better, imagine we had a matrix of blood pressure values and glucose levels
- Some of the data represents normal BP and normal glucose levels, some represent hypertension/hyperglycemia, and some represent hypertension/normal glucose
- Plotting the data from this matrix may highlight the normal group, and the elevated levels group, but elevated BP with normal glucose may drown out as more noise than anything else
- By clustering the data and grouping related data points together, all the groups can get highlighted more
	- Specifically, when the data is orthogonal, meaning that one value can be elevated with the other being low and vice versa, instead of them always affecting each other
### The agglomerative approach
- The algorithm for this approach is
	- Find the 2 closest things
	- Put them together (you now consider them as one thing)
	- Find the next closest thing
- This approach requires
	- A defined distance
	- A merging approach
- And it produces a tree called a dendogram showing how close things are to each other
- There a few distance metrics that we can consider to decide on closeness
	- Eucledian distance - Continuous
	- Correlation similarity - Continuous
	- Manhattan distance - Binary
- You should then pick a distance/similarity that's logical for your problem
#### Eucledian distance
- The eucledian distance follows the Pythagorean theorem of the right angled triangle
- This means that gives 2 sets of coordinates for 2 points ($X_1, Y_1$) and ($X_2, Y_2$), the distance between them should be $\sqrt{(X_2 - X_1)^2 + (Y_2 - Y_1)^2}$
- If we wanted to calculate the eucledian distance for multiple dimensions, we can easily add the coordinates from all the remaining dimensions as such $\sqrt{(X_2 - X_1)^2 + (Y_2 - Y_1)^2 + ... + (Z_2 - Z_1)^2}$ 
#### Manhattan distance
- Here, we simulate a grid, and to go from one point to another, we can't go in a straight line like with the eucledian distance, we have to follow a path, and we want to find the shortest path
- The distance is the absolute sum of all the coordinates that we have to pass by $|A_1 - A_2| + |Y_1 - Y_2| + ... + |Z_1 - Z_2|$, or we could say that it's the sum of the distance traveled in the X direction + the distance traveled in the Y direction
### Example
```R
set.seed(1234)
par(mar = c(0,0,0,0))
x = rnorm(12, mean = rep(1:3, each = 4), sd = 0.2)
y = rnorm(12, mean = rep(c(1,2,1), each = 4), sd = 0.2)
plot(x, y, col = "blue", pch = 19, cex = 2)
text(x + 0.05, y + 0.05, labels = as.character(1:12))
```
![[Pasted image 20260507220904.png]]
- The first thing we need to do to cluster these points, is calculate the pair-wise distance between the points
```R
dataFrame = data.frame(x = x, y = y)
dist(dataFrame)
            1          2          3          4          5          6          7          8
2  0.34120511                                                                             
3  0.57493739 0.24102750                                                                  
4  0.26381786 0.52578819 0.71861759                                                       
5  1.69424700 1.35818182 1.11952883 1.80666768                                            
6  1.65812902 1.31960442 1.08338841 1.78081321 0.08150268                                 
7  1.49823399 1.16620981 0.92568723 1.60131659 0.21110433 0.21666557                      
8  1.99149025 1.69093111 1.45648906 2.02849490 0.61704200 0.69791931 0.65062566           
9  2.13629539 1.83167669 1.67835968 2.35675598 1.18349654 1.11500116 1.28582631 1.76460709
10 2.06419586 1.76999236 1.63109790 2.29239480 1.23847877 1.16550201 1.32063059 1.83517785
11 2.14702468 1.85183204 1.71074417 2.37461984 1.28153948 1.21077373 1.37369662 1.86999431
12 2.05664233 1.74662555 1.58658782 2.27232243 1.07700974 1.00777231 1.17740375 1.66223814
            9         10         11
2                                  
3                                  
4                                  
5                                  
6                                  
7                                  
8                                  
9                                  
10 0.14090406                      
11 0.11624471 0.08317570           
12 0.10848966 0.19128645 0.20802789
```
- `dist` takes a matrix or data frame and calculates the distance between all the different rows in the data frame, returning a distance matrix
- We know from the distance matrix that points five and 6 are closest to each other, so we start by grouping those 2 into 1 point
	- The coordinate of the new point can be calculated based on either the complete distance between the two points, or the average of their X coordinates and their Y coordinates
	- Once we get to a point where the merged points are basically clusters of points, and we want to measure the distance between two clusters, the choice of the merging method matters more, because in the complete distance approach, we take the furthest point of each cluster and calculate the distance between them, while with the average method means that we take their center of gravity, or as we said before, the average of all of the points's X values, and the same for the Y values
	- There is no right or wrong way of merging really, but both techniques do give different results, and checking both of them out is usually advantagous
- We then do the same for points 10 and 11, and so on, and eventually, we will get the dendogram tree
```R
distxy = dist(dataFrame)
hClustering = hClust(distxy)
plot(hClustering)
```
![[Pasted image 20260507221825.png]]
- We can then cut the tree at any height that we want, and the number of lines that we run into would indicate the number of clusters at that level
	- For example, cutting at 2.0 nets 2 clusters, while cutting at 1.0 nets 3 clusters
### Heatmap
- This is a nice function for visualizing matrix or tabular data, especially if it's large
- This function does a hierarchical clustering analysis on both the rows, and the columns of the matrix or table
- So the rows of a table are our observations, and the columns are sets of those observations, and the heatmap will organize them into blocks
```R
set.seed(143)
dataMatrix = as.matrix(dataFrame)[sample(1:12),]
heatmap(dataMatrix)
```
![[Pasted image 20260507232920.png]]
+ The picture may be unstable
	- Change a few points
	- Have different missing values
	- Pick a different distance
	- Change the merging strategy
	- Change the scale of points for one variable
+ But it is deterministic
+ Choosing where to cut isn't always obvious
+ Should be primarily used for exploration
## K-means clustering
- In this clustering technique, we first define how many clusters we want to have
- Each cluster or group will have a centroid that the points will be assigned to
- Every point will belong to the cluster of the nearest centroid
- The centroids are recalculated a certain amount of time, and clusters are regenerated accordingly
- This approach requires
	- A defined distance metric again
	- A number of clusters
	- And an initial guess as to cluster centroids
- It produces a final estimate of cluster centroids, and assigns points to them
- The centroids are recalculated based on the mean of the original clusters that get generated, such as they get closer and closer to the centers of the clusters
- Essentially, the clusters already exist in the data, and the centroids are trying to find the centers of those clusters
```R
kmeansObj = kmeans(dataFrame, centers = 3)
names(kmeansObj)
[1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss" "betweenss"   
[7] "size"         "iter"         "ifault"

KmeansObj$cluster
 [1] 3 3 3 3 2 2 2 2 1 1 1 1
```
```R
par(mar = rep(0.2, 4))
plot(x, y, col = kmeansObj$cluster, pch = 19, cex =2)
points(kmeansObj$centers, col = 1:3, pch = 3, cex = 3, lwd = 3)
```
![[Pasted image 20260508023845.png]]
```R
set.seed(1234)
dataMatrix = as.matrix(dataFrame)[sample(1:12),]
kmeansObj2 = kmeans(dataMatrix, centers = 3)
par(mfrow = c(1, 2), mar = c(2, 4, 0.1, 0.1))
image(t(dataMatrix)[, nrow(dataMatrix):1], yaxt = "n")
image(t(dataMatrix)[, order(kmeansObj$cluster)], yaxt = "n")
```
![[Pasted image 20260508024230.png]]
## Principal Components Analysis and Singular Value Decomposition
### The concept
- SVD is explained as
	- If X is a matrix with each variable in a column and each observation in a row, then the SVD is a matrix decomposition, meaning the matrix is used to create 3 other matrix based on the original, which are $X = UDV^T$ (the T means transposed, this is a transposed matrix)
	- The columns of the U matrix are orthogonal (uncorrelated/independent of each other) left singular vectors, the columns of V are also orthogonal, right singular vectors, and D is a diagonal matrix of singular values
- PCA is explained as
	- The principal components are equal to the right singular values if first scale (subtract the mean, divide by the standard deviation) the variables, then run SVD on the normalized matrix
- in UDV, the V (the right singular vectors) corresponds to the principal components of PCA, so the first right singular vector for example is PC1 the first principle component, and the 2nd is PC2 and so on
- The left singular vectors U relates to how the data points (rows) project onto these components. This is relevant to how PCs are created in the first place
	- Imagine you have a big box of mixed colored beads (your data). 
	- SVD helps you find the main colors (patterns) in the beads. 
	- The right singular vectors tell you about the main color patterns across the bead types (columns), which is like the principal components. 
	- The left singular vectors tell you how much each bead (row) shows those colors.
	- The singular values D explain how much the combination of U and V, the rows and columns, contribute to the data's variance
#### PCA
- PCA is very tough to grasp, but the important idea here is that when talking about principle components, a PC tries to figure out what group of variables cause the highest amount of variance in the data, not the other way around. It looks for patterns in the data, like how youtube for example shows its recommendations, based on your watching habits
- This means that, if for example we have a group of students, those students will have similarities and differences. PCA doesn't care about the similarities, it doesn't care that all students are roughly the same height and have similar hair color, it cares about them having varying interests
- For example the students study different subjects, some biology, some chemistry, some math, etc...
- Students who study biology are more likely to pursue a career in medicine, and to study topics like chemistry and microbiology, and are less likely to study math
- Students who like math, and those that like physics may gravitate towards CS
- In this way, we see that CS brought together students from different fields, there was a similarity between them in that they both liked CS, and that they both disliked biology
- So far, biology is the biggest candidate to be PC1, as it's causing the most amount of variance between the students when considering interest, and career prospects
- But PCA requires continuous numeric data, it doesn't work with categorical data, so, students would need to be assigned some sort of score based on their interests, such as biology being 0, and CS being 10 (assuming bioinformatics doesn't exist) and physics students could be assigned 6, as they may like to go into a niche field like biophysics, but they're more likely to pick CS, while chemistry students are the opposite, and can be assigned 4, but math students will get a 7 because they don't like bio-statistics or something, and so on
- Taking youtube as an example, PC1 would be "these topics cause the highest degree of variance among the viewers" so I'd look at them and see they constitute engineering and science content, which isn't exactly entertainment, so because most people are on youtube for entertainment, those that are there for education have very different watching habits, and that's how I'd know that "ok so if a user likes linux and rust videos, maybe he'll enjoy Go and R, and he likes R, because R is related to datascience then maybe he likes that too, and if he watches the datascience stuff, well now he might like bioinformatics because it's under the same category as datascience, then that leads to biology" and the rabbit whole continues until the user stops responding positively to youtube's recommendations. It's basically that there is always a door that's ajar that would lead to a different yet related category of content
#### SVD
- What SVD does is similar to passing white light through a prism, where here, SVD is that prism, and white light is our data being broke down into 3 colors, which are U, D, and V
- Imagine we are looking at different chemical compounds (rows), and we have specific data about them like weight, solubility, toxicity, and so on (columns)
	- "U" describes the rows, saying how each compound relates to the hidden themes that we're trying to find in the data
		- For example, saying that a compound is behaves like 80% like a solid and 20% like a gas
	- "D" or sigma is the importance of each discovered pattern
	- "V" represents the columns, which tells us how each variable relates to the theme or pattern
- SVD helps us reduce the noise in the data by defining the most important patterns via the D matrix
- It's also good for compression since the most significant parts of the data are good enough for reconstructing it
- SVD needs to run on centered data, where the mean is zero, so normalization is important
- Running SVD will net us the PCs as the D matrix
- From the compounds example, let's say we're looking at different organic acids, ad we notice there is a big difference between them related to solubility
	- D (The Magnitude): This is your "Eureaka!" moment. You see the first singular value is massive (99%). This tells you, "There is one major story being told by these 100 columns of data."
	- V (The Recipe): You look at the first column of $V$. You see high numbers for "Solubility," "pH," and "Temperature," and zeros for everything else. This tells you what the story is. The pattern is a "Solubility/Acidity" profile.
	- U (The Map): You look at $U$. Each compound gets a score. Compounds with a high score here are the ones that are highly soluble/acidic. Those with a low score are the "outliers" of that group.
- In SVD, the Pattern is often called a Latent Variable (Hidden Variable). You can't measure "Solubility Profile" directly with one sensor; you measure solubility, pH, and temp separately. SVD "distills" those into the single pattern.
### Examples
- When plotting, the U and V columns will both plot their data on the x axis, meaning that rows and columns will both be on the x axis
![[Pasted image 20260511010814.png]]
```R
mat
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    2    5    7

svd(mat)
$d
[1] 9.5899624 0.1806108

$u
           [,1]       [,2]
[1,] -0.3897782 -0.9209087
[2,] -0.9209087  0.3897782

$v
           [,1]       [,2]
[1,] -0.2327012 -0.7826345
[2,] -0.5614308  0.5928424
[3,] -0.7941320 -0.1897921
```
- In the above data, we see how SVD decomposed the original matrix "mat" into the 3 matrices UDV
- This operation is actually reversible
- If we have "diag", "matu", and "matv" being the sorted D, U, V matrices respectively
```R
matu %*% diag %*% t(matv)
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    2    5    7
```
- For PCA, we have to scale the matrix first (subtract the column means and divide by the result)
- To do this, we can use the `scale` function
```R
svd(scale(mat))
$d
[1] 1.732051 0.000000

$u
           [,1]      [,2]
[1,] -0.7071068 0.7071068
[2,]  0.7071068 0.7071068

$v
          [,1]       [,2]
[1,] 0.5773503 -0.5773503
[2,] 0.5773503  0.7886751
[3,] 0.5773503 -0.2113249
```
- Now we can run the PCA function `prcomp`
```R
prcomp(scale(mat))
Standard deviations (1, .., p=2):
[1] 1.732051 0.000000

Rotation (n x k) = (3 x 2):
           PC1        PC2
[1,] 0.5773503 -0.5773503
[2,] 0.5773503  0.7886751
[3,] 0.5773503 -0.2113249
```
- Note how the values of the principle components are identical to that of the V column from the SVD function
- The first LEFT singular vector is associated with the row means
- The following plot shows how that first vector shows the pattern in this random matrix (not the one we're working with) clearly. The other right singular values don't show it as clearly
- Same goes for the scatterplot on the right which shows the first RIGHT singular vector associated with the column means
![[Pasted image 20260511023439.png]]
- The D matrix is also knows as "variance explained"
- Its values are basically weights for the U and V matrices accounting for the variation in the data
- These are given in decreasing order, from highest to lowest
- If we plot the D matrix's entries as values and percentages
![[Pasted image 20260511024019.png]]
- We can see that the first value in D accounts for 40% of the variation on its own, which also means that PC1 accounts for that much variation
- This also explains why the first columns in U and V showed the distinctive patters in the rows and means so clearly
- The next plot is going to be one-dimensional
```R
head(constantMatrix)
     [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
[1,]    0    0    0    0    0    1    1    1    1     1
[2,]    0    0    0    0    0    1    1    1    1     1
[3,]    0    0    0    0    0    1    1    1    1     1
[4,]    0    0    0    0    0    1    1    1    1     1
[5,]    0    0    0    0    0    1    1    1    1     1
[6,]    0    0    0    0    0    1    1    1    1     1
```
![[Pasted image 20260511024503.png]]
- Essentially, the data here has 10 columns where 5 are all 0 and 5 are all 1
- This means the data is one-dimensional, as we only have one piece of information, which is which column an entry is in, and that alone determines its value
- Next, is a matrix with 2 patterns
	- Pattern 1: if the first have of the columns have rows with the value `n`, then the 2nd half has `n+5`
	- Pattern 2: 50% of the time, the even numbered columns will have an additional +5
![[Pasted image 20260511025000.png]]
- The plots show the 2 patterns quite clearly, with the middle plot showing the `n` and `n+5` pattern, and the right plot showing the alternating pattern of the even numbered columns
- These two plots are the true pattern that we added, but now we want to see what SVD will generate
![[Pasted image 20260511025511.png]]
- These are the first two right singular vectors, and we can see, especially in the middle one, that both patterns are apparent, as values are alternating up and down, and the 2nd five columns have higher values than the first 5
- Next, the values of the variance explained matrix shows us how much of the variance each component explains
- The first component explains over 50% of the variance, and the 2nd one ~18%
- Finally, lets talk about compression, taking this image as an example
![[Pasted image 20260511030314.png]]
- This creepy image of a face, is stored in a 32 x 32 matrix, which we can compress using SVD
- Checking the D matrix, we can see the most significant components here
![[Pasted image 20260511030439.png]]
- This shows that most of the variation in this matrix is explained by the first 10 components, so what if we try to reconstruct the image with only those components?
```R
a1 = (svd1$u[,1] * svd1$d[1]) %*% t(svd1$v[,1])
```
- This is an R specific note, but here because we got a single value from the D matrix, which is literally a constant, we have to use the normal multiplication operator `*` in conjunction with the matrix multiplication operator `%*%`
- Now using the course provided function `myImage` to see the result
```R
function(iname){
  par(mfrow=c(1,1))
  par(mar=c(4,5,4,5))
  image(t(iname)[,nrow(iname):1])
}

myImage(a1)
```
![[Pasted image 20260511030941.png]]
- This image was created using only the first component, now we will try with two components
```R
a2 = svd1$u[,1:2] %*% diag(svd1$d[1:2]) %*% t(svd1$v[,1:2])
```
- This time we use the `diag` function to make sure D is a diagonal matrix![[Pasted image 20260511031331.png]]
- The image is getting shaped a bit more
- Now we will use 5 components
```R
myImage(svd1$u[,1:5] %*% diag(svd1$d[1:5]) %*% t(svd1$v[,1:5]))
```
![[Pasted image 20260511031505.png]]
- The fact that it's a face is very apparent now, but finally, the image with 10 components
![[Pasted image 20260511031612.png]]
- This goes to prove the point, that those 10 components did capture the essence of the image
- Some closing thoughts from swirl
	- First, again, scale. Make sure to take note on the scale of the different variables, and make sure that they all have consistent units
	- Second, the 2-pattern example shows that SVD and PCA can mix patterns together, so we need to be meticulous when trying to discern all the present patterns
	- Third, we should always remember that SVD and PCA can't handle missing data at all, and that we must always resolve NA values before using them, via techniques such as imputing the data
		- Use the `impute` library to impute missing values by the k nearest neighbor to that value/row
		- If k is five, `impute.knn` will take the 5 nearest rows to the missing one and impute the data in the missing row with the average of the other five
## Working with colors
- The `colorRamp` function can take a array of colors, and returns a function that takes values between 0 and 1 representing the extremes of the color palette
```R
pal = colorRamp(c("red", "blue"))
pal(0)
     [,1] [,2] [,3]
[1,]  255    0    0
pal(1)
     [,1] [,2] [,3]
[1,]    0    0  255
```
- `colorRampPalette` is very similar, but its returned function will take an int argument and return a vector of colors interpolating the palette
```R
pal2 = colorRampPalette(c("red", "blue"))
pal2(1)
[1] "#FF0000"
pal2(2)
[1] "#FF0000" "#0000FF"
```
- To get an actual gradient from the palette, we need to pass a sequence of numbers
- Since the usual shorthand method for vector creation increments by 1, we will use `seq` instead, which takes the range of the int vector as its first two arguments, and then the length of the vector
```R
pal(seq(0, 1, len = 10))
           [,1] [,2]      [,3]
 [1,] 255.00000    0   0.00000
 [2,] 226.66667    0  28.33333
 [3,] 198.33333    0  56.66667
 [4,] 170.00000    0  85.00000
 [5,] 141.66667    0 113.33333
 [6,] 113.33333    0 141.66667
 [7,]  85.00000    0 170.00000
 [8,]  56.66667    0 198.33333
 [9,]  28.33333    0 226.66667
[10,]   0.00000    0 255.00000
```
- We also have the `RColorBrewer`package which can be used for generating color palettes of three different types
	- Sequential: for data ordered low to high (numerical or continuous for example)
	- Diverging: for data that diverges from the mean positively or negatively
	- Qualitative: for factors or categorical data
- The return output from this package can then be passed to `colorRamp` and/or `colorRampPalette`
```R
cols = brewer.pal(3, "BuGn")
pal = colorRampPalette(cols)
image(volcano, col = pal(20))
```
![[Pasted image 20260512020930.png]]
- Another function that uses brewer is `smoothScatter`
```R
x = rnorm(10000)
y = rnorm(10000)
smoothScatter(x, y)
```
![[Pasted image 20260512021050.png]]
- This function is good when you need to make a scatter plot with a ton of overlapping points
- `smoothScatter` will create a 2D histogram of the data, and will then plot it using the brewer color package, by making higher columns darker in color and lower one lighter, hence making the density of the points clearer
- Also, this is what the plot would look like without the colors
```R
plot(x,y, pch =19)
```
![[Pasted image 20260512022024.png]]
- We also have the `rgb` function, which returns a hexadecimal representation of the color that we want, and can also take a 4th argument, which is a number between 0 and 1 known as the alpha that represents transparency
- This is also another way for manually creating a smooth scatter type plot, which is essentially a 2D histogram
```R
plot(x,y, pch =19, col=rgb(0,0,0,0.2))
```
![[Pasted image 20260512022131.png]]
## Case studies
### Samsung data
- This dataset includes data collected from samsung smart phones, as they include accelerometers and gyros that allows them to gather metrics related to different activities, such as walking and standing and so on
- During this study we stuck to just the first subject
- Now looking at the average acceleration of the subject in both the X and Y directions
```R
load("data/samsungData.rda")
sub1 = subset(samsungData, subject == 1)
g1 = ggplot(sub1, aes(1:length(`tBodyAcc-mean()-X`), `tBodyAcc-mean()-X`, colour = activity)) + geom_point() + xlab("Index")
g2 = ggplot(sub1, aes(1:length(`tBodyAcc-mean()-Y`), `tBodyAcc-mean()-Y`, colour = activity)) + geom_point() + xlab("Index")
ggarrange(g1, g2)
```
![[Pasted image 20260514235857.png]]
- We can also try clustering the first 3 columns of the data to cluster it based on mean acceleration in the X Y Z directions
```R
source("./myplclust.R")
distanceMatrix = dist(sub1[, 1:3])
hclustering = hclust(distanceMatrix)
myplclust(hclustering, lab.col = as.numeric(factor(sub1$activity)))
```
![[Pasted image 20260515000437.png]]
- We see that the data isn't very well clustered as the mean acceleration isn't enough to differentiate it
- Next we can take a look at the max acceleration in the X and Y directions
```R
g1 = ggplot(sub1, aes(1:length(`tBodyAcc-max()-X`), `tBodyAcc-max()-X`, color = activity)) + geom_point() + xlab("Index")
g2 = ggplot(sub1, aes(1:length(`tBodyAcc-max()-Y`), `tBodyAcc-max()-Y`, color = activity)) + geom_point() + xlab("Index")
ggarrange(g1, g2)
```
![[Pasted image 20260515000934.png]]
- Again, and as expected, we see a big distinction between dynamic and static activities, which tells us that maximum acceleration can be a predictor of this sort of activities
- We can do the same and cluster the data based on the 3 max acceleration columns
```R
distanceMatrix = dist(sub1[, 10:12])
hclustering = hclust(distanceMatrix)
myplclust(hclustering, lab.col = as.numeric(factor(sub1$activity)))
```
![[Pasted image 20260515001401.png]]
- So unlike last time with when we clustered by the mean, this time we have 2 distinct clusters for dynamic and static activities
- However, going deeper in the clusters still doesn't tell us much as the data is still kinda jumbled
- So the next thing we can try is an SVD analysis, but we had to remove the last two columns as these were the activities and subjects
```R
svd1 = svd(scale(sub1[, -c(562, 563)]))
g1 = ggplot(as.data.frame(svd1$u), aes(1:length(V1), V1, color = sub1$activity)) + geom_point() + xlab("Index")
g2 = ggplot(as.data.frame(svd1$u), aes(1:length(V1), V2, color = sub1$activity)) + geom_point() + xlab("Index")
```
![[Pasted image 20260515002803.png]]
- So here we looked at the first and 2nd left singular vectors (the rows most affected by the variance) where the first one again showed the difference between the activities quite well, although the 2nd didn't really show the separation as much
- But an important question here is, which feature is the maximum contributor in the 2nd left singular vector then? The one contributing the most to the variance shown here
```R
ggplot(as.data.frame(svd1$v), aes(1:length(V2), V2)) + geom_point() + xlab("Index")
```
![[Pasted image 20260515004120.png]]
- We can try to cluster the data again adding the maximum contributor's column to the maximum acceleration columns
```R
maxContrib = which.max(svd1$v[, 2])
distanceMatrix = dist(sub1[, c(10:12, maxContrib)])
hclustering = hclust(distanceMatrix)
myplclust(hclustering, lab.col = as.numeric(factor(sub1$activity)))
```
![[Pasted image 20260515004450.png]]
- The maximum contributor helped separate the moving activities a bit more, but didn't help much with the static activities
- The maximum contributor was actually `fBodyAcc.meanFreq...Z` so the mean body acceleration in the Z direction in the frequency domain
- So that seems to be as far as we can get with hierarchical clustering, but what about kmeans?
- Now, kmeans clustering is actually a stochastic technique, meaning that it has a degree of randomness and gives non-deterministic outputs, due to it having to choose a starting point at random
- We can also set the `nstart` argument to a value higher than one, making kmeans re-run a few times, which can get a better clustering
- So the output below will be different each time this code is run
```R
kClust = kmeans(sub1[, -c(562, 563)], centers = 6)
table(kClust$cluster, sub1$activity)
   
    laying sitting standing walk walkdown walkup
  1      9       2        0    0        0      0
  2     24      33       47    0        0      0
  3      0       0        0   95        0      0
  4      0       0        0    0        0     53
  5     17      12        6    0        0      0
  6      0       0        0    0       49      0
```
- Regardless though, the clustering itself is still informative, as we see that walking has a cluster all by itself, but cluster two is a combination of the 3 idle activities, laying sitting and standing
- The dynamic activities sure are better separated than the idle/static ones
- Lets try again with 100 `nstart`
```R
kClust = kmeans(sub1[, -c(562, 563)], centers = 6, nstart = 100)
table(kClust$cluster, sub1$activity)
   
    laying sitting standing walk walkdown walkup
  1     29       0        0    0        0      0
  2      0      37       51    0        0      0
  3      0       0        0    0       49      0
  4      0       0        0   95        0      0
  5      3       0        0    0        0     53
  6     18      10        2    0        0      0
```
- The results are actually fairly similar, with idle activities not separating very well, but it looks a little better
- So if we can identify the features/columns that have a higher effect on the clustering of those idle activities, we may be able to better understand what features are more important for classifying people performing these activities
```R
plot(kClust$center[1, 1:10], pch = 19, ylab = "Cluster Center", xlab="")
```
![[Pasted image 20260515005913.png]]
- Plotting the first cluster's center (this one only had point from the laying activity) shows that the first 3 features had a positive effect on the center of the cluster's position, while the other features had nearly no effect, where the first 3 are the mean body acceleration
```R
plot(kClust$center[4, 1:10], pch = 19, ylab = "Cluster Center", xlab="")
```
![[Pasted image 20260515010138.png]]
- Next up is the walking, which has a lot more interesting values for also max acceleration
- And that's one of the main use cases of checking the cluster centers, which is to figure out which features mostly affect that cluster, which itself represents an activity
### Airpollution
- The EPA (Environmental Protection Agency) is an organization concerned with the environment in the USA
- The collect all sorts of environmental data, including air quality data
- In the US, there is a legislation known as the "Clean Air Act" which aims to maintain the levels of particulate matter within an acceptable level
- So lets see throw the EPA's data, if this legislation has been successful, by measuring the levels of PM25 (in microgram per cubic meter $um^3$) between two different time periods
```R
pm25_1999 = read.table("./data/Airpollution/RD_501_88101_1999-0.txt", sep = "|", header = FALSE, na.strings = "", comment.char = "#")
pm25_2012 = read.table("./data/Airpollution/RD_501_88101_2012-0.txt", sep = "|", header = FALSE, na.strings = "", comment.char = "#")
cnames = readLines("./data/Airpollution/RD_501_88101_1999-0.txt", 1)
cnames = strsplit(cnames, split = "|", fixed = TRUE)
names(pm25_1999) = make.names(cnames[[1]])
names(pm25_2012) = make.names(cnames[[1]])
```
- After loading the data we can start inspecting it
- We can start with the sample values from the 1999 set
```R
x_1999 = pm25_1999$Sample.Value
class(x_1999)
[1] "numeric"
str(x_1999)
 num [1:117421] NA NA NA 8.84 14.92 ...
summary(x_1999)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.     NAs 
   0.00    7.20   11.50   13.74   17.90  157.10   13217 
mean(is.na(x_1999))
[1] 0.1125608
```
- This dataset has about 11% missing values in the samples column, normally due to missed readings on certain days, as the data was being collected daily
- Whether or not the NAs can cause a problem or not in the course of the analysis is dependent on the question we're trying to answer, so it's not always a problem
- Now lets check the values for the 2012 set
```R
x_2012 = pm25_2012$Sample.Value
summary(x_2012)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.     NAs 
-10.000   4.000   7.633   9.140  12.000 908.970   73133
mean(is.na(x_2012))
[1] 0.05607125
```
- And the median this time is 7.6 as opposed to the previous 11.5 suggesting that the levels of particulate matter on average are dropping, however, we can also see that that maximum read value was significantly higher at 909 as opposed to 157.1
- The amount of missing values is also lower at ~5%
- Now lets start plotting the data, trying a boxplot first
```R
boxplot(x_1999, x_2012)
```
![[Pasted image 20260515110930.png]]
- We can see that the data has quite a lot of right side skew due to the super high max values
- We can take the log of the data in order to address the skew problem
```R
boxplot(log10(x_1999), log10(x_2012))
```
![[Pasted image 20260515111704.png]]
- One odd observation we made with the summary, is that it included negative values, which is unusual as PM is measured by passing airflow through a filter, and weighing the mass of the PM caught on that filter, meaning that negative values shouldn't be possible
- So next, lets take a look at how many negative values even are there in the 2012 set
```R
negative = x_2012 < 0
sum(negative, na.rm = TRUE)
[1] 26474
mean(negative, na.rm = TRUE)
[1] 0.0215034
```
- So it's actually just ~2% of the data that's negative, which is still weird, but these measurement errors are not enough to bother us at the moment
- Next we wanna look at the dates of the 2012 data
```R
dates = pm25_2012$Date
str(dates)
 int [1:1304287] 20120101 20120104 20120107 20120110 20120113 20120116 20120119 20120122 20120125 20120128 ...
```
- The dates are all coded as integers, and seem to be in a year, month, day format (eww the American format)
- We can do a quick transformation to change them to a normal date format
```R
dates = as.Date(as.character(dates), "%Y%m%d")
str(dates)
 Date[1:1304287], format: "2012-01-01" "2012-01-04" "2012-01-07" "2012-01-10" "2012-01-13" "2012-01-16" ...
```
- Now we have proper dates, and we can look at a histogram of the sampling dates by month
```R
hist(dates, "month")
```
![[Pasted image 20260516005833.png]]
- From the plot, we can see that most of the sampling happened near the beginning of the year, but when were the negative values collected?
```R
hist(dates[negative], "month")
```
![[Pasted image 20260516005953.png]]
- Well, the seem to have occurred mainly in the beginning of the year too, but there was also a spike near June
- PM25 tends to be low in the winter and high in the summer, so one hypothesis is that that may be related to the measurement errors
- So, so far, we have been analyzing the change in PM25 in the entire US, but what if we pick one monitor in one state to follow instead?
- Picking one monitor also allows us to control for the fact that different monitors came and went between 1999 and 2012, so we will be searching for one consistent monitor within New York
```R
site_1999 = unique(subset(pm25_1999, State.Code == 36, c(County.Code, Site.ID)))
site_2012 = unique(subset(pm25_2012, State.Code == 36, c(County.Code, Site.ID)))
head(site_1999)
      County.Code Site.ID
65873           1       5
65995           1      12
66056           5      73
66075           5      80
66136           5      83
66197           5     110
```
- We will use this subset to create a new variable that is a mix of the county code and site id
```R
site_1999 = paste(site_1999[,1], site_1999[,2], sep=".")
site_2012 = paste(site_2012[,1], site_2012[,2], sep=".")
str(site_1999)
 chr [1:33] "1.5" "1.12" "5.73" "5.80" "5.83" "5.110" "13.11" "27.1004" "29.2" "29.5" ...
```
- It seems that there were only 18 monitors back in 1999 as opposed to 33 in 2012
- Now, the reason we did this, is because we're still looking for monitors that existed in both data sets, so we're looking for common combinations of `county.code/site.ID`
- For that we can use the `intersect` function
```R
both = intersect(site_1999, site_2012)
both
 [1] "1.5"     "1.12"    "5.80"    "13.11"   "29.5"    "31.3"    "63.2008" "67.1015" "85.55"  
[10] "101.3"
```
- So there are 10 common monitors which exist in both data sets in New York
- Next up, we want a monitor with a lot of observations
- To do this, we can first add the new `county.site` variable that we created to both datasets, then subset both sets by the New York state code, and values of the `county.site` that are in `both`
```R
pm25_1999$county.site = with(pm25_1999, paste(County.Code, Site.ID, sep = "."))
pm25_2012$county.site = with(pm25_2012, paste(County.Code, Site.ID, sep = "."))
cnt_1999 = subset(pm25_1999, State.Code == 36 & county.site %in% both)
cnt_2012 = subset(pm25_2012, State.Code == 36 & county.site %in% both)
```
- Now that we're all set, it's time to see the number of observations per common monitor in New York
```R
sapply(split(cnt_1999, cnt_1999$county.site), nrow)
   1.12     1.5   101.3   13.11    29.5    31.3    5.80 63.2008 67.1015   85.55 
     61     122     152      61      61     183      61     122     122       7 
sapply(split(cnt_2012, cnt_2012$county.site), nrow)
   1.12     1.5   101.3   13.11    29.5    31.3    5.80 63.2008 67.1015   85.55 
     31      64      31      31      33      15      31      30      31      31
```
- What we did above was split the data frame, into multiple frames by the county site, and then count the number of rows in each using an `sapply` to get just the `nrow`s
- We ended up picking country 63 monitor 2008 to check their PM trends
```R
pm25_1999sub = subset(pm25_1999, State.Code == 36 & County.Code == 63 & Site.ID == 2008)
pm25_2012sub = subset(pm25_2012, State.Code == 36 & County.Code == 63 & Site.ID == 2008)
dim(pm25_1999sub)
[1] 122  29
dim(pm25_2012sub)
[1] 30 29
# We lierally could have used the new variable with made specifically for this, which would have been easier, but I guess professor Peng forgot about it?
# subset(pm25_2012, State.Code == 36 & county.site == 63.2008)
```
- It is now time to create a time series plot using the data to visualize how PM levels have been increasing and decreasing with time
```R
dates_1999 = pm25_1999sub$Date
x_1999sub = pm25_1999sub$Sample.Value
dates_2012 = pm25_2012sub$Date
x_2012sub = pm25_2012sub$Sample.Value
dates_1999 = as.Date(as.character(dates_1999), "%Y%m%d")
dates_2012 = as.Date(as.character(dates_2012), "%Y%m%d")
par(mfrow=c(1,2), mar = c(4, 4, 2, 1))
plot(dates_1999, x_1999sub, pch = 20)
abline(h = median(x_1999sub, na.rm=T))
plot(dates_2012, x_2012sub, pch = 20)
abline(h = median(x_2012sub, na.rm=T))
```
![[Pasted image 20260516225006.png]]
- We can see that the 1999 monitor only started in July, and that the median for 2012 is lower than 1999. However, the plot is kinda misleading, because the range for the y axis is different in each plot, making it appear as if 2012 had a higher median
- We can remedy this by unifying the range of the y axis for both of them using the `range` function
```R
rng = range(x_1999sub, x_2012sub, na.rm=T)
rng
[1]  3.0 40.1
plot(dates_1999, x_1999sub, pch = 20, ylim = rng)
abline(h = median(x_1999sub, na.rm=T))
plot(dates_2012, x_2012sub, pch = 20, ylim = rng)
abline(h = median(x_2012sub, na.rm=T))
```
![[Pasted image 20260517121541.png]]
- This is much better, and it even highlights the reduction in the spread of the data, which means that not just the average levels went down, but also the extreme values, so the spikes in pollution levels decreased as well
- Now, we have looked at two different extremes so far, the whole country, and a single monitor, but it would be more useful to look at a whole state, or county
- This is also important because regulation changes happen on a state level, so the changes in PM25 level should affect an entire state as a whole
- So now, we will start looking at the averages of all the states
```R
mn_1999 = with(pm25_1999, tapply(Sample.Value, factor(State.Code), mean, na.rm = T))
mn_2012 = with(pm25_2012, tapply(Sample.Value, factor(State.Code), mean, na.rm = T))
summary(mn_1999)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  4.862   9.519  12.315  12.406  15.640  19.956 
summary(mn_2012)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  4.006   7.355   8.729   8.759  10.613  11.992
```
- Next up, we can turn this data into a data frame of means by state
```R
d_1999 = data.frame(state = names(mn_1999), mean = mn_1999)
d_2012 = data.frame(state = names(mn_2012), mean = mn_2012)
head(d_1999)
  state      mean
1     1 19.956391
2     2  6.665929
4     4 10.795547
5     5 15.676067
6     6 17.655412
8     8  7.533304
```
- And then we merge the data frames
```R
mrg = merge(d_1999, d_2012, by ="state")
dim(mrg)
[1] 52  3
head(mrg)
  state    mean.x    mean.y
1     1 19.956391 10.126190
2    10 14.492895 11.236059
3    11 15.786507 11.991697
4    12 11.137139  8.239690
5    13 19.943240 11.321364
6    15  4.861821  8.749336
```
- `merge` actually split the entries based on the data frame they came from, so `mean.x` is 1999 and `mean.y` is 2012
- Now, plotting and connecting the dots
- We will plot the means of each year in a vertical line, then we can use the `segments` function to connection each point from 1999 (again, representing the mean PM25 of a state) to its 2012 counterpart, to see how the mean PM25 levels of each state changed
```R
par(mfrow=c(1,1))
with(mrg, plot(rep(1999, 52), mrg[, 2], xlim = c(1998, 2013)))
with(mrg, points(rep(2012, 52), mrg[, 3], ))
segments(rep(1999, 52), mrg[, 2], rep(2012, 52), mrg[, 3])
```
![[Pasted image 20260517124806.png]]
- And it seems that while some states really took reducing their PM25 level seriously, others went in the complete other opposite direction, and some didn't have any real significant change
- And this answers our initial question of "How did the PM25 levels change between 1999 and 2012 in the US" which we answered at a country level, a state level, and a monitor level
- The goal of an exploratory analysis such as this one, is to give us a sense of the data, and highlight the important follow up questions that we should be asking