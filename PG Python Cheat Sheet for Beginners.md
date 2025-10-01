---
tags: 
- Programming-Language/Python
- PG
MOC: Technology
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Python index|Back to index]]
- Acquired from [https://www.datacamp.com/cheat-sheet/getting-started-with-python-cheat-sheet](https://www.datacamp.com/cheat-sheet/getting-started-with-python-cheat-sheet)
![](https://media.datacamp.com/cms/python-basics-cheat-sheet-updated0925.png)

Python is the most popular programming language in [data science](https://www.datacamp.com/blog/what-is-data-science-the-definitive-guide). It is easy to learn and comes with a wide array of powerful libraries for data analysis. This cheat sheet provides beginners and intermediate users a guide to using python. Use it to jump-start your journey with python. if you want more detailed guides.

Have this cheat sheet at your fingertips

[Download PDF](https://media.datacamp.com/cms/python-basics-cheat-sheet-updated0925.pdf)

## Accessing help and getting object types

```python
1 + 1 #Everything after the hash symbol is ignored by Python
help(max) #Display the documentation for the max function
type('a') #Get the type of an object — this returns str
```

## Importing packages

Python packages are a collection of useful tools developed by the open-source community. They extend the capabilities of the python language. To install a new package (for example, pandas), you can go to your command prompt and type in `pip install pandas`. Once a package is installed, you can import it as follows.

```python
import pandas # Import a package without an alias
import pandas as pd # Import a package with an alias
from pandas import DataFrame # Import an object from a package
```

## The working directory

The working directory is the default file path that python reads or saves files into. An example of the working directory is `”C://file/path"`. The os library is needed to set and get the working directory.

```python
import os # Import the operating system package
os.getcwd() # Get the current directory
os.setcwd("new/working/directory") # Set the working directory to a new file path
```

## Operators

### Arithmetic operators

```python
102 + 37 #Add two numbers with +
102 - 37 # Subtract a number with -
4 * 6 # Multiply two numbers with *
22 / 7 # Divide a number by another with /
22 // 7 # Integer divide a number with //
3 ** 4 # Raise to the power with **
22 % 7 # Returns 1 # Get the remainder  after division with %
```

### Assignment operators

```python
a = 5 # Assign a value to a
x[0] =1 # Change the value of an item in a list
```

### Numeric comparison operators

```python
3 == 3 # Test for equality with ==
3 != 3 # Test for inequality with !=
3 > 1 # Test greater than with >
3 >= 3 # Test greater than or equal to with >=
3 < 4 # Test less than with <
3 <= 4 # Test less than or equal to with <=
```

### Logical operators

```python
not (2 == 2) # Logical NOT with not
(1 != 1) and (1 < 1) # Logical AND with and
(1 >= 1) or (1 < 1) # Logical OR with or
```

## Getting started with lists

A list is an ordered and changeable sequence of elements. It can hold integers, characters, floats, strings, and even objects.

### Creating lists

```python
# Create lists with [], elements separated by commas
x = [1, 3, 2]
```

### List functions and methods

```python
# Return a sorted copy of the list x
sorted(x) # Returns [1, 2, 3]

# Sort the list in-place (replaces x)
x.sort() # Returns None

# Reverse the order of elements in x
reversed(x) # Returns [2, 3, 1]

# Reverse the list in-place
x.reversed() # Returns None

# Count the number of element 2 in the list
x.count(2)
```

### Selecting list elements

Python lists are zero-indexed (the first element has index 0). For ranges, the first element is included, but the last is not.

```python
# Define the list 
x = ['a', 'b', 'c', 'd', 'e']

# Select the 0th element in the list
x[0] # 'a'

# Select the last element in the list
x[-1] # 'e'

# Select 1st (inclusive) to 3rd (exclusive)
x[1:3] # ['b', 'c']

# Select the 2nd to the end
x[2:] # ['c', 'd', 'e']

# Select 0th to 3rd (exclusive)
x[:3] # ['a', 'b', 'c']
```

### Concatenating lists

```python
# Define the list x and y  
x = [1, 3, 6] 
y = [10, 15, 21]

# Concatenate lists with +
x + y # [1, 3, 6, 10, 15, 21]

# Repeat list n times with *
3 * x # [1, 3, 6, 1, 3, 6, 1, 3, 6]
```

## Getting started with dictionaries

A dictionary stores data values in key-value pairs. That is, unlike lists indexed by position, dictionaries are indexed by their keys, the names of which must be unique.

### Creating dictionaries

```python
# Create a dictionary with {}
{'a': 1, 'b': 4, 'c': 9}
```

### Dictionary functions and methods

```python
# Define the dictionary
a = {'a': 1, 'b': 2, 'c': 3}

# Get the keys
x.keys() # dict_keys(['a', 'b', 'c'])

# Get  the values
x.values() # dict_values([1, 2, 3])

# Get a value from a dictionary by specifying the key
x['a'] # 1
```

## NumPy arrays

NumPy is a python package for scientific computing. It provides a multidimensional array of objects and efficient operations on them. To import NumPy, you can run this Python code `import numpy as np`

### Creating arrays

```python
# Convert a python list to a NumPy array
np.array([1, 2, 3]) # array([1, 2, 3])

# Return a sequence from start (inclusive) to end (exclusive)
np.arange(1,5) # array([1, 2, 3, 4])

# Return a stepped sequence from start (inclusive) to end (exclusive)
np.arange(1,5,2) # array([1, 3])

# Repeat values n times
np.repeat([1, 3, 6], 3) # array([1, 1, 1, 3, 3, 3, 6, 6, 6])

# Repeat values n times
np.tile([1, 3, 6], 3) # array([1, 3, 6, 1, 3, 6, 1, 3, 6])
```

## Math functions and methods

```python
# Calculate logarithm of an array
np.log(x) 
# Calculate exponential of an array
np.exp(x)
# Get maximum value of an array
np.max(x)
# Get minimum value of an array
np.min(x)
# Calculate sum of an array
np.sum(x)
# Calculate mean of an array
np.mean(x)
# Calculate q-th quantile of an array x
np.quantile(x, q)
# Round an array to n decimal places
np.round(x, n)
# Calculate variance of an array
np.var(x)
# Calculate standard deviation of an array
np.std(x)
```

## Getting started with characters and strings

```python
# Create a string variable with single or double quotes
"DataCamp"

# Embed a quote in string with the escape character \
"He said, \"DataCamp\""

# Create multi-line strings with triple quotes
"""
A Frame of Data
Tidy, Mine, Analyze It
Now You Have Meaning
Citation: https://mdsr-book.github.io/haikus.html
"""

# Get the character at a specific position
str[0] 

# Get a substring from starting to ending index (exclusive)
str[0:2]
```

### Combining and splitting strings

```python
# Concatenate strings with +
"Data" + "Framed" # 'DataFramed'

# Repeat strings with *
3 * "data " # 'data data data '

# Split a string on a delimiter
"beekeepers".split("e") # ['b', '', 'k', '', 'p', 'rs']
```

### Mutate strings

```python
# Create a string named str
str = "Jack and Jill"

# Convert a string to uppercase
str.upper() # 'JACK AND JILL'

# Convert a string to lowercase
str.lower() # 'jack and jill'

# Convert a string to title case
str.title() # 'Jack And Jill' 

# Replaces matches of a substring with another
str.replace("J", "P") # 'Pack and Pill'
```

## Getting started with DataFrames

pandas is a fast and powerful package for data analysis and manipulation in python. To import the package, you can use `import pandas as pd`. A pandas DataFrame is a structure that contains two-dimensional data stored as rows and columns. A pandas series is a structure that contains one-dimensional data.

### Creating DataFrames

```python
# Create a dataframe from a dictionary
pd.DataFrame({
    'a': [1, 2, 3],
    'b': np.array([4, 4, 6]),
    'c': ['x', 'x', 'y']
})

# Create a dataframe from a list of dictionaries
pd.DataFrame([
    {'a': 1, 'b': 4, 'c': 'x'},
    {'a': 1, 'b': 4, 'c': 'x'},
    {'a': 3, 'b': 6, 'c': 'y'}
])
```

### Selecting DataFrame Elements

Here are the different ways to select a row, column or element from a dataframe.

```python
# Select the row at position 3
df.iloc[3]

# Select one column by name
df['col']

# Select multiple columns by names
df[['col1', 'col2']]

# Select the column at position 2
df.iloc[:, 2]

# Select the element at row 2, column 3
df.iloc[3, 2]
```

### Manipulating DataFrames

```python
# Concatenate DataFrames vertically
pd.concat([df, df])

# Concatenate DataFrames horizontally
pd.concat([df,df],axis="columns")

# Get rows matching a condition
df.query('logical_condition')

# Drop columns by name
df.drop(columns=['col_name'])

# Rename columns
df.rename(columns={"oldname": "newname"})

# Add a new column
df.assign(temp_f=9 / 5 * df['temp_c'] + 32)

# Calculate the mean of each column
df.mean()

# Get summary statistics by column
df.agg(aggregation_function)

# Get unique rows
df.drop_duplicates()

# Sort by values in a column in ascending order
df.sort_values(by='col_name')

# Get the rows with the n largest values of a column
df.nlargest(n, 'col_name')
```

## Learn Python From Scratch

Master Python for data science and gain in-demand skills.

[Start Learning for Free](https://www.datacamp.com/tracks/associate-data-scientist-in-python)

Topics

Related

Develop a Python Training Program with DataCamp

blog

[View original](https://www.datacamp.com/blog/python-training-program)

Javier Canales Luna

7 min

Python for Data Science - A Cheat Sheet for Beginners

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/python-for-data-science-a-cheat-sheet-for-beginners)

This handy one-page reference presents the Python basics that you need to do data science

Karlijn Willems

Python For Data Science Cheat Sheet For Beginners

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/python-for-data-science-cheat-sheet-for-beginners)

This cheat sheet covers the basics that you need to know to do data science with Python

Karlijn Willems

![](https://media.datacamp.com/legacy/v1649267186/Pandas_Cheat_Sheet_for_Data_Science_in_Python_hqirhp_92e0234cdd.webp?w=750)Pandas Cheat Sheet for Data Science in Python

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/pandas-cheat-sheet-for-data-science-in-python)

A quick guide to the basics of the Python data analysis library Pandas, including code samples.

Karlijn Willems

Python For Data Science - A Cheat Sheet For Beginners

Tutorial

[View original](https://www.datacamp.com/tutorial/python-data-science-cheat-sheet-basics)

This handy one-page reference presents the Python basics that you need to do data science

Karlijn Willems

Python Tutorial for Beginners

Tutorial

[View original](https://www.datacamp.com/tutorial/python)

Get a step-by-step guide on how to install Python and use it for basic data science functions.

Matthew Przybyla