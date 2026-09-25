---
tags: 
- Python
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Python index|Back to index]]
# Python basics
- These notes will not cover every single facet of python, as most of basics are similar to many other languages and not worth going over
- Python is an [_extremely_ popular](https://survey.stackoverflow.co/2025/technology#1-programming-scripting-and-markup-languages) language in the industry, and is well-known for:
	- Backend web servers
	- DevOps and cloud engineering
	- Machine learning
	- Scripting and automation
	- etc...
- A ternary in python
```python
result: float = number / 2 if number % 2 == 0 else (number * 3) + 1
```
## Comments
```python
# speed describes how fast the player
# moves in meters per second
speed = 2

# or multiline

"""
the code found below
will print 'Hello, World!' to the console
"""

print("Hello, World!")
```
- Adding a multiline comment block before a function annotates it which is picked up by LSPs
# Bitwise “&” Operator
- Bitwise operators are similar to logical operators, but instead of operating on boolean values, they apply the same logic to all the bits in a value by column. For example, say you had the numbers `5` and `7` represented in [binary](https://en.wikipedia.org/wiki/Binary_code). You could perform a bitwise AND operation and the result would be `5`.
	- `0101` is 5
	- `0111` is 7
```text
0101
&
0111
=
0101
```
- A `1` in binary is the same as `True`, while `0` is `False`. So really a bitwise operation is just a bunch of logical operations that are completed in tandem by column.
```python
0 & 0 = 0

1 & 1 = 1

1 & 0 = 0
```
- Ampersand `&` is the bitwise AND operator in Python. "AND" is the _name_ of the bitwise operation, while ampersand `&` is the _symbol_ for that operation. For example, `5 & 7 = 5`, while `5 & 2 = 0`.
	- `0101` is 5
	- `0010` is 2
```text
0101
&
0010
=
0000
```
## Binary Notation
- When writing a number in binary, the prefix `0b` is used to indicate that what follows is a binary number. `0b10` is two in binary, but `10` without the `0b` prefix is simply ten.
	- `0b0101` is 5
	- `0b0111` is 7
## Putting It Together
```python
0b0101 & 0b0111
# equals 5

binary_five = 0b0101
binary_seven = 0b0111
binary_five & binary_seven
# equals 5
```
## Guild Permissions
- Sometimes applications store user permissions as binary values. If I have `4` different permissions a user can have, then I can store that as a 4-digit binary number, and if a certain bit is present, I know the permission is enabled. This can be a lot more efficient than storing entire strings.
- Let's pretend we have 4 permissions related to "guilds" in Fantasy Quest ("guild" is just a fancy videogame word for "team"):
	- `can_create_guild` – Leftmost bit (`0b1000`)
	- `can_review_guild` – Second to leftmost bit (`0b0100`)
	- `can_delete_guild` – Second to rightmost bit (`0b0010`)
	- `can_edit_guild` – Rightmost bit (`0b0001`)
- If a user has _no_ permissions, their binary permissions would be `0b0000`.
- If a user only has the `can_create_guild` permission, their binary permissions would be `0b1000`, but a user with `can_review_guild` _and_ `can_edit_guild` permissions would be `0b0101`.
- To check for, say, the `can_review_guild` permission, we can perform a bitwise AND operation on the user's permissions and the enabled `can_review_guild` bit (`0b0100`). If the result is `0b0100` again, we know they have that specific permission!
```python
user_permissions = 0b0101
can_review_guild = 0b0100

# perform bitwise AND to get the user's review permission
user_review_guild_permission = user_permissions & can_review_guild

# check if the user's review permission is equal to `can_review_guild`
```
# Bitwise “|” Operator
- As you may have guessed, the bitwise "or" operator is similar to the bitwise "and" operator in that it works on binary rather than boolean values. However, the bitwise "or" operator "or"s the bits together. Here's an example:
	- `0101` is 5
	- `0111` is 7
```text
0101
|
0111
=
0111
```
- A `1` in binary is the same as `True`, while `0` is `False`. So a bitwise operation is just a bunch of logical operations that are completed in tandem. When two binary numbers are "or"ed together, the result has a `1` in any place where _either_ of the input numbers has a `1` in that place.
- `|` is the bitwise "or" operator in Python. `5 | 7 = 7` and `5 | 2 = 7` as well!
	- `0101` is 5
	- `0010` is 2
```text
0101
|
0010
=
0111
```
## Guild Permissions
- A "guild" is a team of 2-4 players. Here are the guild-specific permissions:
	- `can_invite` – Leftmost bit (`0b1000`)
	- `can_kick` – Second to leftmost bit (`0b0100`)
	- `can_enter_dungeon` – Second to rightmost bit (`0b0010`)
	- `can_surrender` – Rightmost bit (`0b0001`)
- When players are in a guild together, they gain _all_ the permissions of _all_ the other members of the guild!
- For example, if:
	- Jack has the `can_invite` permission: `0b1000`
	- Jill has the `can_kick` permission: `0b0100`
- Then, when they are in a guild together, they should both have the `can_invite` and `can_kick` permissions: `0b1100`.
# Converting Binary
- Fantasy Quest needs to [migrate](https://en.wikipedia.org/wiki/Data_migration) old data from strings that _look like binary_ to the integers that the binary strings represent. For example:
	- `"100" -> 4`
	- `"101" -> 5`
	- `"10010" -> 18`
- The built-in [int()](https://docs.python.org/3/library/functions.html#int) function can convert a binary string to an integer. It takes a second argument that specifies the base of the number (binary is base 2). For example:
```python
# this is a binary string
binary_string = "100"

# convert binary string to integer
num = int(binary_string, 2)
print(num)
# 4
```
# Dictionaries
- [Dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) in Python are used to store data values in `key` -> `value` pairs. Dictionaries are a great way to store groups of information.
```python
# use curly braces
# add key-value pairs
car = {
    "brand": "Toyota",
    "model": "Camry",
    "year": 2019,
}
```
- Here the `car` variable is assigned to a dictionary `{}` containing the keys `brand`, `model` and `year`. The keys' corresponding values are `Toyota`, `Camry` and `2019`.
## Duplicate Keys
- Because dictionaries rely on unique keys, you can't have two of the same key in the same dictionary. If you try to use the same key twice, the first value will simply be overwritten.
## Accessing Dictionary Values
- Dictionary elements must be accessible somehow in code, otherwise they wouldn't be very useful.
- A value is retrieved from a dictionary by specifying its corresponding key in square brackets. The square brackets look similar to indexing into a list.
```python
car = {"make": "Toyota", "model": "Camry"}
print(car["make"])
# Prints: Toyota
```
## Setting Dictionary Values
- You don't need to create a dictionary with values already inside. It is common to create a blank dictionary then populate it later using dynamic values. The syntax is the same as getting data out of a key, just use the assignment operator "=" o give that key a value.
```python
planets = {}
planets["Earth"] = True
planets["Pluto"] = False
print(planets["Pluto"])
# Prints False
```
## Updating Dictionary Values
- If you try to set the value of a key that already exists, you'll end up just updating the value of that key.
```python
planets = {
    "Pluto": True,
}
planets["Pluto"] = False
print(planets["Pluto"])
# Prints False
```
## Deleting Dictionary Values
- You can delete existing keys using the `del` keyword.
```python
names_dict = {"jack": "bronson", "jill": "mcarty", "joe": "denver"}

del names_dict["joe"]

print(names_dict)
# Prints: {'jack': 'bronson', 'jill': 'mcarty'}
```
## Deleting Keys That Don't Exist
- Notice that if you try to delete a key that doesn't exist, you'll get an _error_.
```python
names_dict = {"jack": "bronson", "jill": "mcarty", "joe": "denver"}

del names_dict["unknown"]
# ERROR HERE, key doesn't exist
```
## Counting Practice

### Checking for Existence
- If you're unsure whether a key exists in a dictionary, use the `in` keyword.
```python
cars = {"ford": "f150", "toyota": "camry"}

print("ford" in cars)
# Prints: True

print("gmc" in cars)
# Prints: False
```
# Iterating Over a Dictionary in Python
- We can iterate over a dictionary's keys using the same no-index syntax we used to iterate over values in a list. Use each key in square brackets to get its value.
```python
fruit_sizes = {"apple": "small", "banana": "large", "grape": "tiny"}

for name in fruit_sizes:
    size = fruit_sizes[name]
    print(f"name: {name}, size: {size}")

# name: apple, size: small
# name: banana, size: large
# name: grape, size: tiny
```
>We could have just as easily set the `name` variable to `key` or simply `k`.

## Tip: Negative Infinity
- When you're trying to find a "max" value, it helps to keep track of the "max so far" in a variable and to start that variable at the lowest possible number, negative infinity.
```python
max_so_far = float("-inf")
```
# Sets
- [Sets](https://docs.python.org/3/tutorial/datastructures.html#sets) are _like_ Lists, but they are _unordered_ and they guarantee _uniqueness_. Only _ONE_ of each value can be in a set.
```python
fruits = {"apple", "banana", "grape"}
print(type(fruits))
# Prints: <class 'set'>

print(fruits)
# Prints: {'banana', 'grape', 'apple'}
```
## Add Values
- You can [`.add()`](https://docs.python.org/3/library/stdtypes.html#set.add) values to a set. Think of `.add()` like `append` but for sets!
```python
fruits = {"apple", "banana", "grape"}
fruits.add("pear")
print(fruits)
# Prints: {'pear', 'banana', 'grape', 'apple'}
```
- No error will be raised if you add an item already in the set, and the set will remain unchanged.
## An Empty Set
- Because the empty bracket `{}` syntax creates an empty dictionary, to create an _empty_ set, you need to use the `set()` function.
```python
fruits = set()
fruits.add("pear")
print(fruits)
# Prints: {'pear'}
```
## Set Iteration
```python
fruits = {"apple", "banana", "grape"}
for fruit in fruits:
    print(fruit)
    # Prints:
    # banana
    # grape
    # apple
```
- Note: Sets are unordered, so the order of iteration is _not_ guaranteed.
## Converting Collections
- The `set()` function can convert a list to a set, and `list()` can convert it back:
```python
fruits = ["apple", "banana", "apple"]
unique_fruits = set(fruits)
fruits = list(unique_fruits)
```
## Set Subtraction
- You can use some of the "normal" mathematical operations on sets. For example, you can subtract one set from another. It removes all the values in the second set from the first set.
```python
set1 = {"apple", "banana", "grape"}
set2 = {"apple", "banana"}
set3 = set1 - set2

print(set3)
# Prints: {'grape'}
```
### Tips
- Convert a List to a Set with [set()](https://docs.python.org/3/library/stdtypes.html#set): `set(my_list)`.
- You can subtract the elements in one Set from another Set using the `-` operator.
# Errors and Exceptions in Python
- You've probably encountered some errors in your code from time to time if you've gotten this far in the course. In Python, there are two main kinds of distinguishable errors:
	- Syntax errors
	- Exceptions
## Syntax Errors
- You probably know what these are by now. A syntax error is just the Python interpreter telling you that your code isn't adhering to proper Python syntax.
```python
this is not valid code, so it will error
```
- If I try to run that sentence as if it were valid code I'll get a syntax error:
```text
this is not valid code, so it will error
      ^
SyntaxError: invalid syntax
```
## Exceptions
- Even if your code has the right syntax, however, it may still cause an error when an attempt is made to execute it. Errors detected during execution are called "exceptions" and can be handled gracefully by your code. You can even raise your own exceptions when bad things happen in your code.
- Python uses a [try-except](https://docs.python.org/3/tutorial/errors.html#handling-exceptions) pattern for handling errors.
```python
try:
    10 / 0
except Exception:
    print("can't divide by zero")
```
- The `try` block is executed until an exception is raised or it completes, whichever happens first. In this case, an exception is raised because division by zero is impossible. The `except` block is only executed if an exception is raised in the `try` block.
- If we want to access the data from the exception, we use the following syntax:
```python
try:
    10 / 0
except Exception as e:
    print(e)

# prints "division by zero"
```
- Wrapping potential errors in `try/except` blocks allows the program to handle the exception gracefully without crashing.
# Raising Your Own Exceptions
- Errors are _not_ something to be scared of. Every program that runs in production is expected to manage errors on a constant basis. Our job as developers is to handle the errors gracefully and in a way that aligns with our user's expectations.
## Errors Are NOT Bugs
- When something in our own code happens that isn't the "happy path," we should raise our own exceptions. For example, if someone passes some bad inputs to a function we write, we should not be afraid to raise an exception to let them know they did something wrong.
- An _error_ or _exception_ is raised when something bad happens, but as long as our code handles it as users expect it to, it's _not_ a bug. A bug is when code behaves in ways our users don't expect it to.
- For example, if a player tries to forge a sword out of a metal bar, we might stop that from happening by using `raise` to prevent a _bug_. If the game doesn't have certain items, such as a gold sword, then players shouldn't be able to craft a sword from gold bars even though gold bars do exist.
```python
def craft_sword(metal_bar):
    if metal_bar == "bronze":
        return "bronze sword"
    if metal_bar == "iron":
        return "iron sword"
    if metal_bar == "steel":
        return "steel sword"
    raise Exception("invalid metal bar")
```
- We prevent a bug by _raising an exception_. This exception prevents other developers who use the `craft_sword` function from creating items that don't exist in our game.
- `raise` stops the program from executing and forces the exception to be handled.
## Don't Catch Your Own Exceptions
- As a rule of thumb, you do not want to catch exceptions you raise within the same function block, for example:
```python
# don't do this
def craft_sword(metal_bar):
    try:
        if metal_bar == "bronze":
            return "bronze sword"
        if metal_bar == "iron":
            return "iron sword"
        if metal_bar == "steel":
            return "steel sword"
        raise Exception("invalid metal bar")
    except Exception as e:
        print(f"An error occurred: {e}")
```
- Instead, the caller should handle any potential error by wrapping the function call within a try/except block.
```python
# do this
try:
    craft_sword("gold bar")
except Exception as e:
    print(e)
```
- By raising the exception instead of handling it inside `craft_sword`, we let the caller decide how to proceed. The caller might want to log the error, show a message to the player, or crash the program entirely.
# Different Types of Exceptions
- We haven't covered classes and objects yet, which is what an `Exception` really is at its core. We'll go more into that in the course on object-oriented programming.
- For now, what is important to understand is that there are different types of exceptions, and we can handle them differently depending on the situation. Some exceptions are more specific, like `ZeroDivisionError` (which happens when you divide by zero) or `IndexError` (which happens when you try to access a list element at an invalid index – either too high or too low). Others are more general, like the base `Exception`.
## Syntax
```python
try:
    10 / 0
except ZeroDivisionError:
    print("0 division")
except Exception as e:
    print(e)

try:
    nums = [0, 1]
    print(nums[2])
except IndexError:
    print("index error")
except Exception as e:
    print(e)
```
- Which will print:
```text
0 division
index error
```
## Why Specific Exceptions Come First
- When handling exceptions, it's important to catch the **most specific ones** first, because Python stops checking once it finds a matching exception handler. If you catch a more general Exception first, any specific errors will never get handled individually.
- For example:
```python
try:
    nums = [0, 1]
    print(nums[2])
except Exception:
    print("An error occurred")
except IndexError:
    print("Index error")
```
- In this case, the general Exception will catch the error before the `IndexError` can be reached, and the message "Index error" will never be printed. Always handle the most specific exception first!
## Alias Exception Messages
- As seen in the example, you can also access the error using `as`, like this:
```python
except Exception as e:
    print(e)
```
- The default behavior of `print` is that it will print the string representation of whatever object is passed to it. In this case, it will print the error message.
# Type Hints
- Some functions accept [numbers](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex) as arguments; others accept [strings](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str). Some return [lists](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range); others return [dictionaries](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict), [booleans](https://docs.python.org/3/library/stdtypes.html#boolean-type-bool), or [`None`](https://docs.python.org/3/reference/datamodel.html#none)
- When a program is small, you can _usually_ remember the types of your variables. But as programs grow, it's easy to forget:
	- Is `level` an `int` or a `str`?
	- Does `get_item()` always return an item name (`str`), or sometimes `None` if it can't find one?
	- Is `inventory` a list of strings, or a dictionary of item counts?
- [Type hints](https://docs.python.org/3/library/typing.html) let us write those expectations directly in our code:
```python
def get_damage(weapon: dict, level: int) -> int:
    return weapon["damage"] + (level * 2)
```
- The `weapon: dict`, `level: int`, and `-> int` parts are type hints. They tell humans _and_ code editors what kinds of values the function expects and returns.
- Type hints _don't make Python stop being Python_. It's still a [dynamically typed](https://en.wikipedia.org/wiki/Type_system#Dynamic_type_checking_and_runtime_type_information) language, and it won't automatically reject the wrong value just because a type hint says so.
- Type hints are for:
	- Making code easier to read
	- Helping your editor autocomplete and warn you about mistakes
	- Making bugs easier to spot before running your code
## Basic Types
- To add a type hint to a variable declaration, put a colon after the variable name, then the type. This comes _before_ the equals sign and the value:
```python
character_name: str = "Sir Galahad"
character_level: int = 7
character_health: float = 72.5
has_magic: bool = True
```
- _The values work the exact same way they did before._ In fact, when it comes to simple variable declarations like this, you don't actually _need_ the hint. In this example:
```python
character_health = 72.5
```
- Because `character_health` is assigned a value of `72.5`, your tooling can _infer_ that it's a `float`. That said, if you also want to _see_ the type name, you can optionally add it.
## Function Parameters
- [Function parameters](https://docs.python.org/3/glossary.html#term-parameter) can have type hints too! The syntax is the same as variable type hints: put a colon after the parameter name, then the type.
```python
def greet_player(name: str):
    print(f"Welcome, {name}!")
```
- When a function has multiple parameters, each one can have its own type hint:
```python
def add_gold(current_gold: int, found_gold: int):
    return current_gold + found_gold
```
- While adding a type hint to a variable declaration like:
```python
character_health: float = 72.5
```
- is considered a bit _redundant_ due to type inference, adding type hints to function parameters is _not_ redundant. If you don't add them, your tooling won't know what types the function expects, which makes autocomplete and error checking less effective.
>Hover your cursor over the `status` variable. See how the tooltip can show you that it's a string? That's what makes type hinting useful! Note that `name`, `level`, `health`, and `has_magic` are _all "unknown"_ because Python can't infer function parameter types without hints.
## Return Types
- You can _also_ annotate the type that you expect a function to [return](https://docs.python.org/3/reference/simple_stmts.html#the-return-statement). When you know what types go into and come out of a function, you can (probably) use it without having to read every line of the function body. Return types come after the parameter list, before the colon:
```python
def add_gold(current_gold: int, found_gold: int) -> int:
    return current_gold + found_gold
```
- The `-> int` means this function is _expected_ to return an integer.
- **The syntax is a bit different** from type hints on variables and parameters: we use `->` instead of `:`, and there's no variable name before the type hint. This is because it doesn't really matter what name (if any) the function uses internally for the return value; we just care about the type.
- Here's another example:
```python
def get_greeting(player_name: str) -> str:
    return f"Welcome, {player_name}!"
```
- The `-> str` means this function is expected to return a string.
## List and Set Hints
- We've covered hints for **basic types** like `str`, `int`, `float`, and `bool`, but you can also add hints for **container types**: types that _hold other values_. For example:
	- [`list`](https://docs.python.org/3/library/stdtypes.html#lists): mutable sequence of values
	- [`set`](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset): unordered collection of unique values
	- [`dict`](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict): collection of key-value pairs
	- [`tuple`](https://docs.python.org/3/library/stdtypes.html#tuples): immutable sequence of values
- When we type-hint a container, we specify what kind of container it is _and_ what type of values it contains. For example, a _list_ of _strings_ can be expressed as `list[str]`:
```python
inventory: list[str] = ["Iron Sword", "Healing Potion"]
```
- The "contained" type goes in square brackets after the container type. Similarly, for a _set_ of _strings_, we would write `set[str]`:
```python
unique_items: set[str] = {"Iron Sword", "Healing Potion
```
## Dictionary Hints
- [Dictionaries](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict) are container types too, but they map **keys** to **values**, so their type hints include _both_:
```python
item_counts: dict[str, int] = {
    "Wooden Arrow": 30,
    "Small Amethyst": 2,
}
```
- The first type is for the keys; the second is for the values.
```python
dict[key_type, value_type]
```
- So `dict[str, int]` means:
	- The keys are strings
	- The values are integers
- Not all types [can be used as dictionary _keys_](https://docs.python.org/3/glossary.html#term-hashable). The key types that you'll see most often are strings and integers. Dictionary _values_, on the other hand, can be any type.
## Tuple Hints
- Lists and sets _usually_ hold multiple values of the same type:
```python
inventory: list[str] = ["Black Knight Halberd", "Skull Lantern", "Notched Whip"]
```
- But [tuples](https://docs.python.org/3/library/stdtypes.html#tuples) are a **small fixed group of values** where each _position_ has its own meaning. Because they're fixed, it's quite common for those values to be of different types. For example, a loot drop might have an item name and a quantity:
```python
drop: tuple[str, int] = ("Garnet Mark", 2)
```
- `tuple[str, int]` means:
	- There are two values in the tuple
	- The first value is a string
	- The second value is an integer
- A tuple can have any number of values – though `2` and `3` are the most common. Here's an example representing a character's HP, MP, and stamina:
```python
stats: tuple[int, float, int] = (100, 42.5, 75)
```
- The type hint `tuple[int, float, int]` tells us this is a three-value tuple with an integer, a float, and another integer.
## Specific Container Types
- It's possible to type-hint a container with _just_ the container type:
```python
items: list = ["Black Firebomb", "Titanite Chunk"]
```
- This says `items` is a list, but it doesn't tell us what _kind of values_ go inside! Assuming you know what's inside, best to be specific:
```python
items: list[str] = ["Black Firebomb", "Titanite Chunk"]
```
- That said, bare container type hints aren't _wrong_. Sometimes you really _don't know_ what types of values a container will hold, or the specific type hint would be too complicated to be useful. You'll see that occasionally with `dict`s. Just give clear type hints whenever possible!
## Nested Types
- We've looked at relatively simple container types like `list[str]`, but they can get more complex when _one container holds another container_. That is, it's possible to have **nested container types**.
- A dictionary, for example, could map each character's _name_ to their _list of spells_:
```python
character_spells: dict[str, list[str]] = {
    "Gandalf": ["Fireball", "Light"],
    "Frodo": ["Hide"],
}
```
- We read `dict[str, list[str]]` from the outside in:
	- It's a dictionary (`dict`)
	- Each _key_ is a string (`str`)
	- Each _value_ is a list of strings (`list[str]`)
- In extreme cases, nested types can get _super_ confusing, but honestly it's less confusing than it would be _without_ the typing. For the time being, just know that type hints _can_ describe containers within containers.
## Optional Values
- Sometimes we work with variables that [may or may not](https://docs.python.org/3/library/typing.html#typing.Optional) have an "actual value." For example, a character _might_ have a damage bonus, or they might not. If they _don't_, we can represent that lack of value with [`None`](https://docs.python.org/3/reference/datamodel.html#none).
- The [`|` operator](https://docs.python.org/3/library/typing.html#typing.Optional) indicates that a value can be of multiple types:
```python
damage_bonus: int | None
```
- That means `damage_bonus` can be either an integer (the bonus amount) _or_ `None`. For another example, a function might return a prepared spell if one is ready, or `None` if no spell is prepared:
```python
def get_prepared_spell(has_spell: bool) -> str | None:
    if has_spell:
        return "Fireball"

    return None
```
## Fix Code With Type Hints
- When type hints and code behavior disagree, _one of them is wrong_.
- If you know that you've properly typed a function signature, you can use it as the "source of truth," or as a clue for how the implementation should work.
# The Zen of Python
Tim Peters, a long time Pythonista, describes the guiding principles of Python in his famous short piece, [The Zen of Python.](https://peps.python.org/pep-0020/)
```
Beautiful is better than ugly.

Explicit is better than implicit.

Simple is better than complex.

Complex is better than complicated.

Flat is better than nested.

Sparse is better than dense.

Readability counts.

Special cases aren't special enough to break the rules.

Although practicality beats purity.

Errors should never pass silently.

Unless explicitly silenced.

In the face of ambiguity, refuse the temptation to guess.

There should be one-- and preferably only one --obvious way to do it.

Although that way may not be obvious at first unless you're Dutch.

Now is better than never.

Although never is often better than right now.

If the implementation is hard to explain, it's a bad idea.

If the implementation is easy to explain, it may be a good idea.

Namespaces are one honking great idea -- let's do more of those!
```