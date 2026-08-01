---
tags:
- paradigms
- programming-language
moc: programming
---

[[_0000 home|home]] | [[_0006 programming moc|back to programming moc]] | [[pg paradigms index|back to index]]
# The world of OOP
- [Object-Oriented Programming](https://en.wikipedia.org/wiki/Object-oriented_programming), or "OOP", is a pattern for (allegedly) writing **clean and maintainable code**.
## Clean Code
- Paradigms like [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) and [functional programming](https://en.wikipedia.org/wiki/Functional_programming) are all about making code easier to work with. After all, programmers are just feeble humans. Code that's easy for humans to understand is called "clean code".
> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.
> 
> -- Martin Fowler
### Clean Code Does Not
- Make your programs run faster
- Make your programs function correctly
- Only occur in object-oriented programming
### Clean Code Does
- Make code easier to work with
- Make it easier to find and fix bugs
- Make the development process faster
- Help us retain our sanity
## DRY Code
- Another "rule of thumb" for writing maintainable code is "Don't Repeat Yourself" (DRY). It means that, when possible, you should _avoid writing the same code in multiple places_. Repeating code can be bad because:
	- If you need to change it, you have to change it in multiple places
	- If you forget to change it in one place, you'll have a bug
	- It's more work to write it over and over again
## Classes
- A [class](https://docs.python.org/3/tutorial/classes.html) is a special type in an object-oriented programming language like Python. If you squint really hard, it's kinda like a dictionary in that it usually stores name-value pairs:
```python
# Defines a new class called "Soldier"
# with three properties: health, armor, damage
class Soldier:
    health: int = 5
    armor: int = 3
    damage: int = 2
```
- Just like a string, integer or float, a class is a _type_, but instead of being a built-in type, classes are custom types that you define.
## Objects
- So if a class is a new custom type, what's an object? Objects are just [instances](https://stackoverflow.com/questions/20461907/what-is-meaning-of-instance-in-programming) of a class.
- For example:
```python
health = 50
# health is an instance of an integer type
aragorn = Soldier()
# aragorn is an instance of the Soldier class type
```
- Each new instance of a class is an "object".
### Example
```python
class Archer:
    health: int = 40
    arrows: int = 10

# Create several instances of the Archer class
legolas = Archer()
bard = Archer()

# Print class properties
print(legolas.health)  # 40
print(bard.arrows)  # 10
```
