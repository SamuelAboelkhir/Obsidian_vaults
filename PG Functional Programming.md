---
tags:
- Paradigms
- Programming-Language
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Paradigms index|Back to index]]

# What Is Functional Programming?
- Functional programming is a style (or "paradigm" if you're pretentious) of programming where we compose functions instead of mutating state (updating the value of variables).
	- [Functional programming](https://en.wikipedia.org/wiki/Functional_programming) is more about declaring _what_ you want to happen, rather than _how_ you want it to happen.
	- [Imperative](https://en.wikipedia.org/wiki/Imperative_programming) (or procedural) programming declares both the _what_ and the _how_.

 **Example of imperative code:**
```python
car = create_car()
car.add_gas(10)
car.clean_windows()
```
 **Example of functional code:**
```python
return clean_windows(add_gas(create_car()))
```
- The important distinction is that in the functional example, we never change the value of the `car` variable, we just compose functions that return new values, with the outermost function, `clean_windows` in this case, returning the final result.
- [Haskell](https://www.haskell.org/), [OCaml](https://ocaml.org/) and [Elixir](https://elixir-lang.org/). are examples of functional languages
# Immutability
- In FP, we strive to make data [_immutable_](https://en.wikipedia.org/wiki/Immutable_object). Once a value is created, it cannot be changed. _Mutable_ data, on the other hand, can be changed after it's created.
## Who Cares?
- Immutable data is easier to think about and work with. When 10 different functions have access to the same variable, and you're debugging a problem with that variable, you have to consider the possibility that any of those functions could have changed the value.
- When a variable is immutable, you can be sure that it hasn't changed since it was created. It's a helluva lot easier to work with.
- _Generally speaking, immutability means fewer bugs and more maintainable code._
# Declarative programming
- Functional programming aims to be _declarative_. We prefer to declare _what_ we want the computer to do, rather than muck around with the details of _how_ to do it.
## Declarative Styling
- The following CSS changes all [`button`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) elements to have red text:
```css
button {
  color: red;
}
```
- It does _not_ execute line-by-line like an imperative language. Instead, it simply declares the desired style, and it's up to a web browser to figure out how to apply and display it.
## Imperative Styling
- Unlike functional programming (and CSS), a lot of code is _imperative_. We write out the exact step-by-step implementation details. This Python script draws a red button on a screen using the [Tkinter](https://docs.python.org/3/library/tkinter.html) library:
```python
from tkinter import Button, Tk  # first, import the library

master: Tk = Tk()  # create a window
master.geometry("200x100")  # set the window size
button: Button = Button(master, text="Submit", fg="red")  # create a button
button.pack()
master.mainloop()  # start the event loop
```
# It's Math
- Functional programming tends to be popular among developers with a strong mathematical background. After all, a math equation isn't procedural – it's declarative. Take the following equation:
```text
avg = Σx/N
```
- To put this calculation in plain English:
	1. `Σ` is just the Greek letter [Sigma](https://en.wikipedia.org/wiki/Sigma), and it represents "the [sum](https://en.wikipedia.org/wiki/Summation) of a collection."
	2. `x` is the collection of numbers we're averaging.
	3. `N` is the number of elements in the collection.
	4. `avg` is equal to the sum of all the numbers in collection `x` divided by the number of elements in collection `x`.
- So, the equation really just says that `avg` is the average of all the numbers in collection `x`. This math equation is a _declarative_ way of writing "calculate the average of a list of numbers." Here's some _imperative Python code_ that does the same thing:
```python
def get_average(nums: list[int]) -> float:
    total = 0
    for num in nums:
        total += num
    return total / len(nums)
```
- However, with functional programming, we would write code that's _a bit more_ declarative:
```python
def get_average(nums: list[int]) -> float:
    return sum(nums) / len(nums)
```
- Here we're not keeping track of state (the `total` variable in the first example is "[stateful](https://en.wikipedia.org/wiki/State_\(computer_science\))"). We're simply composing functions together to get the result we want.
# Should I Use Functions or Classes?
- **If you're unsure, default to functions.** Reach for classes when you need something long-lived and stateful that would be easier to model if you could share behavior _and data structure_ via inheritance. This is often the case for:
	- Video games
	- Simulations
	- GUIs
- The difference is:
> **Classes** encourage you to think about the world as a hierarchical collection of objects. Objects bundle behavior, data, and state together in a way that draws boundaries between instances of things, like chess pieces on a board.

> **Functions** encourage you to think about the world as a series of data transformations. Functions take data as input and return a transformed output. For example, a function might take the entire state of a chess board and a move as inputs, and return the new state of the board as output.

- _Use what feels right to you in your projects, and adjust and refactor as you improve your skills._
# Debugging FP
- It's _nearly impossible_, even for tenured senior developers, to write perfect code the first time. That's why debugging is such an important skill. The trouble is, sometimes you have these "elegant" (sarcasm intended) one-liners that are tricky to debug:
```python
def get_player_position(
    position: float, velocity: float, friction: float, gravity: float
) -> float:
    return calc_gravity(calc_friction(calc_move(position, velocity), friction), gravity)
```
- If the output of `get_player_position` is incorrect, it's hard to know what's going on inside that black box. _Break it up!_ Then you can inspect the `moved`, `slowed`, and `final` variables more easily:
```python
def get_player_position(
    position: float, velocity: float, friction: float, gravity: float
) -> float:
    moved = calc_move(position, velocity)
    slowed = calc_friction(moved, friction)
    final = calc_gravity(slowed, gravity)
    print("Given:")
    print(
        f"position: {position}, velocity: {velocity}, friction: {friction}, gravity: {gravity}"
    )
    print("Results:")
    print(f"moved: {moved}, slowed: {slowed}, final: {final}")
    return final
```
- Once you've run it, found the issue, and solved it, you can remove the `print` statements.
# Functional vs. OOP
- Functional programming and object-oriented programming are **styles for writing code**. One isn't inherently superior to the other, but to be a well-rounded developer you should understand both well and use ideas from each when appropriate.
- You'll encounter developers who love functional programming and others who love object-oriented programming. However, contrary to popular opinion, FP and OOP are _not_ always at odds with one another. They aren't opposites. Of the four pillars of OOP, [inheritance](https://en.wikipedia.org/wiki/Inheritance_\(object-oriented_programming\)) is the only one that doesn't fit with functional programming.
![[Pasted image 20260823200245.png|366]]
- Inheritance isn't seen in functional code due to the mutable classes that come along with it. Encapsulation, polymorphism and abstraction are still used all the time in functional programming.
- When working in a language that supports ideas from both FP and OOP (like Python, JavaScript, or Go) the best developers are the ones who can use the best ideas from both paradigms effectively and appropriately.
# Statements vs. Expressions
- Studying functional programming is really about returning to the most basic aspects of programming and looking at them in a new way. [Statements](https://en.wikipedia.org/wiki/Statement_\(computer_science\)) and [expressions](https://en.wikipedia.org/wiki/Expression_\(computer_science\)) are a great example of that.
## Statements
- "Statements" are _actions to be carried out_. For example:
	- "Set `n` to `7`"
	- "Define a function named `greet`"
	- "If `x > 10`, `print` a greeting to Alice"
- In Python, such statements look like this:
```python
n: int = 7  # Variable assignment statement


def greet(name: str) -> str:  # Function definition statement
    return f"Hello, {name}!"


if x > 10:  # `if` statement
    print(greet("Alice"))

for i in range(n):  # `for` loop statement
    print(i)
```
- **Every complete instruction is a statement.**
## Expressions
- _Expressions_ are a _subset_ of statements that _produce values_. _Evaluating an expression_ results in a _value_ that can be used in whatever way is needed. It can be assigned to a variable, returned from a function, etc.
```python
result: int = 2 + 2  # Arithmetic expression
length: int = len("hello")  # Function call expression
total_cost: float = len(items) * cost  # Multiple expressions combined into one
```
- One thing that may surprise you is that, in most languages (including Python), _every function call is an expression_. When you call a function, it returns a value – whether or not you realize it or do anything with that value.
- Even if a Python function doesn't have a `return` statement, it still implicitly returns `None`. You can test this by assigning a `print` call to a variable:
```python
x: None = print("hello")  # hello
print(x)  # None
```
- Sure enough: `print`, the first function we all learn, technically returns a value.
# Expressions Over Statements
- Because expressions always produce values, they're _reusable_ and _declarative_. You can compose expressions and nest them within each other – but you can't always do that with other kinds of statements.
- **Functional programming encourages the use of expressions over statements** where possible, because expressions tend to minimize side effects, and make the code easier to reason about. For example, a function that returns a sum is an expression:
```python
total: int = sum([1, 2, 3, 4])
```
- We can get the same result with a loop, but that involves a series of statements:
```python
total: int = 0
for n in [1, 2, 3, 4]:
    total += n
```
- Again, it's simple to combine expressions:
```python
print(sum([1, 2, 3, 4]) * 2)  # 20
```
- But we can't really do the same thing with our series of statements:
```python
# This doesn't work!
print((
total = 0
for n in [1, 2, 3, 4]:
    total += n
) * 4)
```
- Expressions tend to be _concise_ and _logically pure_. Some languages that are designed for functional programming, like Haskell, treat _everything_ as an expression. In those languages, even control flow constructs like `if` and `case` are expressions that return values.
# Functions As Values
- In Python, functions are just values, like strings, integers, or objects. For example, we can assign an existing function to a variable:
```python
from collections.abc import Callable


def add(x: int, y: int) -> int:
    return x + y


# assign the function to a new variable
# called `addition`. It behaves the same
# as the original `add` function
addition: Callable[[int, int], int] = add
print(addition(2, 5))
# 7
```
- `Callable` is the [type hint for a function](https://docs.python.org/3/library/typing.html#annotating-callable-objects). `Callable[[int, int], int]` means a function that takes two `int`s as arguments and returns an `int`.
# Anonymous Functions
- Anonymous functions have _no name_, and in Python, they're called [lambda functions](https://docs.python.org/3/reference/expressions.html#lambda) after [lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus). Here's a lambda function that takes a single argument `x` and returns the result of `x + 1`:
```python
lambda x: x + 1
```
- Notice that the [expression](https://docs.python.org/3/reference/expressions.html#expressions) `x + 1` is returned _automatically_, no need for a `return` statement. Compare that to how you'd normally write a function:
```python
def add_one(x: int) -> int:
    return x + 1
```
- Because functions are just values, we can assign the function to a variable named `add_one`:
```python
from collections.abc import Callable

add_one: Callable[[int], int] = lambda x: x + 1
print(add_one(2))
# 3
```
- Lambda functions might _look_ scary, but they're still just functions. Because they simply return the result of an expression, they're often used for small, simple evaluations. Here's an example that uses a lambda to get a value from a dictionary:
```python
get_age: Callable[[str], int | str] = lambda name: {
    "lane": 29,
    "hunter": 69,
    "allan": 17,
}.get(name, "not found")
print(get_age("lane"))
# 29
```
# First-Class and Higher-Order Functions
- A programming language "supports first-class functions" when functions are treated like any other variable. That means functions can be passed as arguments to other functions, can be returned by other functions, and can be assigned to variables.
    First-class function: A function that is treated like any other value
    Higher-order function: A function that accepts another function as an argument or returns a function
- Python does support first-class and higher-order functions.
## First-Class Example
```python
from collections.abc import Callable


def square(x: int) -> int:
    return x * x


# Assign function to a variable
f: Callable[[int], int] = square

print(f(5))
# 25
```
## Higher-Order Example
```python
def square(x: int) -> int:
    return x * x


def my_map(func: Callable[[int], int], arg_list: list[int]) -> list[int]:
    result: list[int] = []
    for i in arg_list:
        result.append(func(i))
    return result


squares: list[int] = my_map(square, [1, 2, 3, 4, 5])
print(squares)
# [1, 4, 9, 16, 25]
```
# Map
- "Map," "filter," and "reduce" are three commonly used [higher-order functions](https://en.wikipedia.org/wiki/Higher-order_function) in functional programming.
- In Python, the built-in [map](https://docs.python.org/3/library/functions.html#map) function takes a function and an [iterable](https://docs.python.org/3/glossary.html#term-iterable) (often a list) as inputs. It returns an [iterator](https://en.wikipedia.org/wiki/Iterator) that applies the function to every item, yielding the results.
![[Pasted image 20260829211014.png]]
- With `map`, we can operate on lists without using loops and nasty stateful variables. For example, given this code:
```python
def square(x: int) -> int:
    return x * x


nums: list[int] = [1, 2, 3, 4, 5]
squared_nums: list[int] = []
for num in nums:
    num_squared: int = square(num)
    squared_nums.append(num_squared)

print(squared_nums)
# [1, 4, 9, 16, 25]
```
- We could use `map` instead, like this:
```python
from collections.abc import Iterator


def square(x: int) -> int:
    return x * x


nums: list[int] = [1, 2, 3, 4, 5]
squared_nums: Iterator[int] = map(square, nums)

print(list(squared_nums))
# [1, 4, 9, 16, 25]
```
- `map()` returns a "map object," so the [`list()` type constructor](https://docs.python.org/3/library/stdtypes.html#list) is needed to convert it back into a standard list.
# Filter

The built-in [`filter` function](https://docs.python.org/3/library/functions.html#filter) takes a function and an iterable (often a list) and returns an iterator that keeps elements from the original iterable only where the result of the function on that item returned `True`.
![[Pasted image 20260829211711.png]]
In Python:

```python
def is_even(x: int) -> bool:
    return x % 2 == 0


numbers: list[int] = [1, 2, 3, 4, 5, 6]
evens: list[int] = list(filter(is_even, numbers))
print(evens)
# [2, 4, 6]
```
# Reduce
- The built-in [`functools.reduce()`](https://docs.python.org/3/library/functools.html#functools.reduce) function takes a function and a list of values, and applies the function to each value in the list, _accumulating a single result_ as it goes.
![[Pasted image 20260829211800.png|325]]
```python
# import functools from the standard library
import functools


def add(sum_so_far: int, x: int) -> int:
    print(f"sum_so_far: {sum_so_far}, x: {x}")
    return sum_so_far + x


numbers: list[int] = [1, 2, 3, 4]
sum: int = functools.reduce(add, numbers)
# sum_so_far: 1, x: 2
# sum_so_far: 3, x: 3
# sum_so_far: 6, x: 4
# 10 doesn't print, it's just the final result
print(sum)
# 10
```
- Notice that we're passing the function `add` without the `()`! It means that `reduce` will take care of execution and pass the parameters for you. Think of passing `add` like handing someone a recipe (the instructions), instead of the finished dish (the result of the execution).
# Map, Filter, and Reduce Review
- Higher-order functions like `map`, `filter`, and `reduce` allow us to _avoid stateful iteration and mutation of variables_.
- Take a look at this [imperative](https://en.wikipedia.org/wiki/Imperative_programming) code that calculates the [factorial](https://en.wikipedia.org/wiki/Factorial) of a number:
```python
def factorial(n: int) -> int:
    # a procedure that continuously multiplies
    # the current result by the next number
    result: int = 1
    for i in range(1, n + 1):
        result *= i
    return result
```
- Here's the same factorial function using `reduce`:
```python
import functools


def factorial(n: int) -> int:
    return functools.reduce(lambda x, y: x * y, range(1, n + 1))
```
- In the functional example, we're just combining functions to get the result we want. There's no need to reassign variables or keep track of the program's state in a loop.
- A loop is inherently stateful! Depending on which iteration you're on, the `i` variable has a different value.
# Zip
- The [`zip` function](https://docs.python.org/3/library/functions.html#zip) takes two iterables (often lists), and returns a _new_ iterable where each element is a _tuple_ containing one element from each of the original iterables.
```python
a: list[int] = [1, 2, 3]
b: list[int] = [4, 5, 6]

c: list[tuple[int, int]] = list(zip(a, b))
print(c)
# [(1, 4), (2, 5), (3, 6)]
```
# Pure functions
- Pure functions have two properties
	- They _always_ return the same value given the same arguments.
	- Running them causes no [side effects](https://en.wikipedia.org/wiki/Side_effect_\(computer_science\)).
- In short: **pure functions don't do anything with anything that exists outside of their scope**.
## Example of a Pure Function
```python
def find_max(nums: list[int]) -> float:
    max_val: float = float("-inf")
    for num in nums:
        if max_val < num:
            max_val = num
    return max_val
```
## Example of an Impure Function
```python
# instead of returning a value
# this function modifies a global variable
global_max: float = float("-inf")


def find_max(nums: list[int]) -> None:
    global global_max
    for num in nums:
        if global_max < num:
            global_max = num
``` 
# Reference vs. Value
- When you pass a value into a function as an argument, one of two things can happen:
	- It's passed by **reference**: The function has access to the original value and can change it.
	- It's passed by **value**: The function only has access to a copy. Changes to the copy within the function don't affect the original.
- _There is more nuance to it, but this explanation works for an introduction._ In Python, the following types are passed by **reference**:
	- Lists
	- Dictionaries
	- Sets
- These types, on the other hand, are passed by **value**:
	- Integers
	- Floats
	- Strings
	- Booleans
	- Tuples
- Most _container_ types are passed by reference (except for tuples!), and most _basic_ types are passed by value.
## Example of Pass-By-Reference
- Lists are passed by reference and are **mutable**:
```python
def modify_list(inner_lst: list[int]) -> None:
    inner_lst.append(4)
    # the original "outer_lst" is updated
    # because inner_lst is a reference to the original


outer_lst: list[int] = [1, 2, 3]
modify_list(outer_lst)
# outer_lst = [1, 2, 3, 4]
```
## Example of Pass-By-Value
- Integers are passed by value; they can be copied freely but are **immutable**:
```python
def attempt_to_modify(inner_num: int) -> None:
    inner_num += 1
    # the original "outer_num" is not updated
    # because inner_num is a copy of the original


outer_num: int = 1
attempt_to_modify(outer_num)
# outer_num = 1
```
>Simply assigning a new variable to an existing dictionary doesn't copy that dictionary; it points to the same dictionary. Instead, use the [`.copy()` method](https://docs.python.org/3/library/stdtypes.html#dict.copy) to create a new copy of a dictionary.
# Pass by Reference Impurity
- Because certain types in Python are passed by reference, we can mutate values that we didn't intend to. This is a form of function impurity!
- Remember, a pure function has _no side effects_. It shouldn't modify anything outside of its scope, _including its inputs_. It should return new copies of inputs instead of changing them.
## Pure Function
```python
def remove_format(default_formats: dict[str, bool], old_format: str) -> dict[str, bool]:
    new_formats: dict[str, bool] = default_formats.copy()
    new_formats[old_format] = False
    return new_formats
```
## Impure Function
```python
def remove_format(default_formats: dict[str, bool], old_format: str) -> dict[str, bool]:
    default_formats[old_format] = False
    return default_formats
```
## Why Do We Care?
- One of the biggest differences between _good_ and _great_ developers is how often they incorporate pure functions into their code.
- Pure functions are easier to read, easier to reason about, easier to test, and easier to combine.
- Even if you're working in an imperative language like Python, you can (and should) write pure functions whenever reasonable.
- There's nothing worse than trying to debug a program where the order of function calls needs to be _juuuuust right_ because they all read and modify the same global variable.
# Input and Output
![[Pasted image 20260922020810.png|600]]
Comic by [xkcd](https://xkcd.com/1790/).

- The term "i/o" stands for input/output. In the context of writing programs, i/o refers to anything in our code that interacts with the "outside world." And "outside world" just means anything that's not stored in our application's memory (like variables).
## Examples of I/O
- Reading from or writing to a file on the hard drive
- Accessing the internet
- Reading from or writing to a database
- Even simply _printing to the console_!!
_All i/o is a form of "side effect."_
# Should I I/O?
- A program that doesn't do _any_ i/o is pretty useless. What's the point of computing something if you can't see the results?
![[Pasted image 20260922020902.png]]
- In functional programming, i/o is viewed as _dirty but necessary_. We know we can't _eliminate_ i/o from our code, so we just _contain_ it as much as possible. There should be a clear place in your project that does nasty i/o stuff, and the rest of your code can be pure.
- For example, a Python program might:
	1. Read a file from the hard drive as the program starts
	2. Run a bunch of pure functions to analyze the data
	3. Write the results of the analysis to another file on the hard drive at the end	
![[Pasted image 20260922020936.png|426]]	
# No-Op
- A [no-op](https://en.wikipedia.org/wiki/NOP_\(code\)) is an operation that does... nothing.
- If a function doesn't return anything, it's probably _impure_. Apart from returning a value, the only reason for a function to exist is to perform a side effect. Otherwise it would be a no-op, right?
## Example No-Op
- This function performs a useless computation, since it doesn't return anything or perform a side effect. It's a **no-op**.
```python
def square(x: int) -> None:
    x * x
```
## Example Side Effect
- This function lacks a return statement but performs a _side effect_: it changes the value of the `y` variable that is outside of its scope. It's **impure**.
```python
y: int = 5


def add_to_y(x: int) -> None:
    global y
    y += x


add_to_y(3)
# y = 8
```
>The [`global` keyword](https://docs.python.org/3/reference/simple_stmts.html#global) tells Python to allow modification of the outer-scoped `y` variable.
## Printing Is Impure
- Even the `print` function (technically) has a side effect! It doesn't return anything, but it does print text to the console, which is a form of I/O.
- Some Python built-ins that are definitely worth knowing:
	- [`str.split`](https://docs.python.org/3/library/stdtypes.html#str.split) – splits a string into a list of substrings based on a separator (by default, whitespace)
	- [`str.strip`](https://docs.python.org/3/library/stdtypes.html#str.strip) – removes leading and trailing characters (by default, whitespace) from a string
	- [`map`](https://docs.python.org/3/library/functions.html#map) – applies a function to every item of an iterable (like a list) and returns an iterator of the results
	- [`join`](https://docs.python.org/3/library/stdtypes.html#str.join) – combines an iterable of strings into a single string, with a specified separator between them
- The syntax of `join` is simple but can be a bit counterintuitive:
```python
# We call join as a method on the separator, not on the list of strings
" ".join(["I", "love", "Python"])
# "I love Python"
```
# Memoization
- [Memoization](https://en.wikipedia.org/wiki/Memoization) is a technical term that basically means [caching](https://en.wikipedia.org/wiki/Cache_\(computing\)) (storing a copy of) the result of a computation so that we don't have to compute it again in the future. For example, take this simple function:
```python
def add(x: int, y: int) -> int:
    return x + y
```
- A call of `add(5, 7)` will _always_ evaluate to `12`. If you think about it, once we know that `add(5, 7)` can be replaced with `12`, we can just store `12` in memory as the result value. Then, the next time we need to `add(5, 7)`, we can _look up_ the value instead of repeating a (potentially expensive) CPU operation.
- The slower and more complex the function, the more memoization can help speed things up.
> It's pronounced "memOization," not "memORization." This confused me for quite a while in college. I thought my professor just didn't speak goodly...
# Referential Transparency
- Pure functions are always [referentially transparent](https://www.baeldung.com/cs/referential-transparency#referential-transparency).
- "Referential transparency" is a fancy way of saying that a function call can be _replaced_ by its would-be return value because it's the same every time. **Referentially transparent functions can be safely memoized.** For example `add(2, 3)` can be replaced by the value `5`.
- The great thing about pure functions is that it's _always_ safe to memoize them. Impure functions often can't be memoized because they might perform a side effect in addition to returning a static value, or they might return different values given the same arguments.
## Should I Always Memoize?
- No! Memoization is a _tradeoff_ between memory and speed. If your function is fast to execute, it's probably not worth memoizing, because the amount of memory your program will need to store the results will go way up.
- It's also a bunch of extra code to write, so you should only do it if you have a good reason.
# Recursion
- [Recursion](https://en.wikipedia.org/wiki/Recursion_\(computer_science\)) is a famously tricky concept to grasp, but it's honestly quite simple – don't let it intimidate you! A recursive function is just a function that calls itself.
> Recursion is the process of defining something in terms of itself.
## Example of Recursion
- If you thought loops were the only way to iterate over a list, you were wrong! Recursion is fundamental to functional programming because it's how we iterate over lists while avoiding stateful loops. Take a look at this function that sums the numbers in a list:
```python
def sum_nums(nums: list[int]) -> int:
    if len(nums) == 0:
        return 0
    return nums[0] + sum_nums(nums[1:])


print(sum_nums([1, 2, 3, 4, 5]))
# 15
```
- Don't break your brain on the example above! Let's break it down step by step:
### 1. Solve a Small Problem
- Our goal is to sum all the numbers in a list, but we're not allowed to loop. So, we start by solving the smallest possible problem: summing the first number in the list with the rest of the list:
```python
return nums[0] + sum_nums(nums[1:])
```
### 2. Recurse
- So, what actually happens when we call `sum_nums(nums[1:])`? Well, we're just calling `sum_nums` with a smaller list! In the first call, the `nums` input was `[1, 2, 3, 4, 5]`, but in the next call it's just `[2, 3, 4, 5]`. We just keep calling `sum_nums` with smaller and smaller lists.
### 3. The Base Case
- So what happens when we get to the "end"? `sum_nums(nums[1:])` is called, but `nums[1:]` is an empty list because we ran out of numbers. We need to write a **base case** to stop the madness.
```python
if len(nums) == 0:
    return 0
```
- The "base case" of a recursive function is the part of the function that does _not_ call itself.
### Recursive Calls
- Step into recursive calls, hit the base case, then watch return values unwind.
## Recursion on a Tree
- Recursion is often used in "tree-like" structures. For example:
	- Nested dictionaries
	- File systems
	- HTML documents
	- JSON objects
- That's because trees can have _unknown_ depth. It's hard to write a series of loops because you don't know how many levels deep the tree goes.
```python
for entry_i in directory:
    if entry_i.is_dir:
        for entry_j in entry_i:
            if entry_j.is_dir:
                for entry_k in entry_j:
                    ...
```
## Dangers of Recursion
- Recursion is great because it's simple and elegant (simple != easy). It's often the most straightforward way to solve a problem. But there are some dangers to be aware of:
	1. **Stack overflow:** Each function call requires a bit of memory. So, if you recurse too deeply, you can run out of ["stack" memory](https://en.wikipedia.org/wiki/Stack-based_memory_allocation), which will crash your program. (This is what the famous [website](https://stackoverflow.com/) is named after.)
	2. If you don't have a solid base case, you can end up in an infinite loop (which will likely lead to a stack overflow).
	3. Especially in a language like Python, recursion is often slower than a `for` loop because each function call requires some memory. [Tail call optimization](https://en.wikipedia.org/wiki/Tail_call) can help with this, but Python doesn't support it.
# Function Transformations
- "Function transformation" is just a concise way to describe a specific type of [higher-order function](https://en.wikipedia.org/wiki/Higher-order_function). It's when a function takes a function (or functions) as input and returns a _new_ function. Let's look at an example:
![[Pasted image 20260922021624.png|629]]
```python
from collections.abc import Callable


def multiply(x: int, y: int) -> int:
    return x * y


def add(x: int, y: int) -> int:
    return x + y


# self_math is a higher-order function
# input: a function that takes two arguments and returns a value
# output: a new function that takes one argument and returns a value
def self_math(math_func: Callable[[int, int], int]) -> Callable[[int], int]:
    def inner_func(x: int) -> int:
        return math_func(x, x)

    return inner_func


square_func: Callable[[int], int] = self_math(multiply)
double_func: Callable[[int], int] = self_math(add)

print(square_func(5))
# prints 25

print(double_func(5))
# prints 10
```
- The `self_math` function takes a function that operates on two _different_ parameters (e.g. `multiply` or `add`) and returns a new function that operates on _one_ parameter _twice_ (e.g. `square` or `double`).
## More Transformations
- Here's some example code for you to reference as you work through the assignment:
```python
from collections.abc import Callable


def multiply(x: int, y: int) -> int:
    return x * y


def add(x: int, y: int) -> int:
    return x + y


def self_math(math_func: Callable[[int, int], int]) -> Callable[[int], int]:
    def inner_func(x: int) -> int:
        return math_func(x, x)

    return inner_func


square_func: Callable[[int], int] = self_math(multiply)
double_func: Callable[[int], int] = self_math(add)

print(square_func(5))
# prints 25

print(double_func(5))
# prints 10
```
# Why Transform?

You might be wondering:

- "When would I use function transformations in the real world?"
- "Isn't it simpler to just define functions at the top level of the code, and call them as needed?"

Good questions. To be clear, we don't just transform functions at [runtime](https://en.wikipedia.org/wiki/Execution_\(computing\)#Runtime) for the fun of it! We use advanced techniques like function transformation only when they make our code _simpler than it would otherwise be_.

## Code Reusability
- Creating variations of the same function dynamically can make it a lot easier to _share common functionality_. Take a look at this `formatter` function. It accepts a "pattern" and returns a new function that formats text according to that pattern:
```python
from collections.abc import Callable


def formatter(pattern: str) -> Callable[[str], str]:
    def inner_func(text: str) -> str:
        result: str = ""
        i: int = 0
        while i < len(pattern):
            if pattern[i : i + 2] == "{}":
                result += text
                i += 2
            else:
                result += pattern[i]
                i += 1
        return result

    return inner_func
```
- Now we can create new formatters easily:
```python
bold_formatter: Callable[[str], str] = formatter("**{}**")
italic_formatter: Callable[[str], str] = formatter("*{}*")
bullet_point_formatter: Callable[[str], str] = formatter("* {}")
```
- And use them like this:
```python
print(bold_formatter("Hello"))
# **Hello**
print(italic_formatter("Hello"))
# *Hello*
print(bullet_point_formatter("Hello"))
# * Hello
```
## Closures
- 90% of the time, when I use function transformations, it's because I want to create a **closure**. We'll talk about closures in the next chapter!
# Closures
- A [closure](https://en.wikipedia.org/wiki/Closure_\(computer_programming\)) is a function that references variables from outside its own function body. The function definition _and its environment_ are bundled together into a single entity.
- Put simply, a closure is just a function that **keeps track of some values** from the place where it was _defined_, no matter where it's executed later on.
## Example
- The `concatter()` function returns a function called `doc_builder` (yay higher-order functions!) that has a reference to an _enclosed_ `doc` value.
```python
from collections.abc import Callable


def concatter() -> Callable[[str], str]:
    doc: str = ""

    def doc_builder(word: str) -> str:
        # "nonlocal" tells Python to use the 'doc'
        # variable from the enclosing scope
        nonlocal doc
        if doc:
            doc += " "
        doc += word
        return doc

    return doc_builder


# save the returned 'doc_builder' function
# to the new function 'harry_potter_aggregator'
harry_potter_aggregator: Callable[[str], str] = concatter()
harry_potter_aggregator("Mr.")
harry_potter_aggregator("and")
harry_potter_aggregator("Mrs.")
harry_potter_aggregator("Dursley")
harry_potter_aggregator("of")
harry_potter_aggregator("number")
harry_potter_aggregator("four,")
harry_potter_aggregator("Privet")

print(harry_potter_aggregator("Drive"))
# Mr. and Mrs. Dursley of number four, Privet Drive
```
- When `concatter()` is called, it creates a new "stateful" function that _remembers_ the value of its internal `doc` variable. Each successive call to `harry_potter_aggregator` appends to that same `doc`!
## nonlocal
- Python has a keyword called [`nonlocal`](https://docs.python.org/3/reference/simple_stmts.html#nonlocal) that is _required_ to modify a variable from an enclosing scope. Most programming languages don't require this keyword, but Python does.
# Currying
- Function [currying](https://en.wikipedia.org/wiki/Currying) is a specific _kind_ of function transformation, where we translate a _single_ function that accepts _multiple_ arguments into _multiple_ functions that each accept a _single_ argument.
- This is a "normal" 3-argument function:
```python
box_volume(3, 4, 5)
```
- This is a "curried" _series of functions_ that does the same thing:
```python
box_volume(3)(4)(5)
```
- Here's another example that includes the implementation:
```python
def sum(a: int, b: int) -> int:
    return a + b


print(sum(1, 2))
# prints 3
```
- And the same thing curried:
```python
from collections.abc import Callable


def sum(a: int) -> Callable[[int], int]:
    def inner_sum(b: int) -> int:
        return a + b

    return inner_sum


print(sum(1)(2))
# prints 3
```
- The `sum` function only takes a _single_ input, `a`. It returns a _new_ function that takes a single input, `b`. This new function, when called with a value for `b`, will return the sum of `a` and `b`. _We'll talk later about why this is useful_
## Why Curry?
- It's fairly obvious that:
```python
def sum(a: int, b: int) -> int:
    return a + b
```
- is simpler than:
```python
from collections.abc import Callable


def sum(a: int) -> Callable[[int], int]:
    def inner_sum(b: int) -> int:
        return a + b

    return inner_sum
```
- So why would we _ever_ want to do the more complicated thing? Well, currying can be used to **change a function's signature** to make it conform to a specific shape. For example:
```python
def colorize(converter: Callable[[str], str], doc: str) -> None:
    # ...
    converter(doc)
    # ...
```
- The `colorize` function accepts a function called `converter` as input, and at some point during its execution, it calls `converter` with a single argument. That means that it expects `converter` to accept exactly one argument. So, if I have a conversion function like this:
```python
def markdown_to_html(doc: str, asterisk_style: str) -> str:
    # ...
```
- I can't pass `markdown_to_html` to `colorize` because `markdown_to_html` wants _two_ arguments. To solve this problem, I can curry `markdown_to_html` into a function that takes a single argument:
```python
def markdown_to_html(asterisk_style: str) -> Callable[[str], str]:
    def asterisk_md_to_html(doc: str) -> str:
        # do stuff with doc and asterisk_style...

    return asterisk_md_to_html

markdown_to_html_italic: Callable[[str], str] = markdown_to_html("italic")
colorize(markdown_to_html_italic, doc)
```
# Decorators
- Remember function transformations, where a (higher-order) function takes a function and returns a function with new behavior? [Python decorators](https://docs.python.org/3/glossary.html#term-decorator) offer a kind of [syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar) around that. ("Syntactic sugar" just means "a more convenient syntax.")
**Example:**
```python
from collections.abc import Callable


def vowel_counter(func_to_decorate: Callable[[str], None]) -> Callable[[str], None]:
    vowel_count: int = 0

    def wrapper(doc: str) -> None:
        nonlocal vowel_count
        vowels: str = "aeiou"
        for char in doc:
            if char.lower() in vowels:
                vowel_count += 1
        print(f"Vowel count: {vowel_count}")
        func_to_decorate(doc)

    return wrapper


@vowel_counter
def process_doc(doc: str) -> None:
    print(f"Document: {doc}")


process_doc("What")
# Vowel count: 1
# Document: What

process_doc("A wonderful")
# Vowel count: 5
# Document: A wonderful

process_doc("world")
# Vowel count: 6
# Document: world
```
- The `@vowel_counter` line is "decorating" the `process_doc` function with the `vowel_counter` function. `vowel_counter` is called once when `process_doc` is defined with the `@` syntax, but the `wrapper` function that it returns is called every time `process_doc` is called. That's why `vowel_count` is preserved and printed after each time.
## It's Just Syntactic Sugar
- Python decorators are just another (sometimes simpler) way of writing a higher-order function. These two pieces of code are _identical_:
### With Decorator
```python
@vowel_counter
def process_doc(doc: str) -> None:
    print(f"Document: {doc}")


process_doc("Something wicked this way comes")
```
### Without Decorator
```python
def process_doc(doc: str) -> None:
    print(f"Document: {doc}")


process_doc = vowel_counter(process_doc)
process_doc("Something wicked this way comes")
```
# Args and Kwargs
- In Python, [`*args` and `**kwargs`](https://book.pythontips.com/en/latest/args_and_kwargs.html) allow a function to accept and deal with a _variable_ number of arguments.
	- `*args` collects positional arguments into a _tuple_
	- `**kwargs` collects keyword (named) arguments into a _dictionary_
```python
def print_arguments(*args: object, **kwargs: object) -> None:
    print(f"Positional arguments: {args}")
    print(f"Keyword arguments: {kwargs}")


print_arguments("hello", "world", a=1, b=2)
# Positional arguments: ('hello', 'world')
# Keyword arguments: {'a': 1, 'b': 2}
```
## Positional Arguments
- Positional arguments are the ones you're already familiar with, where the order of the arguments matters. Like this:
```python
def sub(a: int, b: int) -> int:
    return a - b


# a=3, b=2
res: int = sub(3, 2)
# res = 1
```
## Keyword Arguments
- [Keyword arguments](https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments) are passed in by name. _Order does not matter_. Like this:
```python
def sub(a: int, b: int) -> int:
    return a - b


res: int = sub(b=3, a=2)
# res = -1
res = sub(a=3, b=2)
# res = 1
```
## A Note on Ordering
- Any positional arguments _must come before_ keyword arguments. This will _not_ work:
```python
sub(b=3, 2)
```
# More on Decorators
- The `*args` and `**kwargs` syntax is great for decorators that are intended to work on functions with different [signatures](https://developer.mozilla.org/en-US/docs/Glossary/Signature/Function).
## Example
- The `log_call_count` function below doesn't care about _the number or the types_ of the decorated function's (`func_to_decorate`) arguments. It just wants to count how many times the function is called. However, it still needs to _pass_ any arguments through to the wrapped function.
```python
from collections.abc import Callable


def log_call_count(func_to_decorate: Callable[..., object]) -> Callable[..., object]:
    count: int = 0

    def wrapper(*args: object, **kwargs: object) -> object:
        nonlocal count
        count += 1
        print(f"Called {count} times")
        # Pass any and all arguments to the decorated function
        return func_to_decorate(*args, **kwargs)

    return wrapper
```
- `Callable[..., object]` is the most general type hint for a function. It means "any function that takes any arguments and returns anything."
## LRU Cache
- [`lru_cache` from the `functools` module](https://docs.python.org/3/library/functools.html#functools.lru_cache) is both a decorator and an example of memoization.
- LRU stands for "[least recently used](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_Recently_Used_\(LRU\))." It's a type of cache that stores items up to a certain size limit. When it gets full, it makes space for new items by discarding the least recently used items first. The cache can be effective because items that are used a lot – like frequently repeated calls to the same function – are less likely to be discarded. They stay in-cache.
- The `lru_cache` decorator memoizes the inputs and outputs of the decorated function. It speeds up repeated calls to a slow function with the same inputs. A function that reads from disk, makes network requests, or requires a lot of computation could be a good candidate for LRU caching _if_ it also sees many identical calls.
- Here's an example from the Python docs that perfectly illustrates how and why to use `lru_cache`:
```python
from functools import lru_cache


@lru_cache()
def factorial_r(x: int) -> int:
    if x == 0:
        return 1
    else:
        return x * factorial_r(x - 1)


factorial_r(10)  # no cached results; makes 11 recursive calls
# 3628800
factorial_r(5)  # just looks up cached result value
# 120
factorial_r(12)  # makes 2 new recursive calls; the other 11 are cached
# 479001600
```
- Because the `factorial` function is recursive and the inputs are sequential numbers, it does get called repeatedly with the same inputs. Without caching, the function would be called 30 times in the code above. With `lru_cache`, the function is only called 13 times. You don't often need to compute factorials, but this example ties together how to use a decorator _and_ memoization _and_ recursion.
# Sum Types
- Remember when I said, "Pure functions are my favorite part of functional programming"? Well, [sum types](https://en.wikipedia.org/wiki/Tagged_union) are a close second.
- A "sum" type is the opposite of a "product" type. This Python object is an example of a _product_ type:
```python
man.studies_finance = True
man.has_trust_fund = False
```
- The total number of combinations a `man` can have is `4`, the _product_ of `2 * 2`:

| studies_finance | has_trust_fund |
| --------------- | -------------- |
| `True`          | `True`         |
| `True`          | `False`        |
| `False`         | `True`         |
| `False`         | `False`        |
- If we add a third attribute, perhaps a `has_blue_eyes` boolean, the total number of possibilities multiplies again, to `8`!

| studies_finance | has_trust_fund | has_blue_eyes |
| --------------- | -------------- | ------------- |
| `True`          | `True`         | `True`        |
| `True`          | `True`         | `False`       |
| `True`          | `False`        | `True`        |
| `True`          | `False`        | `False`       |
| `False`         | `True`         | `True`        |
| `False`         | `True`         | `False`       |
| `False`         | `False`        | `True`        |
| `False`         | `False`        | `False`       |
- But let's pretend that we live in a world where there are _really_ only [three types of people](https://www.youtube.com/watch?v=tEt0IuQJX2o) that our program cares about:
	1. Dateable
	2. Undateable
	3. Maybe dateable
- We can _reduce_ the number of cases our code needs to handle by using a (fake Pythonic) sum type with only 3 possible _types_:
```python
class Person:
    def __init__(self, name: str) -> None:
        self.name = name


class Dateable(Person):
    pass


class MaybeDateable(Person):
    pass


class Undateable(Person):
    pass
```
- Then we can use the [isinstance](https://docs.python.org/3/library/functions.html#isinstance) built-in function to check if a `Person` is an instance of one of the subclasses. It's a clunky way to represent sum types, but hey, it's Python.
```python
def respond_to_text(guy_at_bar: Person) -> str:
    if isinstance(guy_at_bar, Dateable):
        return f"Hey {guy_at_bar.name}, I'd love to go out with you!"
    elif isinstance(guy_at_bar, MaybeDateable):
        return f"Hey {guy_at_bar.name}, I'm busy but let's hang out sometime later."
    elif isinstance(guy_at_bar, Undateable):
        return "Have you tried being rich?"
    else:
        raise ValueError("invalid person type")
```
## Sum Types vs. Product Types
- A product type combines fields, while a sum type represents one of several alternatives. Each alternative can carry data, like the person's name. To be clear: **Python doesn't really support sum types**. We have to use a workaround and invent our own little system and enforce it ourselves.
# Union Types
- We can simulate the shape of sum types in Python by using classes – like our `MaybeParsed` class with subclasses named `Parsed` and `ParseError`. That's better than nothing, but it's awkward.
- The [type hints](https://docs.python.org/3/library/typing.html) system in modern Python offers a more direct way of describing a value that may be one type or another. We can use what's called a [union type](https://docs.python.org/3/library/stdtypes.html#union-type):
```python
def parse_document(doc_name: str, content: str) -> Parsed | ParseError: ...
```
- The `Parsed | ParseError` annotation means, "This function returns either a `Parsed` value or a `ParseError` value." Crucially, the `|` ("or") operator lets us express that relationship without forcing both classes to inherit from the same parent class. `Parsed` and `ParseError` still need to be real types, but they don't need to belong to a shared class hierarchy.
- A union type can list any number of possible types for a given value. One of the most common use cases is for _optional_ values like `str | None` – i.e., a value that may be a string, or may be `None` if the string isn't available yet or couldn't be retrieved.
- In functional programming, union types are used constantly to make "this or that" situations explicit: _some_ value or _none_; a _result_ from a function or an _error_.
- Python is still ultimately a dynamically typed language. The union type, like other type hints, is meant to help developers and their tools (code editors, type checkers). **It's not enforced at runtime.** But being able to document the shape of your data makes it easier to write robust programs.
# Enums
- So far, we've used classes to model the different cases in a sum type, and union type hints as a simpler way of describing the possible types of a value (albeit with no automatic enforcement at runtime).
- If what you're trying to represent is a **fixed set of values**, you have another good option in Python's type system: [enums](https://docs.python.org/3/library/enum.html).
- Let's say we have a `Color` variable that we want to restrict to only three possible values:
	- `RED`
	- `GREEN`
	- `BLUE`
- We could use a plain old `str` to represent these values, but that's annoying because we have to keep track of the "valid" values and defensively check for invalid ones all over our codebase. Instead, we can use an `Enum`:
```python
from enum import Enum

Color = Enum("Color", ["RED", "GREEN", "BLUE"])
print(Color.RED)  # this works, prints 'Color.RED'
print(Color.TEAL)  # this raises an exception
```
- There is also a manual class-based syntax:
```python
from enum import Enum


class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3


print(Color.RED)  # this works, prints 'Color.RED'
print(Color.TEAL)  # this raises an exception
```
>The class-based syntax is more verbose, but safer because it prevents ambiguity between the variable name and the enum name. With `Color = Enum("Color", ...)`, the string `"Color"` sets the enum class name, while `Color =` assigns that class to a variable. While those names normally _shouldn't_ be different, they _can_ be.
- Now `Color` is a sum type! _At least, as close as we can get in Python._ There are a few benefits:
	1. A `Color` can only be `RED`, `GREEN`, or `BLUE`. If you try to use `Color.TEAL`, Python raises an exception.
	2. There is a central place to see the "valid" values for a `Color`.
	3. Each `Color` has a "name" (e.g. `RED`) and an integer value (e.g. `1`). The value can be useful if you need to store, compare, or serialize the enum in a specific way.
# Sum Types
- Unfortunately, Python does _not_ support sum types as well as some [statically typed](https://developer.mozilla.org/en-US/docs/Glossary/Static_typing) languages.
- Python [doesn't enforce](https://docs.python.org/3/library/typing.html) your types before your code runs. That's why we need this line here to `raise` an `Exception` if a color is invalid:
```python
def color_to_hex(color: Color) -> str:
    if color == Color.GREEN:
        return "#00FF00"
    elif color == Color.BLUE:
        return "#0000FF"
    elif color == Color.RED:
        return "#FF0000"
    # handle the case where the color is invalid
    raise Exception("unknown color")
```
- In a language like [Rust](https://www.rust-lang.org/), which has an exceptionally rich type system, we could write the same thing like this:
```rust
fn color_to_hex(color: Color) -> String {
    match color {
        Color::Green => "#00FF00".to_string(),
        Color::Blue => "#0000FF".to_string(),
        Color::Red => "#FF0000".to_string(),
    }
}
```
- Notice how there isn't any case for an unknown enum variant? That's because the Rust code will _fail to compile_ (a step that happens before the code runs at all) if the types don't line up. The Rust compiler enforces that a `Color` value can only be one of the defined variants, and the `match color` block is required to handle every variant!
- **This static enforcement is a huge benefit of sum types.** It's a shame we can't get that in Python.
# Match
- Let's take another look at our example [`Enum`](https://docs.python.org/3/library/enum.html) from the previous lessons:
```python
from enum import Enum


class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3
```
## Working With Enums
- Python has a [`match` statement](https://docs.python.org/3/tutorial/controlflow.html#match-statements) that tends to be a lot cleaner than a series of `if`/`elif`/`else` statements when we're working with a fixed set of possible values (like a sum type, or more specifically an enum):
```python
def get_hex(color: Color) -> str:
    match color:
        case Color.RED:
            return "#FF0000"
        case Color.GREEN:
            return "#00FF00"
        case Color.BLUE:
            return "#0000FF"

        # default case (invalid Color)
        case _:
            return "#FFFFFF"
```
- If you have _two_ values to match, you can use a `tuple`:
```python
class Shade(Enum):
    LIGHT = 1
    DARK = 2


def get_hex(color: Color, shade: Shade) -> str:
    match (color, shade):
        case (Color.RED, Shade.LIGHT):
            return "#FFAAAA"
        case (Color.RED, Shade.DARK):
            return "#AA0000"
        case (Color.GREEN, Shade.LIGHT):
            return "#AAFFAA"
        case (Color.GREEN, Shade.DARK):
            return "#00AA00"
        case (Color.BLUE, Shade.LIGHT):
            return "#AAAAFF"
        case (Color.BLUE, Shade.DARK):
            return "#0000AA"

        # default case (invalid combination)
        case _:
            return "#FFFFFF"
```
- The values after `match` (`color` and `shade`) are compared against enum members in each `case` (`Color.RED` and `Shade.LIGHT`). If a match is found, the code in the block is executed.