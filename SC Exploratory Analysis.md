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
- Regardless, plots in ggplot2 are generally made up of aesthetics `aes` which include things like size, shape, and color, along side `gemos` which is where you indicate if you're plotting lines, or points, etc...
- 