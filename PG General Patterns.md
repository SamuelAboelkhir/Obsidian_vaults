---
tags: 
- Other
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]
# String interpolation
- The act of passing the value of a variable in a string
- Most languages have a method that's similar to C (the G.O.A.T)
```C
  printf("Hello %s", name)
```

```typescript
const name = "Alice";
const age = 30;

// Template literals with ${}
const message = `Hello, ${name}! You are ${age} years old.`;
console.log(message); // "Hello, Alice! You are 30 years old."

// Expressions in template literals
const result = `2 + 2 = ${2 + 2}`;
console.log(result); // "2 + 2 = 4"

// Multi-line strings
const multiline = `
Name: ${name}
Age: ${age}
Year born: ${new Date().getFullYear() - age};
```

```python
name = "Alice"
age = 30
  
// F-strings
message = f"Hello, {name}! You are {age} years old."
print(message)  # "Hello, Alice! You are 30 years old.

// Expressions in f-strings
result = f"2 + 2 = {2 + 2}"
print(result)  # "2 + 2 = 4"

// Format specifiers
pi = 3.14159
formatted = f"Pi is approximately {pi:.2f}"
print(formatted)  # "Pi is approximately 3.14"

// .format() Method
message = "Hello, {}! You are {} years old.".format(name, age)

// Can also use named placeholders
message = "Hello, {name}! You are {age} years old.".format(name=name, age=age)
```

```java
String name = "Alice";
int age = 30;

// String.format()
String message = String.format("Hello, %s! You are %d years old.", name, age);
System.out.println(message);

// System.out.printf() (direct printing)
System.out.printf("Hello, %s! You are %d years old.%n", name, age);
```

```lua
 Lua
function greet(name)
print("Hello, " .. name)
end
```
# Type systems
- A **type system** is a set of rules that determines:
	- what types values and expressions have
	- which operations are valid for those types
	- when and how type errors are detected
	- how different types are considered compatible with each other
- Type systems have several **independent dimensions**. Two particularly useful dimensions are:
    - **When are types checked?**
        - Static typing
        - Dynamic typing
    - **How are types considered compatible?**
        - Nominal typing
        - Structural typing
- **Duck typing** is related to structural typing, but is better thought of as a programming style/behavior rather than a strict alternative to static/dynamic typing.
## Static typing
- In a **statically typed** language, type checking happens before the program executes, typically during compilation.
- The compiler determines the type of expressions and variables and verifies that operations performed on them are valid.
- Static typing does **not** necessarily mean that I have to explicitly write every type. The compiler can often **infer** types.
### Explicit types
```ts
let age: number = 25;

age = "hello";
// ❌ Type error
```
### Implicit types
```ts
let age = 25;

age = "hello";
// ❌ Type error
```
### Examples of statically typed languages
- C
- C++
- Java
- C#
- Rust
- Go
- Kotlin
- TypeScript
## Dynamic typing
- In a **dynamically typed** language, type information is associated with values at runtime rather than being fixed and checked entirely before execution.
- A variable can generally refer to values of different types during the execution of the program.
- This does **not** mean that the language doesn't have types. Python, for example, has integers, strings, lists, objects, etc.; the distinction is **when type constraints are checked**.
```python
x = 10

x = "hello"

x = [1, 2, 3]
```
### Examples of dynamically typed languages
- Python
- JavaScript
- PHP
- Lua
## Duck typing
- This isn't exactly a system with a specific implementation
- Languages that allow duck typing basically mean that if I try to use a property from an object, and it has it, then in that context, it fullfils that type
- It's based on the saying "_If it quacks like a duck, and walks like a duck, then it's a duck_"
```python
class Duck:
    def quack(self):
        print("Quack!")


class Robot:
    def quack(self):
        print("Beep quack!")


def make_it_quack(thing):
    thing.quack()


make_it_quack(Duck())
make_it_quack(Robot())
```
### Duck typing vs structural typing
- The important distinction is that **structural typing is enforced by the type system**, whereas duck typing generally discovers compatibility through actual use at runtime.
- For example, Python can happily accept:
```python
def make_it_quack(x):
    x.quack()
```
- and only discover the problem when:
```python
make_it_quack(Car())
```
- actually executes.
- A statically typed structural type system can catch the equivalent problem **before execution**.
## Structural typing
- This one is fairly similar to duck typing, but the difference is that it requires the actual interface to exist
- In the language, you don't need to explicitly mention that your type implements the interface though. As long as it structurally matches it, then it implements it
- A good example here is Go, which uses structural typing and static typing heavily
```Go
type Duck interface {
	quack()
}

type Human struct {}

func (*Human) quack() {
	fmt.Println("I quacked")
}

func canQuack(x Duck) {
	x.quack()
}

func main(){
	canQuack(&Human{})
}
```
### Languages with structural typing
- TypeScript
- Go
## Nominal typing
- This is typing with interfaces and explicit interface assignment
- This means that this is a case were we need to explicitly use the `implements` key word, and similar keywords, to say that a type implements an interface
- Two types can have identical methods but still be considered different, incompatible types unless an explicit relationship exists
```java
interface Duck {
    void quack();
}

class Robot implements Duck {
    @Override
    public void quack() {
        System.out.println("Beep quack!");
    }
}
```
### Examples of predominantly nominally typed languages
- Java
- C#
- C++
- Rust
- Kotlin

| Language       | Typing time | Compatibility                                             |
| -------------- | ----------- | --------------------------------------------------------- |
| **TypeScript** | Static      | Structural                                                |
| **Go**         | Static      | Structural                                                |
| **Java**       | Static      | Primarily nominal                                         |
| **C#**         | Static      | Primarily nominal                                         |
| **Python**     | Dynamic     | Commonly duck-typed                                       |
| **JavaScript** | Dynamic     | Commonly duck-typed                                       |
| **Rust**       | Static      | Primarily nominal                                         |
| **C++**        | Static      | Primarily nominal, with some structural/template behavior |