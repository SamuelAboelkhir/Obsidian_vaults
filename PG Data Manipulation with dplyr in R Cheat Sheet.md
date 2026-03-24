---
tags:
- R
- Programming-Language
MOC: Programming
source: "https://www.datacamp.com/cheat-sheet/data-manipulation-with-dplyr-in-r-cheat-sheet"
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG R index|Back to index]]

![Data Manipulation with Dplyr Cheat Sheet](https://images.datacamp.com/image/upload/v1660559784/image1_e4022b23cf.png)

Data Manipulation with Dplyr Cheat Sheet

Have this cheat sheet at your fingertips

[Download PDF](https://images.datacamp.com/image/upload/v1660300796/Marketing/Blog/Manipulating_Data_in_dplyr_Cheat_Sheet.pdf)

## Helpful syntax to know

### Installing and loading dplyr

```
# Install dplyr through tidyverse 
install.packages(“tidyverse”)

# Install it directly
install.packages(“dplyr”)

# Load dplyr into R
library(dplyr)
```

### The %>% operator

%>% is a special operator in R found in the magrittr and dplyr packages. %>% lets you pass objects to functions elegantly, and helps you make your code more readable. Consider this example of choosing columns a and b from the dataframe df

```
# Without the %>% operator
select(df, a, b)

# By using the %>% operator
df %>% select(a, b)
```

## Dataset used throughout this cheat sheet

Throughout this cheat sheet, we will be using this example dataset called airbnb\_listings, containing Airbnb listings with data on their location, year listed, number of rooms, and more.

<table><colgroup><col> <col> <col> <col> <col></colgroup><tbody><tr><td colspan="5"><strong>airbnb_listings</strong></td></tr><tr><td><strong>listing_id</strong></td><td><strong>city</strong></td><td><strong>country</strong></td><td><strong>number_of_rooms</strong></td><td><strong>year_listed</strong></td></tr><tr><td>1</td><td>Paris</td><td>France</td><td>5</td><td>2018</td></tr><tr><td>2</td><td>Tokyo</td><td>Japan</td><td>2</td><td>2017</td></tr><tr><td>3</td><td>New York</td><td>USA</td><td>2</td><td>2022</td></tr></tbody></table>

## Transforming data with dplyr

### Basic column operations with dplyr

```
# Select one or more columns with select() 
airbnb_listings %>% 
select(listing_id, city)

# Select columns based on start characters 
airbnb_listings %>% 
select(starts_with(“c”))

# Select columns based on end characters 
airbnb_listings %>% 
select(ends_with(“s”))

# Select all but one column (e.g., listing_id) 
airbnb_listings %>% 
select(-listing_id)

# Select all columns within a range 
airbnb_listings %>% 
select(country : year_listed)

# Reorder columns using relocate() 
airbnb_listings %>% 
relocate(city, country)

# Rename a column using rename() 
airbnb_listings %>% 
rename(year = year_listed)

# Select columns matching a regular expression 
airbnb_listings %>% 
select(matches(“(.n.) | (n.)”))
```

### Creating new columns with dplyr

```
# Create a time_on_market column using the difference of today’s year and the year_listed 
airbnb_listings %>% 
mutate(time_on_market = 2022 - year_listed)

# Create a full_address column by combining city and country 
airbnb_listings %>% 
transmute(full_address = paste(city, country))

# Add the number of observations for a column (e.g., number of listings per city) 
airbnb_listings %>% 
add_count(city)
```

### Working with rows

```
# Filter rows on one condition (e.g., country) 
airbnb_listings %>% 
filter(country == "France")

# Filter on two OR more conditions (country OR number_of_rooms) 
airbnb_listings %>% 
filter(country == "France" | number_of_rooms > 3)

# Filter on two AND more conditions (country AND number_of_rooms) 
airbnb_listings %>% 
filter(country == "France" & number_of_rooms > 3)

# Filter by checking if a value exists in another set of values 
airbnb_listings %>% 
filter(country %in% c( "Japan", "France"))

# Filter rows based on index of rows (e.g., first 3 rows) 
Airbnb_listings %>% 
slice(1:3)

# Sort rows by values in a column in ascending order 
airbnb_listings %>% 
arrange(number_of_rooms)

# Sort rows by values in a column in descending order 
airbnb_listings %>% 
arrange(desc(city))

# Remove duplicate rows in all the dataset 
airbnb_listings %>% 
distinct()

# Find unique values in the country column 
airbnb_listings %>% 
distinct(country)

# Select rows based on top-n values of a column (e.g., top 3 listings with the highest amount of rooms)
airbnb_listings %>% 
top_n(3, number_of_rooms)
```

## Aggregating data with dplyr

```
# Count groups within a column (e.g., count number of cities in airbnb_listings) 
airbnb_listings %>% 
count(city)

# Count groups within a column and return sorted 
airbnb_listings %>% 
count(country, sort = TRUE)

# Return the total sum of values for a column (e.g., total number of rooms) 
airbnb_listings %>% 
summarise(total_rooms=sum(number_of_rooms))

# Return the average of values for a column (e.g, average number of rooms in a given listing)
airbnb_listings %>% 
summarise(avg_room = mean(number_of_rooms))

# Return a custom summary statistic (e.g., average amount of time a listing stays on) 
airbnb_listings %>% 
summarise(average_listing_duration = 2022 - mean(year_listed))

# Group by a variable and return counts of each group (e.g., number of listings by country) 
airbnb_listings %>% 
group_by(country) %>% 
summarise(n = n())

# Group by a variable and return the average value per group (e.g., average number of rooms in listings per city) 
airbnb_listings %>% 
group_by(city) %>% 
summarise(avg_rooms = mean(number_of_rooms))
```

## Combining tables in dplyr

**df\_1**

| **x1** | **x2** |
| --- | --- |
| 1 | 2 |
| 3 | 6 |
| 5 | 4 |

**df\_2**

| **x1** | **x2** |
| --- | --- |
| 1 | 2 |
| 4 | 6 |
| 2 | 5 |

```
# Appending a table to the right side (horizontal) of another 
bind_cols(df_1, df_2)

# Appending a table to the bottom (vertical) of another 
bind_rows(df_1, df_2)

# Combining rows that exist in both tables and dropping duplicates 
union(df_1, df_2)

# Finding identical columns in both tables 
intersect(df_1, df_2)

# Finding rows that don’t exist in another table 
setdiff(df_1, df_2)
```

## Joining tables with dplyr

To showcase joins in dplyr, we’ll use an additional dataset containing details on \*host\_listings\* for airbnb listings

<table><colgroup><col> <col> <col> <col> <col></colgroup><tbody><tr><td colspan="5"><strong>airbnb_listings</strong></td></tr><tr><td><strong>listing_id</strong></td><td><strong>city</strong></td><td><strong>country</strong></td><td><strong>number_of_rooms</strong></td><td><strong>year_listed</strong></td></tr><tr><td>1</td><td>Paris</td><td>France</td><td>5</td><td>2018</td></tr><tr><td>2</td><td>Tokyo</td><td>Japan</td><td>2</td><td>2017</td></tr><tr><td>3</td><td>New York</td><td>USA</td><td>2</td><td>2022</td></tr></tbody></table>

<table><colgroup><col> <col> <col> <col></colgroup><tbody><tr><td colspan="4"><strong>host_listings</strong></td></tr><tr><td>host_id</td><td>name</td><td>listing_id</td><td>number_of_reviews</td></tr><tr><td>1</td><td>Jen Bricker</td><td>1</td><td>34</td></tr><tr><td>2</td><td>Richie Cotton</td><td>2</td><td>12</td></tr><tr><td>3</td><td>Raven Todd Dasilva</td><td>3</td><td>55</td></tr></tbody></table>

### Joining tables with dplyr

**Inner Join**

```
# Returns only records where a joining field finds a match in both tables. 
airbnb_listings %>% 
inner_join(host_listings, by = "listing_id")
```

**Left Join**

```
# Returns rows in left table and missing values for any columns from the right table where joining field did not find a match 
host_listings %>% 
left_join(airbnb_listings, by = "listing_id")
```

**Right Join**

```
# Returns rows in right table and missing values for any columns from the left table where joining field did not find a match 
host_listings %>% 
right_join(airbnb_listings, by = "listing_id")
```

**Full Join**

```
# Returns all records from both table, irrespective of whether there is a match on the joining field
host_listings %>% 
full_join(airbnb_listings, by = "listing_id")
```

**Anti Join**

```
# Returns records in the first table and excludes matching values from the second table 
airbnb_listings %>% 
anti_join(host_listings, by = "listing_id")
```

Topics

Related

The data.table R Package Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/the-datatable-r-package-cheat-sheet)

The data.table cheat sheet helps you master the syntax of this R package, and helps you to do data manipulations.

Karlijn Willems

![](https://media.datacamp.com/legacy/v1649270380/Tidyverse_Cheat_Sheet_For_Beginners_d4lkxp_537ffd3077.webp?w=750)Tidyverse Cheat Sheet For Beginners

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/tidyverse-cheat-sheet-for-beginners)

This tidyverse cheat sheet will guide you through the basics of the tidyverse, and 2 of its core packages: dplyr and ggplot2!

Karlijn Willems

![](https://media.datacamp.com/legacy/v1677237149/Reshaping_data_with_tidy_R_in_R_fdc7a037c8.png?w=750)Reshaping Data with tidyr in R

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/reshaping-data-with-tidyr-in-r)

In this cheat sheet, you will learn how to reshape data with tidyr. From separating and combining columns, to dealing with missing data, you'll get the download on how to manipulate data in R.

Richie Cotton

![R Text Cheat Sheet.png](https://media.datacamp.com/legacy/v1671056642/R_Text_Cheat_Sheet_3b4753f59a.png?w=750)Text Data In R Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/text-data-in-r-cheat-sheet)

Richie Cotton

The data.table R Package Cheat Sheet

Tutorial

[View original](https://www.datacamp.com/tutorial/data-table-cheat-sheet)

The data.table cheat sheet helps you master the syntax of this R package, and helps you to do data manipulations.

Karlijn Willems

Getting Started with the Tidyverse: Tutorial

Tutorial

[View original](https://www.datacamp.com/tutorial/tidyverse-tutorial-r)

Start analyzing titanic data with R and the tidyverse: learn how to filter, arrange, summarise, mutate and visualize your data with dplyr and ggplot2!

Hugo Bowne-Anderson