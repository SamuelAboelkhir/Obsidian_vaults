---
tags: 
- Python
- Programming-Language
MOC: Programming
source: "https://www.geeksforgeeks.org/python/generators-in-python/"
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Python index|Back to index]]
- A generator in python and many other languages that provide generators is a type of iterable object that looks like a function, as it can have parameters, and acts as an iterable.
- It generates a list of values on the fly, one by one, and without storing it as an actual list in memory.
- A generator is usually used inside of loops, and when reached in during the loop execution, it would start to run and generate the its first value. Once a `yield` statement is reached inside the generator, it will stop executing and will return the value being yielded.
- After the loop finishes its current iteration, it will start the next iteration, and will proceed until the generator is reached again.
- Once the generator has been reached, it will pick up where it left off after the yield statement, until it reaches another yield statement.
- The generator will finish once it reaches a `finish` statement, or once the loop itself has ended, or has lead any kind of satisfactory conclusion, that would lead to a `break` statement.

# Generators explanation from GeeksForGeeks
A ****generator function**** is a special type of function that returns an iterator object. Instead of using return to send back a single value, generator functions use yield to produce a series of results over time. This allows the function to generate values and pause its execution after each yield, maintaining its state between iterations.

****Example:****

```python
def fun(max):
	cnt = 1
	while cnt <= max:
		yield cnt
		cnt += 1

ctr = fun(5)
for n in ctr:
	print(n)
```

**Output**
```
1
2
3
4
5
```

****Explanation:**** This generator function fun yields numbers from 1 up to a specified max. Each call to ****next()**** on the generator object resumes execution right after the yield statement, where it last left off.

## Why Do We Need Generators?

- ****Memory Efficient:**** Handle large or infinite data without loading everything into memory.
- ****No List Overhead:**** Yield items one by one, avoiding full list creation.
- ****Lazy Evaluation:**** Compute values only when needed, improving performance.
- ****Support Infinite Sequences:**** Ideal for generating unbounded data like Fibonacci series.
- ****Pipeline Processing:**** Chain generators to process data in stages efficiently.

Let's take a deep dive in python generators:

## Creating Generators

Creating a generator in Python is as simple as defining a function with at least one yield statement. When called, this function doesn’t return a single value; instead, it returns a generator object that supports the iterator protocol. The generator has the following syntax in [Python](https://www.geeksforgeeks.org/python/python-programming-language-tutorial/):

> def generator\_function\_name(parameters):  
> \# Your code here  
> yield expression  
> \# Additional code can follow

****Example:**** we will create a simple generator that will yield three integers. Then we will print these integers by using Python [for loop](https://www.geeksforgeeks.org/python/python-for-loops/).

```Python
def fun():
	yield 1
	yield 2
	yield 3
	
# Driver code to check above generator function
for val in fun():
	print(val)
```
  
**Output**
```
1
2
3
```

## Yield vs Return

- ****Yield:**** is used in generator functions to provide a sequence of values over time. When yield is executed, it pauses the function, returns the current value and retains the state of the function. This allows the function to continue from same point when called again, making it ideal for generating large or complex sequences efficiently.
- ****Return:**** is used to exit a function and return a final value. Once return is executed, function is terminated immediately and no state is retained. This is suitable for cases where a single result is needed from a function.

****Example with return:****

```python
def fun():
	return 1 + 2 + 3

res = fun()
print(res)
```
  
**Output**
```
6
```

## Generator Expression

****Generator expressions**** are a concise way to create generators. They are similar to list comprehensions but use parentheses instead of square brackets and are more memory efficient.

****Syntax:****

> (expression for item in iterable)

****Example:**** We will create a generator object that will print the squares of integers between the range of 1 to 6 (exclusive).

```python
sq = (x*x for x in range(1, 6))
for i in sq:
	print(i)
```
  
**Output**
```
1
4
9
16
25
```

### Applications of Generators in Python

Suppose we need to create a stream of Fibonacci numbers. Using a generator makes this easy, you just call next() to get the next number without worrying about the stream ending.

- Generators are especially useful for processing large data files, like logs, because:
- They handle data in small parts, saving memory
- They don’t load the entire file at once
- While iterators can do similar tasks, generators are quicker to write since you don’t need to define \_\_next\_\_ and \_\_iter\_\_ methods manually.