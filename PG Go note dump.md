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

## Ignoring Return Values
- A similar concept is also possible in javascript I believe
- If a function returns a value that you don't care about, you can actually ignore it by assigning it to a blank identifier `_`
```Go
func getPoint() (x int, y int) {
    return 3, 4
}

// ignore y value
x, _ := getPoint()
```
- In some languages that have this ability, it's merely a convention, but in Go, it's a full fledged language feature that completely discards the value
- This feature is useful because the Go compiler returns and error if you have unused variable declarations, so instead of assigning an unwanted value to a variable that you never use, you can just discard it
## Named Return Values
- Return values in Go can be given names, in which case they're treated as if they were new variables that were defined at the top of the function. Then you can do a naked return as return will automatically return the return values
- This practice is best for short functions, as it hurts readability. They can document the purpose of the returned values though
```Go
func getCoords() (x, y int) {
	// x and y are initialized with zero values

	return // automatically returns x and y
}

// This is the same as the above function
func getCoords() (int, int) {
	var x int
	var y int
	return x, y
}
```
- This is quite the unique behavior, the ability to declare the return variables in the signature, not just their types which auto initializes them and makes them return with an empty return
- The best practice is to both have named return values to clarify their purpose, and to still have them explicitly mentioned in the return instead of using a naked return as to not hurt readability
```Go
func calculator(a, b int) (int, int, error) {
    if b == 0 {
      return 0, 0, errors.New("can't divide by zero")
    }
    mul := a * b
    div := a / b
    return mul, div, nil
}
```
- nil is the zero value of an error
## Early Returns
- Go also supports early returns from inside conditionals for example
- This is nice as it means we can use Guard Clauses (an early return from a function when a condition is met) to make conditional blocks more one-dimensional and easier to read
```Go
// Nested conditionals without Guard Clauses
func getInsuranceAmount(status insuranceStatus) int {
  amount := 0
  if !status.hasInsurance(){
    amount = 1
  } else {
    if status.isTotaled(){
      amount = 10000
    } else {
      if status.isDented(){
        amount = 160
        if status.isBigDent(){
          amount = 270
        }
      } else {
        amount = 0
      }
    }
  }
  return amount
}

// Conditionals with Guard Clauses
func getInsuranceAmount(status insuranceStatus) int {
  if !status.hasInsurance(){
    return 1
  }
  if status.isTotaled(){
    return 10000
  }
  if !status.isDented(){
    return 0
  }
  if status.isBigDent(){
    return 270
  }
  return 160
}
```
## Functions As Values
- Go supports first-class and higher-order functions
- A language is said to have first-class functions when functions can be treated like any other variable, such as, being able to pass a function as an argument to another function, and be assigned as a value to a variable
```Go
// Say we have these two simple functions
func add(x, y int) int {
	return x + y
}

func mul(x, y int) int {
	return x * y
}

// And this functions that can use them
func aggregate(a, b, c int, arithmetic func(int, int) int) int {
  firstResult := arithmetic(a, b)
  secondResult := arithmetic(firstResult, c)
  return secondResult
}

// Now we can do this
func main() {
	sum := aggregate(2, 3, 4, add)
	// sum is 9
	product := aggregate(2, 3, 4, mul)
	// product is 24
}
```
- The above example is possible because aggregate's callback function here is not very specific, it's just a function that takes two ints as its arguments and returns an int, which applies to our two simple functions
## Anonymous Functions
- You can also define unnamed functions in Go
```Go
func conversions(converter func(int) int, x, y, z int) (int, int, int) {
	convertedX := converter(x)
	convertedY := converter(y)
	convertedZ := converter(z)
	return convertedX, convertedY, convertedZ
}

func double(a int) int {
    return a + a
}

func main() {
    // using a named function
	newX, newY, newZ := conversions(double, 1, 2, 3)
	// newX is 2, newY is 4, newZ is 6

    // using an anonymous function
	newX, newY, newZ = conversions(func(a int) int {
	    return a + a
	}, 1, 2, 3)
	// newX is 2, newY is 4, newZ is 6
```
## Defer
- This is a unique feature of Go
- The `defer` keyword allows us to mark a function, that will be executed automatically whenever and wherever the enclosing function it was called inside of returns
```Go
func GetUsername(dstName, srcName string) (username string, err error) {
	// Open a connection to a database
	conn, _ := db.Open(srcName)

	// Close the connection *anywhere* the GetUsername function returns
	defer conn.Close()

	username, err = db.FetchUser()
	if err != nil {
		// The defer statement is auto-executed if we return here
		return "", err
	}

	// The defer statement is auto-executed if we return here
	return username, nil
}
```
## Block Scope
- Like javascript and C, Go is block-scoped
- A variable that's declared inside a block is accessible only in that block and its nested blocks
```Go
package main

// scoped to the entire "main" package (basically global)
var age = 19

func sendEmail() {
    // scoped to the "sendEmail" function
    name := "Jon Snow"

    for i := 0; i < 5; i++ {
        // scoped to the "for" body
        email := "snow@winterfell.net"
    }
}

// Explicit block
package main

import fmt

func main() {
    {
        age := 19
        // this is okay
        fmt.Println(age)
    }

    // this is not okay
    // the age variable is out of scope
    fmt.Println(age)
}
```
- Blocks are defined by curly braces `{}`. New blocks are created for:
	- Functions
	- Loops
	- If statements
	- Switch statements
	- Select statements
	- Explicit blocks
