---
tags:
- R
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG R index|Back to index]]
- Acquired from [https://www.datacamp.com/cheat-sheet/ggplot2-cheat-sheet](https://www.datacamp.com/cheat-sheet/ggplot2-cheat-sheet)

Data visualization skills are table stakes for anyone looking to grow their R skills. ggplot2 is one of R’s premiere packages, as it allows an accessible approach to building robust data visualizations in R. In this cheat sheet, you’ll have a handy guide for all the functions and techniques to get you started with ggplot2.

Have this cheat sheet at your fingertips

[Download PDF](https://images.datacamp.com/image/upload/v1666806657/Marketing/Blog/ggplot2_cheat_sheet.pdf)

## The grammar of graphics

The grammar of graphics is a framework for specifying the components of a plot. This approach of building plots in a modular way allows a high level of flexibility for creating a wide range of visualizations. The framework can make plots easier to draw because you only have to declare what you want in the plot—the software, such as R's ggplot2 package, has to figure out how to draw it. This cheat sheet covers all you need to know about building data visualizations in ggplot2.

## Creating your first ggplot2 plot

### A single line plot

```R
# Create a lineplot in ggplot2
ggplot(data, aes(x = x_column, y = y_column)) + 
       geom_line()
```
- `ggplot()` creates a canvas to draw on.
- `data` is the data frame containing data for the plot. It contains columns named `x_column` and `y_column`.
- `aes()` matches columns of data to the aesthetics of the plot. Here, `x_column` is used for the x-axis and `y_column` for the y-axis.
- `geom_line()` adds a line geometry. That is, it draws a line plot connecting each data point in the dataset.

## Geometries, attributes, and aesthetics in ggplot2

- **Geometries** are visual representations of the data. Common geometries are points, lines, bars, [histograms](https://www.datacamp.com/tutorial/make-histogram-basic-r), boxes, and maps. The visual properties of geometries, such as color, size, and shape can be defined as attributes or aesthetics.
- **Attributes** are fixed values of visual properties of geometries. For example, if you want to set the color of all the points to red, then you would be setting the color attribute to red. Attributes must always be defined inside the geometry function.
```R
# Create a red lineplot in ggplot2      
ggplot(data, aes(x = x_column, y = y_column)) +
       geom_line(color = "red")
```
- **Aesthetics** are values of visual properties of geometries that depend on data values. For example, if you want the color of the points to depend on values in `z_column` then you would be mapping `z_column` to the `color` aesthetic. Aesthetics can be defined inside the geometry function or inside `ggplot()`. The latter makes the aesthetics apply to all the geometries included in the plot.
```R
# Create a lineplot where lines are colored according to another in ggplot2
ggplot(data, aes(x = x_column, y = y_column)) +
       geom_line(aes(color = z_column))
```

Here are the most common aesthetic mappings and attributes you will encounter in ggplot2

- `x` set or map the x-axis coordinate
- `y` set or map the x-axis coordinate
- `color` set or map the color or edge color
- `fill` set or map the interior (fill) color
- `size` set or map the size or width
- `alpha` set or map the transparency

## The most common visualizations in ggplot2

### Capture a trend

```R
# Create a multi-line plot with ggplot2
ggplot(data, aes(x_column, y_column, color = color_column)) +   
       geom_line()
```

**Note: Swap the** `color` **aesthetic for the group aesthetic to make all lines the same color.**

```R
# Create an area chart with ggplot2
ggplot(data, aes(x = x_column, y = y_column)) +
       geom_area()
```
```R
# Create a stacked area chart with ggplot2
ggplot(data, aes(x = x_column, y = y_column, fill=z_column)) +
       geom_area()
```

### Visualize relationships

```R
# Create a scatter plot with ggplot2
ggplot(data, aes(x = x_column, y = y_column)) +
       geom_point()
```
```R
# Create a bar plot with ggplot2 
ggplot(data, aes(x = x_column, y = y_column)) + 
       geom_col()
```

**Note: Swap** `geom_col() ` **for** `geom_bar()` **to calculate the bar heights from counts of the x values.**

```R
# Create a lollipop chart with ggplot2
ggplot(data, aes(x = x_column, y = y_column)) +
       geom_point() + 
       geom_segment(aes(x = x_column, xend = x_column, y = 0, 
                        yend = y_column))
```
```R
# Create a bubble plot with ggplot2
ggplot(data, aes(x = x_column, y = y_column, size = size_column)) +
       geom_point(alpha = 0.7) +
       scale_size_area()
```

**Note: In a bubble plot, "bubbles" can overlap, which can be solved by adjusting the transparency attribute,** `alpha. scale_size_area()` **makes the points to be proportional to the values in** `size_column`**.**

### Visualize distributions

```R
# Create a histogram with ggplot2
ggplot(data, aes(x_column)) + 
       geom_histogram(bins = 15)
```
```R
# Create a box plot with ggplot2
ggplot(data, aes(x = x_column, y = y_column)) +
       geom_boxplot()
```
```R
# Create a violin plot with ggplot2
ggplot(data, aes(x = x_column, y = y_column, fill = z_value)) +
       geom_violin()
```
```R
# Create a density plot with ggplot2
ggplot(data, aes(x = x_column)) +
       geom_density()
```

## Customizing visualizations with ggplot2

### Manipulating axes

```R
# Switching to logarithmic scale
ggplot(data, aes(x = x_column, y = y_column)) + 
      geom_point() +
      scale_x_log10() # or scale_y_log10
```
```R
# Reverse the direction of the axis
    ggplot(data, aes(x = x_column, y = y_column)) + 
      geom_point() + 
      scale_x_reverse()
```
```R
# Square root scale
ggplot(data, aes(x = x_column, y = y_column)) +
       geom_point() +
       scale_x_sqrt()
```
```R
# Changing axis limits without clipping
ggplot(data, aes(x = x_column, y = y_column)) +
      geom_point() +
      coord_cartesian(xlim = c(min, max), 
                      ylim = c(min, max),
                      clip = "off")
```
```R
# Changing axis limits with clipping     ggplot(data, aes(x = x_column, y = y_column)) + 
      geom_point() + 
      xlim(min, max) + 
      ylim(min, max)
```

### Manipulating labels and legends

```R
# A scatter plot that will be used throughout these examples
base_plot <- ggplot(data, aes(x = x_column, y = y_column, color = color_column)) +   
                    geom_point()

# Adding labels on the plot
base_plot + labs(x = 'X Axis Label', y = 'Y Axis Label', title = 'Plot title', 
                 subtitle = 'Plot subtitle', caption = 'Image by author')

# When using any aesthetics
base_plot + labs(color = "Diamond depth", size = "Diamond table")

# Remove the legend
base_plot + theme(legend.position = "none")

# Change legend position outside of the plot — You can also pass "top", "right", or "left"
base_plot + theme(legend.position = "bottom")

# Place the legend into the plot area
base_plot + theme(legend.position = c(0.1, 0.7))
```

### Changing colors

```R
# Change the outline color of a histogram geom
ggplot(diamonds, aes(price)) + 
       geom_histogram(color = "red")

# Change the fill color of a histogram geom
ggplot(diamonds, aes(price)) +
       geom_histogram(fill = "blue")

# Add a gray color scale
ggplot(iris, aes(Sepal.Length, Sepal.Width, color = Species)) +
       geom_point(size = 4) +
       scale_color_grey()

# Change to other native color scales
ggplot(iris, aes(Sepal.Length, Sepal.Width, color = Species)) +
       geom_point(size = 4) + 
       scale_color_brewer(palette = "Spectral")
```

### Changing shape

```R
# Change the shape of markers 
ggplot(diamonds, aes(price, carat)) +
       geom_point(shape = 1)

shape = 1 makes the points circles. Run example(points) to see the shape for each number. 

# Change the shape of markers based on a third column
base_plot + 
   geom_point(size = 2)

# Change the shape radius
base_plot + 
   scale_radius(range = c(1, 6))

# Change max shape area size
base_plot + 
   scale_size_area(max_size = 4)
```

### Changing fonts

```R
# Change font family
base_plot + 
   theme(text = element_text(family = "serif"))

# Change font size
base_plot + 
   theme(text = element_text(size = 20))

# Change text angle
base_plot + 
   theme(text = element_text(angle = 90))

# Change alignment with hjust and vjust
base_plot + 
   theme(text = element_text(hjust = 0.7, vjust = 0.4))
```

### Changing themes

```R
# Minimal theme
base_plot + theme_minimal()

# White background
base_plot + theme_bw()

# Dark theme (high contrast)
base_plot + theme_dark()

# Classic theme
base_plot + theme_classic()
```

## Facetting

Faceting breaks your plot into multiple panels, allowing you to compare different portions of your dataset side-by-side. For example, you can show data for each category of a categorical variable in its own panel.

```R
# Facet the figures into a rectangular layout of two columns
base_plot + facet_wrap(vars(cut), ncol = 2)

# Facet the figures into a rectangular layout of two rows
base_plot + facet_wrap(vars(cut), nrow = 2)

# Facet into a rectangular layout but give axes free ranges (variable plot dimensions):
base_plot + facet_wrap(vars(cut), scales = "free")

# Facet by both columns and rows with facet_grid
base_plot + 
   facet_grid(rows = vars(clarity), cols = vars(cut),
              space = "free", scales = "free")
```

Have this cheat sheet at your fingertips

[Download PDF](https://images.datacamp.com/image/upload/v1666806657/Marketing/Blog/ggplot2_cheat_sheet.pdf)

Topics

Related

![](https://media.datacamp.com/legacy/v1649270380/Tidyverse_Cheat_Sheet_For_Beginners_d4lkxp_537ffd3077.webp?w=750)Tidyverse Cheat Sheet For Beginners

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/tidyverse-cheat-sheet-for-beginners)

This tidyverse cheat sheet will guide you through the basics of the tidyverse, and 2 of its core packages: dplyr and ggplot2!

Karlijn Willems

![Plotly express.png](https://media.datacamp.com/legacy/v1668598938/Plotly_express_16326cf130.png?w=750)Plotly Express Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/plotly-express-cheat-sheet)

Plotly is one of the most widely used data visualization packages in Python. Learn more about it in this cheat sheet.

Richie Cotton

![](https://media.datacamp.com/legacy/v1683650925/datarhys_an_absurdist_oil_painting_of_a_humanoid_robot_assembli_43ea48e7_758d_48ef_9154_2c1d956ec52a_0164a808ee.png?w=750)How to Make a ggplot2 Histogram in R

Tutorial

[View original](https://www.datacamp.com/tutorial/make-histogram-ggplot2)

Learn how to make a ggplot2 histogram in R. Make histograms in R based on the grammar of graphics.

Kevin Babitz

![](https://media.datacamp.com/legacy/v1706533365/datarhys_an_absurdist_oil_painting_of_a_coder_stood_outside_wit_a7e623cf_b0eb_44c8_acd8_5eaf6c211664_017798a34f.png?w=750)Visualizing Climate Change Data with ggplot2: A Step-by-Step Tutorial

Tutorial

[View original](https://www.datacamp.com/tutorial/visualizing-climate-change-data-with-ggplot2)

Learn how to use ggplot2 in R to create compelling visualizations of climate change data. This step-by-step tutorial teaches you to find, analyze, and visualize historical weather data.

Bruno Ponne

![](https://media.datacamp.com/legacy/v1694694255/video_games_ggplot2_7ab75076e5.jpg?w=750)Visualizing Video Game Sales Data with ggplot2 in R

code-along

[View original](https://www.datacamp.com/code-along/live-training-visualizing-video-game-sales-data-with-ggplot2-in-r)

Learn to do exploratory data analysis and create visualizations with ggplot2.

Richie Cotton

![[PG data-visualization-ggplo2.pdf]]