---
tags:
- Go
- General
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Go index|Back to index]]
# Basic Variables
- `bool`: a boolean value, either `true` or `false`
- `string`: a sequence of characters
- `int`: a signed integer
- `float64`: a floating-point number
- `byte`: exactly what it sounds like: 8 bits of data
## Declaration
- Variables can be declared in 2 ways
```Go
//Sad way
var mySkillIssues int //Defaults to 0
mySkillIssues = 42 //Overwrites with 42

//GOATed way
//int
mySkillIssues := 42 //Using a walrus operator to infer the type

//float64
pi := 3.14159

//string
message := "Hello, world!"

//bool
isGoat := true
```
- Walrus operators should always be used instead of the normal declaration, but they can't be used outside of functions in the [global/package scope](https://dave.cheney.net/2017/06/11/go-without-package-scoped-variables)
## Go is GOATed
![[Go_is_GOATed.png]]
- Go also has a small memory footprint, and is very lightweight
- Go programs include a small amount of extra code that's included in the executable binary, which is the Go Runtime
- This runtime also utilizes a garbage collector that frees up unused memory automatically
## Comments
- Comments in Go are just like C, which makes sense since C's creators made Go, however, we will see some vast differences between the two languages later
## The Compilation Process
![[the_compilation_process.png]]
- Go programs have a somewhat similar structure to Java
```Go
//1. lets the Go compiler know that we want this code to compile and run as a standalone program, as opposed to being a library that's imported by other programs.
package main

//2. imports the [`fmt` (formatting) package](https://pkg.go.dev/fmt) from the [standard library](https://pkg.go.dev/std). It allows us to use `fmt.Println` to print to the console.
import "fmt"

//3. defines the `main` function, the entry point for a Go program.
func main() {
	fmt.Println("The compiled textio server is starting")
}
```
- There are two kinds of errors in programming:
	1. **Compilation errors.** Occur when code is compiled. It's generally better to have compilation errors because they'll never accidentally make it into production. You can't ship a program with a compiler error because the resulting executable won't even be created.
	2. **Runtime errors.** Occur when a program is running. These are generally worse because they can cause your program to crash or behave unexpectedly.
- Go is fast and compiled
- It runs faster than interpreted languages, but it doesn't run quit as fast as compiled languages since it's garbage collected. However, it does compile much faster than them
## Type Sizes
- units, floats, and complex numbers have type sizes
- **Signed integers** (no decimal)
```go
int  int8  int16  int32  int64
```
- **Unsigned integers** (non-negative numbers/no decimal)
```go
uint uint8 uint16 uint32 uint64 uintptr
```
- **Signed decimal numbers**
```go
float32 float64
```
- **Complex numbers** (a complex number has a real and imaginary part)
```go
complex64 complex128
```
- The default int and uint depend on the environment
- Generally speaking though, you should use the standard sizes unless there is a reason not to, due to performance for example
- The standard sizes are:
	- `int`
	- `uint`
	- `float64`
	- `complex128`
- You can also convert between some types
```Go
temperatureFloat := 88.26
temperatureInt := int64(temperatureFloat)
```
- Go is statically typed, and forces you to specify the type as you code
- This is pretty handy because it means your editor and compiler can catch type errors during development, as opposed to dynamically typed languages, where only catch these errors as you run the code
## Concatenating Strings
- You can concatenate two strings with the `+` operator, but you can't concat a string with an int or float
## Same Line Declarations
- Go also allows for same line declarations
```Go
mileage, company := 80276, "Toyota"
```
## Constants
- Constants are also unable of using the walrus operator
```Go
const pi = 3.14159
```
- They can be primitive types only, not complex types, and they must be known at compile time
- They're usually declared with a static value, but they can also be computed as long as this computation happens at compile time. You can't declare a constant with a value that's only computed at run-time though
## Formatting Strings in Go
- Go also uses `Printf` and `Sprintf` that are available in the C family
- Go has a default format formatter `%v` that can be used as a catchall
```Go
s := fmt.Sprintf("I am %v years old", 10)
// I am 10 years old

s := fmt.Sprintf("I am %v years old", "way too many")
// I am way too many years old
```
## Runes and String Encoding
- Many programming languages a character is a single byte, and we use [[PG Character Encoding#ASCII]] to represent 128 characters using 7 bits, which is enough for the English alphabet, numbers, and some special characters
- In Go, we have a special called "rune", which is an alias for int32
- This means a rune is large enough to hold any [[PG Character Encoding#Unicode]]
- Go uses [[PG Character Encoding#UTF-8 Code Groups by Values]]
- There are 2 main takeaways:
	1. When you need to work with individual characters in a string, you should use the `rune` type. It breaks strings up into their individual characters, which can be more than one byte long.
	2. We can include a wide variety of Unicode characters in our strings, such as emojis and Chinese characters, and Go will handle them just fine.
# Conditionals
- I don't know why, like seriously I don't know why, but Go conditionals, do not use _parentheses_ around conditions. Why????
```Go
if height > 6 {
    fmt.Println("You are super tall!")
} else if height > 4 {
    fmt.Println("You are tall enough!")
} else {
    fmt.Println("You are not tall enough!")
}
```
- Also in Go, you **MUST** put the opening brace on the same line as the condition and not on a new line
- Go too can have initial statements in the condition
```Go
//Instead of
length := getLength(email)
if length < 1 {
    fmt.Println("Email is invalid")
}

//We can do
if length := getLength(email); length < 1 {
    fmt.Println("Email is invalid")
}
```
## Switch
- Go switches don't have `break` as they break implicitly
- Instead, if you want the case to fall through, that's what you need to explicitly specify
```Go
func getCreator(os string) string {
    var creator string
    switch os {
    case "linux":
        creator = "Linus Torvalds"
    case "windows":
        creator = "Bill Gates"

    // all three of these cases will set creator to "A Steve"
    case "macOS":
        fallthrough
    case "Mac OS X":
        fallthrough
    case "mac":
        creator = "A Steve"

    default:
        creator = "Unknown"
    }
    return creator
}
```
# Functions
```Go
func sub(x int, y int) int {
    return x-y
}
```
- When you have multiple parameters in a Go function, you can group ones with a similar type, and define their type once
```Go
func addToDatabase(hp, damage int, name string, level int) {
  // ?
}
```