## Closures
- These are functions that reference variables from outside their body, and they can both access and assign to the referenced variables
- The closure function remembers the variables that it had access to even after it returns
```Go
func concatter() func(string) string {
	doc := ""
	return func(word string) string {
		doc += word + " "
		return doc
	}
}

func main() {
	harryPotterAggregator := concatter()
	harryPotterAggregator("Mr.")
	harryPotterAggregator("and")
	harryPotterAggregator("Mrs.")
	harryPotterAggregator("Dursley")
	harryPotterAggregator("of")
	harryPotterAggregator("number")
	harryPotterAggregator("four,")
	harryPotterAggregator("Privet")

	fmt.Println(harryPotterAggregator("Drive"))
	// Mr. and Mrs. Dursley of number four, Privet Drive
}
```
- When using closures, the compiler automatically escapes any variables that must outlive the stack frame, to the heap
- When the compiler sees that that the inner function is referencing a variable from the outer variable, that should normally be popped from the stack, it will allocate it to the heap and create a hidden pointer usable only by the inner function every time its called that points to the variable that's now on the heap
- So now, every time `harryPotterAggreagator` is called, the inner function will be created on the stack in a stack frame. The `doc` variable will be updated, and persist since it's on the heap, and the stack frame will be popped
- As per boots, the variable lives in a closure "environment" object on the heap
- The returned function value, is also apparently 2 pointers, the pointer to the code, and pointer to the environment in a struct
```Go
type closure struct {
    fn   *functionCode   // pointer to compiled code
    env  *environment    // pointer to captured variables (like doc)
}
```
## Currying
- This is a concept from functional programming that involves applying a function partially
- This allows a function with multiple arguments, to be transformed into a sequence of functions, each taking a single argument
```Go
func main() {
  squareFunc := selfMath(multiply)
  doubleFunc := selfMath(add)

  fmt.Println(squareFunc(5))
  // prints 25

  fmt.Println(doubleFunc(5))
  // prints 10
}

func multiply(x, y int) int {
  return x * y
}

func add(x, y int) int {
  return x + y
}

func selfMath(mathFunc func(int, int) int) func (int) int {
  return func(x int) int {
    return mathFunc(x, x)
  }
}
```
- In the above example, `selfMath` takes an input function as its argument, and returns a function
- The return function itself takes an argument, and returns the result of passing that argument to the input function that's gonna use it in a calculation
# Struct
- Good news to me, an average structs enjoyer, Go has them too!!!!
```Go
type car struct {
	brand      string
	model      string
	doors      int
	mileage    int
}
```
- Go also supports nested structs
```Go
type car struct {
  brand string
  model string
  doors int
  mileage int
  frontWheel wheel
  backWheel wheel
}

type wheel struct {
  radius int
  material string
}

// We access the fields in a struct with the dot `.` operator just like in C 
myCar := car{}
myCar.frontWheel.radius = 5
```
## Anonymous Structs
- These are nameless structs that are meant to be used once, never to be referenced again
```Go
myCar := struct {
  brand string
  model string
} {
  brand: "Toyota",
  model: "Camry",
}

// Anonymous structs can be nested in other structs too
type car struct {
  brand string
  model string
  doors int
  mileage int
  // wheel is a field containing an anonymous struct
  wheel struct {
    radius int
    material string
  }
}

var myCar = car{
  brand:   "Rezvani",
  model:   "Vengeance",
  doors:   4,
  mileage: 35000,
  wheel: struct {
    radius   int
    material string
  }{
    radius:   35,
    material: "alloy",
  },
}
```
## Embedded Structs
- It should probably have been made obvious once structs were brought up that Go is not an object oriented language
- Embedded structs allow for data-only inheritance though
```Go
type car struct {
  brand string
  model string
}

type truck struct {
  // "car" is embedded, so the definition of a
  // "truck" now also additionally contains all
  // of the fields of the car struct
  car
  bedSize int
}
```
## Embedded vs Nested
- An embedded struct's fields are accessible at the top level like normal fields
- Like nested structs you can assign the promoted fields with the embedded struct in a "composite literal", so you don't have to do struct.nested.field, you can just do struct.field
- A composite type is any complex type like a struct or array
- The composite literal is basically the syntax used to assign the values of a composite type
```Go
lanesTruck := truck{
  bedSize: 10,
  car: car{
    brand: "Toyota",
    model: "Tundra",
  },
}

fmt.Println(lanesTruck.brand) // Toyota
fmt.Println(lanesTruck.model) // Tundra
```
- The main difference in declaration is that for the struct to be embedded you must add it to the other struct directly. You can't name the field, if you do it becomes nested