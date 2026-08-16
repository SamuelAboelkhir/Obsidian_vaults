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
## Methods
- A method is a function that's tied directly to a class with access to its properties
```python
class Soldier:
    health: int = 5

    # This is a method that reduces the
    # health of the soldier
    def take_damage(self, damage: int) -> None:
        self.health -= damage


soldier_one = Soldier()
soldier_one.take_damage(2)
print(soldier_one.health)
# prints "3"

soldier_two = Soldier()
soldier_two.take_damage(1)
print(soldier_two.health)
# prints "4"
```
- In python, the first parameter a method takes is always `self` which is the instance of the class that the method is being called on, since a class is just a blueprint and can have multiple copies or instances in memory
- This is a similar concept to `this` in javascript
- `self` being an instance of the class, or in other words, a reference to an object, gives us access to the properties of that particular instance's properties, such as its own variables values
- While methods don't normally need to return anything since they are mainly useful for mutating the class instance's values, it can still be made to `return` of course
	- A good example of this is a `getter` method
```python
class Soldier:
    armor: int = 2
    num_weapons: int = 2

    def get_speed(self) -> int:
        speed = 10
        speed -= self.armor
        speed -= self.num_weapons
        return speed


soldier_one = Soldier()
print(soldier_one.get_speed())
# prints "6"
```
## Constructors
- When defining properties on a class, it's rare to ever do it like this
```python
class Soldier:
    name: str = "Legolas"
    armor: int = 2
    num_weapons: int = 2
```
- Instead, we usually use a constructor function called `__init__`, which is called automatically when a new class instance is created
```python
class Soldier:
    def __init__(self) -> None:
        self.name = "Legolas"
        self.armor = 2
        self.num_weapons = 2
```
- This is usually safer, and makes the starting properties configurable instead of needing to hard code them
```python
class Soldier:
    def __init__(self, name: str, armor: int, num_weapons: int) -> None:
        self.name = name
        self.armor = armor
        self.num_weapons = num_weapons


soldier_one = Soldier("Legolas", 2, 10)
print(soldier_one.name)
# prints "Legolas"
print(soldier_one.armor)
# prints "2"
print(soldier_one.num_weapons)
# prints "10"

soldier_two = Soldier("Gimli", 5, 1)
print(soldier_two.name)
# prints "Gimli"
print(soldier_two.armor)
# prints "5"
print(soldier_two.num_weapons)
# prints "1"
```
## Class variables vs Instance variables
### Instance Variables
- Instance variables are declared in the constructor, and vary from instance to instance (object to object)
```python
class Wall:
    def __init__(self) -> None:
        self.height = 10  # instance variable (per object)


south_wall = Wall()
south_wall.height = 20  # only updates this instance of a wall
print(south_wall.height)
# prints "20"

north_wall = Wall()
print(north_wall.height)
# prints "10"
```
### Class Variables
- Class variables on the other hand are shared between all instances of the class, and are declared at the top level of the class definition
- They're less commonly used than instance variables
```python
class Wall:
    height: int = 10  # class variable (shared across all instances)


south_wall = Wall()
print(south_wall.height)
# prints "10"

Wall.height = 20  # updates all instances of a Wall

print(south_wall.height)
# prints "20"
```
- Class variables are known as static variables in other languages, such as javascript
- Class variables are usually looked down upon and are not really recommended because they make it hard to track which parts of the program are making updates
## Encapsulation
- [Encapsulation](https://en.wikipedia.org/wiki/Encapsulation_\(computer_programming\)) is the practice of hiding complexity inside a ["black box"](https://en.wikipedia.org/wiki/Black_box) so that it's easier to focus on the problem at hand.
- A function is an example of encapsulation, as calling the function only requires the function signature, without any knowledge of the inner workings of said function being necessary
### Public and Private
- Properties and methods on a class are public by default, which means that they are accessible with `.` operator
- Private data members on the other hand are a way to implement encapsulation logic on a class definition
- Two underscores `__` are used in python as a prefix to make a property private
```python
class Wall:
    def __init__(self, armor: int, magic_resistance: int) -> None:
        self.__armor = armor
        self.__magic_resistance = magic_resistance

    def get_defense(self) -> int:
        return self.__armor + self.__magic_resistance


front_wall = Wall(10, 20)

# This results in an error
print(front_wall.__armor)

# This works
print(front_wall.get_defense())
# 30
```
- The point here is to hide away internal properties and methods that you don't need to think about or use when interacting with the class beyond its initial definition
- It's kinda like how the inner workings of your car's power steering are hidden from you. You don't need to be an expert on hydraulic systems to turn the wheel from side to side.
- Note that you won't need to understand all the details behind this, but Python "private" members aren't _truly_ private. Python just changes their names with ["name mangling"](https://docs.python.org/3/reference/expressions.html#private-name-mangling) to make outside access less straightforward.
- Also, keep in mind that **Encapsulation is about organization, not security.**
- Private in python is also not real private, but rather just a convention, as [there are ways to get around](https://stackoverflow.com/questions/3385317/private-variables-and-methods-in-python) the double underscore rule.
## Abstraction
- This concept is very similar to encapsulation but the reasoning behind it is different
### Abstraction vs. Encapsulation
- Abstraction is about _creating a simple interface for complex behavior._ It focuses on what's exposed (public).
- Encapsulation is about _hiding internal state._ It focuses on tucking away the implementation details (private).

- When we're abstracting, we're also encapsulating, and vice versa. The two concepts are so interconnected that differentiating them is almost not worth it
- To reiterate again, abstraction is about presenting a complex implementation with a simple interface, so it focuses on what to show, while encapsulation focuses on hiding the complex implementation, so it's about what to hide instead
- Good abstraction is essential when developing libraries for other developers to use
## How OOP Developers Think
- Classes in object-oriented programming are all about grouping data and behavior _together_ in one place: an _object_. Object-oriented programmers tend to think about programming as a modeling problem. They think:
> "How can I write a `Human` class that holds the **data** and simulates the **behavior** of a real human?"
- To provide some contrast, when functional programmers aren't busy writing white papers, they tend to think of their code as inputs and outputs, and how those inputs and outputs transition the world from one state to the next:
> "My game has 7 humans in it. When one takes a step, what's the next state of the game?"
- OOP isn't the only pattern for organizing code, but it's one of the more popular ones. If you understand multiple ways of thinking about code, you'll be a much better developer overall
![[Pasted image 20260811011004.png]]
## Inheritance
- Pretty much every single language supports abstraction and encapsulation, however, inheritance is the first concept that's actually unique to class-based languages
- Inheritance allows a child class to inherit properties and methods from its parent class
- Here, we have the following classes
```python
class Aircraft:
    def __init__(self, height: int, speed: int) -> None:
        self.height = height
        self.speed = speed

    def fly_up(self) -> None:
        self.height += self.speed
```
```python
class Helicopter:
    def __init__(self, height: int, speed: int) -> None:
        self.height = height
        self.speed = speed
        self.direction = 0

    def fly_up(self) -> None:
        self.height += self.speed

    def rotate(self) -> None:
        self.direction += 90
```
- The helicopter class and aircraft class share a lot of the same code, so instead of this duplication, helicopter can instead inherit the similarities from aircraft, and add its own unique behavior on top
```python
class Helicopter(Aircraft):
    def __init__(self, height: int, speed: int) -> None:
        super().__init__(height, speed)
        self.direction = 0

    def rotate(self) -> None:
        self.direction += 90
```
- The `super()` method is used to access the parent class's constructor and methods
- By using it in helicopter, we're essentially calling the aircraft constructor first, then adding the `direction` property to helicopter
- We can also add another child, jet, since that's also an aircraft
```python
class Jet(Aircraft):
    def __init__(self, speed: int) -> None:
        # Jets always start on the ground
        super().__init__(0, speed)

    def go_supersonic(self) -> None:
        self.speed *= 2
```
- The idea here is similar to taxonomy, since OOP seeks to model the world, so aircraft is like a family, while helicopter and jet are species
- So, inheritance should only really be used, as long as one class, the inheritor, is always a subset of the parent, like a species and family relationship
![[Pasted image 20260811010937.png|472]]
### Inheritance hierarchy
- This is just to re-emphasize that you shouldn't have a class inherit from another just because they have some overlap
- Rather, you only inherit, when class A is included in class B as a more specific version of it
![[Pasted image 20260811011146.png]]
## Wide not deep
- A child class must fit every parent class "type" above it. That just doesn't happen very often in a deep tree. It's more likely that you'll have a base class with many child classes that are slightly different variations of the base. They're all "siblings" of each other.
![[Pasted image 20260811011447.png]]
## Polymorphism
- While inheritance is the most _unique_ trait of object-oriented languages, polymorphism is probably the most powerful. It also is _not_ particularly unique to class-based languages. Polymorphism is the ability of a variable, function or object to take on multiple forms
```python
class Creature:
    def move(self) -> None:
        print("the creature moves")


class Dragon(Creature):
    def move(self) -> None:
        print("the dragon flies")


class Kraken(Creature):
    def move(self) -> None:
        print("the kraken swims")


creatures: list[Creature] = [Creature(), Dragon(), Kraken()]
for creature in creatures:
    creature.move()
# prints:
# the creature moves
# the dragon flies
# the kraken swims
```
- Because all three classes have a `.move()` method, we can shove the objects into a single list, and call the same method on each of them, even though the _implementation_ (method body) is different
- This idea is sometimes referred to as "duck typing". If it looks like a duck, swims like a duck, and quacks like a duck, it's a duck. Or, in our example, if it has a `.move()` method, we can treat it like a `Creature`.
- [[PG Go Main#Interfaces in Go|Go]] implements this concept using interfaces
- Function overloading is another form of polymorphism since it's about having different functions within the same scope, sharing the same name, being different because they have different arguments and bodies
### Operator overloading
- Python has the ability to override how operators work
- This is used to define the behavior of different operators on a class's instances
```python
class Point:
    def __init__(self, x: int, y: int) -> None:
        self.x = x
        self.y = y


p1 = Point(4, 5)
p2 = Point(2, 3)
p3 = p1 + p2
# TypeError: unsupported operand type(s) for +: 'Point' and 'Point'
```
- By using special methods with double underscores [[PG Python dunder]] we can define for example how `+` works on two instances of a custom class
```python
class Point:
    def __init__(self, x: int, y: int) -> None:
        self.x = x
        self.y = y

    def __add__(self, other: "Point") -> "Point":
        x = self.x + other.x
        y = self.y + other.y
        return Point(x, y)


p1 = Point(4, 5)
p2 = Point(2, 3)
p3 = p1 + p2
# p3 is (6, 8)
```
### Overriding Build-in Methods
- Python even allows us to override some of its build-in methods
```python
class Point:
    def __init__(self, x: int, y: int) -> None:
        self.x = x
        self.y = y


p1 = Point(4, 5)
print(p1)
# <Point object at 0xa0acf8>
```
- To make this point class print itself
```python
class Point:
    def __init__(self, x: int, y: int) -> None:
        self.x = x
        self.y = y

    def __str__(self) -> str:
        return f"({self.x},{self.y})"


p1 = Point(4, 5)
print(p1)
# prints "(4,5)"
```
- The `__repr__` works similarly to `__str__` with the difference being is that `__repr__` is mainly meant for debugging rather than printing strings to end users