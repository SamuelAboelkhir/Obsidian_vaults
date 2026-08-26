---
tags:
- Paradigms
- Programming-Language
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Paradigms index|Back to index]]

# Functional programming
## What Is Functional Programming?
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
## Immutability
- In FP, we strive to make data [_immutable_](https://en.wikipedia.org/wiki/Immutable_object). Once a value is created, it cannot be changed. _Mutable_ data, on the other hand, can be changed after it's created.
### Who Cares?
- Immutable data is easier to think about and work with. When 10 different functions have access to the same variable, and you're debugging a problem with that variable, you have to consider the possibility that any of those functions could have changed the value.
- When a variable is immutable, you can be sure that it hasn't changed since it was created. It's a helluva lot easier to work with.
- _Generally speaking, immutability means fewer bugs and more maintainable code._
## Declarative programming
- Functional programming aims to be _declarative_. We prefer to declare _what_ we want the computer to do, rather than muck around with the details of _how_ to do it.
### Declarative Styling
- The following CSS changes all [`button`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) elements to have red text:
```css
button {
  color: red;
}
```
- It does _not_ execute line-by-line like an imperative language. Instead, it simply declares the desired style, and it's up to a web browser to figure out how to apply and display it.
### Imperative Styling
- Unlike functional programming (and CSS), a lot of code is _imperative_. We write out the exact step-by-step implementation details. This Python script draws a red button on a screen using the [Tkinter](https://docs.python.org/3/library/tkinter.html) library:
```python
from tkinter import Button, Tk  # first, import the library

master: Tk = Tk()  # create a window
master.geometry("200x100")  # set the window size
button: Button = Button(master, text="Submit", fg="red")  # create a button
button.pack()
master.mainloop()  # start the event loop
```
## It's Math
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
## Should I Use Functions or Classes?
- **If you're unsure, default to functions.** Reach for classes when you need something long-lived and stateful that would be easier to model if you could share behavior _and data structure_ via inheritance. This is often the case for:
	- Video games
	- Simulations
	- GUIs
- The difference is:
> **Classes** encourage you to think about the world as a hierarchical collection of objects. Objects bundle behavior, data, and state together in a way that draws boundaries between instances of things, like chess pieces on a board.

> **Functions** encourage you to think about the world as a series of data transformations. Functions take data as input and return a transformed output. For example, a function might take the entire state of a chess board and a move as inputs, and return the new state of the board as output.

- _Use what feels right to you in your projects, and adjust and refactor as you improve your skills._
## Debugging FP
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
## Functional vs. OOP
- Functional programming and object-oriented programming are **styles for writing code**. One isn't inherently superior to the other, but to be a well-rounded developer you should understand both well and use ideas from each when appropriate.
- You'll encounter developers who love functional programming and others who love object-oriented programming. However, contrary to popular opinion, FP and OOP are _not_ always at odds with one another. They aren't opposites. Of the four pillars of OOP, [inheritance](https://en.wikipedia.org/wiki/Inheritance_\(object-oriented_programming\)) is the only one that doesn't fit with functional programming.
![[Pasted image 20260823200245.png|366]]
- Inheritance isn't seen in functional code due to the mutable classes that come along with it. Encapsulation, polymorphism and abstraction are still used all the time in functional programming.
- When working in a language that supports ideas from both FP and OOP (like Python, JavaScript, or Go) the best developers are the ones who can use the best ideas from both paradigms effectively and appropriately.
## Statements vs. Expressions
- Studying functional programming is really about returning to the most basic aspects of programming and looking at them in a new way. [Statements](https://en.wikipedia.org/wiki/Statement_\(computer_science\)) and [expressions](https://en.wikipedia.org/wiki/Expression_\(computer_science\)) are a great example of that.
### Statements
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
### Expressions
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
## Expressions Over Statements
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
## Functions As Values
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
## Anonymous Functions
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
