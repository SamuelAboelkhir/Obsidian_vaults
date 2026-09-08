---
tags:
- Go
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

//string literal
message := `Hello, world!
			how are ya?`

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
- Go also has `%T` which returns the type of a variable
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
- You can do a similar thing with imports, where you can say `import _ "<package>"` to tell Go that you need this package for its side effects, not to directly use it
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
- `defer` is also LIFO, so in case of multiple defers, the last one is always executed first
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
## Function Literals
- Functions also have literal syntax
```Go
func pingPong(numPings int) {
	pings := make(chan struct{})
	pongs := make(chan struct{})
	go ponger(pings, pongs)
	go pinger(pings, numPings)
	func() {
		i := 0
		for range pongs {
			fmt.Println("got pong", i)
			i++
		}
		fmt.Println("pongs done")
	}()
}
```
- Note that similar to javascript, adding the `()` at the end of a function literal, calls it immediately
# Struct
- Good news to me, an average structs enjoyer, Go has them too!!!!
```Go
type car struct {
	brand string
	model string
	doors int
	mileage int
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
## Struct Methods
- Structs can have methods in Go, but the way that's done is a bit weird
```Go
type rect struct {
  width int
  height int
}

// area has a receiver of (r rect)
// rect is the struct
// r is the placeholder
func (r rect) area() int {
  return r.width * r.height
}

var r = rect{
  width: 5,
  height: 10,
}

fmt.Println(r.area())
// prints 50
```
- You basically create the struct first, then you create a function, and that function gets what's known as a receiver
- The receiver is kinda like another argument, and conventionally it's name is the first letter of the struct
- This adds a function to the struct, so as the example above shows, now if we assign `r` as a new rect, we can call `r.area()` as a method of the rect struct
- Boots elaborated on this further
```Go
// Regular function
func getBasicAuth(a authenticationInfo) string {
    return "Authorization: Basic " + a.username + ":" + a.password
}

// You'd call it like this:
auth := authenticationInfo{"user", "pass"}
result := getBasicAuth(auth)  // passing auth as an argument

// Method
func (a authenticationInfo) getBasicAuth() string {
    return "Authorization: Basic " + a.username + ":" + a.password
}

// You call it like this:
auth := authenticationInfo{"user", "pass"}
result := auth.getBasicAuth()  // calling it ON the auth struct
```
- So in a sense, you don't define the function inside the struct as you would with a class or object in OOP, since Go isn't even OOP it's procedural programming, but rather you assign functions to the struct
## Memory Layout
- Works exactly like C, nothing else to say here
## Empty Structs
- These structs are literally empty, meaning they take up zero bytes of memory
- They are used in Go as a unary value
```Go
// anonymous empty struct type
empty := struct{}{}

// named empty struct type
type emptyStruct struct{}
empty := emptyStruct{}
```
# Interfaces in Go
- An interface is a collection of functions under one type
- A struct that defines all the functions of an interface as methods with the same return types will satisfy the interface and become a member of it
- A struct can belong to multiple interfaces
```Go
type shape interface {
  area() float64
  perimeter() float64
}

type rect struct {
    width, height float64
}
func (r rect) area() float64 {
    return r.width * r.height
}
func (r rect) perimeter() float64 {
    return 2*r.width + 2*r.height
}

type circle struct {
    radius float64
}
func (c circle) area() float64 {
    return math.Pi * c.radius * c.radius
}
func (c circle) perimeter() float64 {
    return 2 * math.Pi * c.radius
}

func printShapeData(s shape) {
	fmt.Printf("Area: %v - Perimeter: %v\n", s.area(), s.perimeter())
}
```
- Like structs, we can have empty interfaces
```Go
interface{}
```
- An empty interface has no requirements, therefore all structs will implicitly satisfy it
- It's also preferable to name the interface's parameters for clarity
```Go
// Bad
type Copier interface {
  Copy(string, string) int
}

// Good
type Copier interface {
  Copy(sourceFile string, destinationFile string) (bytesCopied int)
}
```
## Type Assertion
- In cases where you need to access one of the types that implement an interface, you can do so with type assertion
- It's kinda similar to C's casting, but while C's compiler trusts you to do whatever you want, Go will actually check the type at runtime, and will PANIC if the type doesn't match the assertion
```Go
type shape interface {
	area() float64
}

type circle struct {
	radius float64
}

func (c circle) area() float64 {
	// ...
}

func printShapeInfo(s shape) {
	c, ok := s.(circle)
	if ok {
		radius := c.radius
		fmt.Println("s is a circle, radius: %v", radius)
		return
	}
}
```
- We can also use a switch to do several type assertions
```GO
func printNumericValue(num interface{}) {
	switch v := num.(type) {
	case int:
		fmt.Printf("%T\n", v)
	case string:
		fmt.Printf("%T\n", v)
	default:
		fmt.Printf("%T\n", v)
	}
}

func main() {
	printNumericValue(1)
	// prints "int"

	printNumericValue("1")
	// prints "string"

	printNumericValue(struct{}{})
	// prints "struct {}"
}
```
## Clean Interfaces
- Interfaces have some best practices and rules of thumb to keep them actually good and usable
1. **Keep Interfaces Small**
	- Interfaces are meant to define the minimal behavior necessary to accurately represent an idea or concept
	- Here's an example from the standard HTTP package
	```Go
	type File interface {
	    io.Closer
	    io.Reader
	    io.Seeker
	    Readdir(count int) ([]os.FileInfo, error)
	    Stat() (os.FileInfo, error)
	}
	```
	- Any type that satisfies this interface will be considered a `File`, regardless of it's underlying `Concrete Type`
2. **Interfaces Should Have No Knowledge of Satisfying Types**
	- An interface doesn't need to have any idea or direct relation with the types that satisfy it
	```Go
	type car interface {
		Color() string
		Speed() int
		IsFiretruck() bool
	}
	```
	- In this example, we're checking for every car if it's a firetruck, which isn't very practical as now we have to check if a car is, well, any other type of vehicle too `IsTank()`, `IsSedan()` and so on
	- Instead, type assertion should have been used to figure out what type of car we're dealing with
	- In some cases, we can have a sub-interface that in a sense, extends the main interface
	```Go
	type firetruck interface {
		car
		HoseLength() int
	}
	```
3. **Interfaces Are Not Classes**
	- Interfaces are not classes, they are slimmer.
	- Interfaces don't have constructors or deconstructors that require that data is created or destroyed.
	- Interfaces aren't hierarchical by nature, though there is syntactic sugar to create interfaces that happen to be supersets of other interfaces.
	- Interfaces define function signatures, not their underlying behavior (function definition/body). They don't make code "DRY" as each struct that satisfies the interface, will need to have its own copies of the functions in the interface
# Note on Types
- This isn't covered as a lesson on boot.dev, and was just used in `CH10 L11` but I wanted to cover it because it's interesting
- You can declare a new type called a **Named Type**, of any underlying type, and then assign `const` values to it
```Go
type transactionType string

const (
    transactionDeposit    transactionType = "deposit"
    transactionWithdrawal transactionType = "withdrawal"
)
```
- This just make this type a bit more unique, and gives autocomplete on the allowed values
- Also you obviously don't have to pass `const` values in particular, but it's good practice
- We can declare a type out of basically anything, and use it to wrap other values
```Go
type HandlerFunc func(ResponseWriter, *Request)
// This type is defined in the http/net package
http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
    // ...
})
```
# Errors
- Errors in Go are values, and they have their own interface
```Go
type error interface {
    Error() string
}
```
- When something can go wrong in a function, it should return the zero value of its original return type, and an error. Otherwise, it should return the correct value, and `nil`
- Taking `Atoi` as an example, how do we use it safely?
```Go
func Atoi(s string) (int, error)

// Atoi converts a stringified number to an integer
i, err := strconv.Atoi("42b")
if err != nil {
    fmt.Println("couldn't convert:", err)
    // because "42b" isn't a valid integer, we print:
    // couldn't convert: strconv.Atoi: parsing "42b": invalid syntax
    // Note:
    // 'parsing "42b": invalid syntax' is returned by the .Error() method
    return
}
// if we get here, then the
// variable i was converted successfully
```
- Since errors are interfaces, we can make custom types that implement that interface
```Go
type userError struct {
    name string
}

func (e userError) Error() string {
    return fmt.Sprintf("%v has a problem with their account", e.name)
}

// Then use it as an error
func sendSMS(msg, userName string) error {
    if !canSendToUser(userName) {
        return userError{name: userName}
    }
    ...
}
```
- This approach tends to over complicate things though, so we have another way of doing this, the errors package
```Go
package main

import (
	"errors"
)

func divide(x, y float64) (float64, error) {
	if y == 0 {
		return 0, errors.New("no dividing by 0")
	}
	return x / y, nil
}
```
- One more thing you can do, is print the error directly with `fmt.Errorf()`
```Go
package main

import (
	"fmt"
	"net/http"
)

func fetchData(url string) (int, error) {
	res, err := http.Get(url)
	if err != nil {
		return 0, fmt.Errorf("network error: %v", err)
	}
	defer res.Body.Close()
	if res.StatusCode != http.StatusOK {
	    return res.StatusCode, fmt.Errorf("non-OK HTTP status: %s", res.Status)
	}
	return res.StatusCode, nil
}
```
## Panic!!!!
- Generally speaking, don't ever use `Panic`
- Panic is another way of handling errors
- It basically causes the running function to return, and it keeps returning up the stack until it crashes the whole app, or reaches a `recover`
- Recover is a function that we can defer earlier and it will basically return control to the app and continue the execution in case of a panic
```Go
func enrichUser(userID string) User {
    user, err := getUser(userID)
    if err != nil {
        panic(err)
    }
    return user
}

func main() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("recovered from panic:", r)
        }
    }()

    // this panics, but the defer/recover block catches it
    // a truly astonishingly bad way to handle errors
    enrichUser("123")
}
```
- A better alternative to panic is `log.Fatal`
# Loops
- Go's loops use the standard C syntax
```Go
for INITIAL; CONDITION; AFTER{
  // do something
}
```
- The new thing here is that we can omit sections of the loop 
```Go
for INITIAL; ; AFTER {
  // do something forever
}
```
- This leads to another unique thing about Go, we don't have while loops, nor do we need them when we can omit everything from a for loop except the condition, which makes it a while loop in practice
```Go
for CONDITION {
  // do some stuff while CONDITION is true
}
```
# Slices in Go
## Arrays
- Like C, these are fixed size groups of variables of the same type
- Arrays can be declared without initialization, or with initialization
```Go
// Without
var myInts [10]int
// Or
myInts := [10]int{}

// With
primes := [6]int{2, 3, 5, 7, 11, 13}
```
## Slices
- Slices are in a way, the dynamic array struct we used to make in C, and they're ordered
- You use them 99 times out of a 100 in Go because they're dynamically-sized
- The zero value of a slice is `nill`
- Non-nil slices as the name implies are parts of something, and as such, they actually always have an underlying array, but that's not always explicitly specified. However, we can make it explicit
```Go
primes := [6]int{2, 3, 5, 7, 11, 13}
mySlice := primes[1:4]
// mySlice = {3, 5, 7}

// Or
mylice := []int{}
```
- Go slices have the same syntax as python slices. Low index is inclusive, and high index is exclusive just like python too
```Go
// From low to high
arrayname[lowIndex:highIndex]
// From low to end
arrayname[lowIndex:]
// From start to high
arrayname[:highIndex]
// Entire array
arrayname[:]
```
- Behind the scenes, when we expand a slice, as I said before, it's the exact same as what used to happen in C with dynamic array structs. When the slice expands beyond the original array's confines, a new array will be created somewhere else in memory, the data will be copied over, and we will have a bigger array, just like with `realloc`
## Make
- `make()` is a memory allocation and initialization function, used primarily with slices, maps and channels
- When creating a slice, we don't need to think about the underlying array
- We can create a slice with the `make()` function which will fill the slice with zero values of the declared type up to the declared length. This is good for pre-declaring the size of an array if we know it beforehand, reducing a bit of unnecessary computations that can affect performance. We can even pre-declare the capacity of the underlying array, but if we don't, it will default to being equal to the slice's length
```Go
// func make([]T, len, cap) []T
mySlice := make([]int, 5, 10)

// the capacity argument is usually omitted and defaults to the length
mySlice := make([]int, 5)
```
- We can also use a slice literal to pre-fill the slice with some values
```Go
// The empty square brackets make this a slice, if they had a value, it would have been an array
mySlice := []string{"I", "love", "go"}
```
- A slice has a length and a capacity
- The length is how many elements are in the slice right now. It can be viewed with `len()`
- The capacity is the number of elements in the underlying array counting from the first element in the slice, and it can be accessed with `cap()`
## Variadic
- Many functions can take an arbitrary number of final arguments, which is possible by passing `...` allowing variadic functions to receive variadic arguments as a slice
```Go
func concat(strs ...string) string {
    final := ""
    // strs is just a slice of strings
    for i := 0; i < len(strs); i++ {
        final += strs[i]
    }
    return final
}

func main() {
    final := concat("Hello ", "there ", "friend!")
    fmt.Println(final)
    // Output: Hello there friend!
}
```
- You can also pass actual slices to variadic functions, using the spread operator. Yes, the same one from javascript
```Go
func printStrings(strings ...string) {
	for i := 0; i < len(strings); i++ {
		fmt.Println(strings[i])
	}
}

func main() {
    names := []string{"bob", "sue", "alice"}
    printStrings(names...)
}
```
- Also, see how an empty interface can be useful in this following example
```Go
func Println(a ...interface{}) (n int, err error)
```
- `Println()` and other print functions are variadic, and can take any number of arguments, but we were talking about needing to pass variadic arguments of the same type. Well, using an empty interface that doesn't specify a type, we can pass arguments of any type, and they will be acceptable by the function
## Append
- Append adds elements to slices dynamically, and if the underlying array is full, it will create a new one and point the slice to it
- Append is variadic
## Range
- The `range` keyword in Go is syntactic sugger that facilitates iterating over elements of a slice, where the `ELEMENT` is a copy of the value at `INDEX` of the slice
```Go
for INDEX, ELEMENT := range SLICE {
}

// Example
fruits := []string{"apple", "banana", "grape"}
for i, fruit := range fruits {
    fmt.Println(i, fruit)
}
// 0 apple
// 1 banana
// 2 grape
```
## Slice of Slices
- Not really breaking news considering every language can do this, but a slice can hold other slices to create a matrix or 2D slice
```Go
rows := [][]int{}
rows = append(rows, []int{1, 2, 3})
rows = append(rows, []int{4, 5, 6})
fmt.Println(rows)
// [[1 2 3] [4 5 6]]
```
- Note that append changes the underlying array of the input slice and returns a new slice, which usually means that you shouldn't use append on any slice, other than itself
```Go
// don't do this!
someSlice = append(otherSlice, element)
```
- To explain why this is a bad practice, consider the 2 following examples
#### Example A
```Go
// Example A works a expected
a := make([]int, 3)
fmt.Println("len of a:", len(a))
fmt.Println("cap of a:", cap(a))
// len of a: 3
// cap of a: 3

b := append(a, 4)
fmt.Println("appending 4 to b from a")
fmt.Println("b:", b)
fmt.Println("addr of b:", &b[0])
// appending 4 to b from a
// b: [0 0 0 4]
// addr of b: 0x44a0c0

c := append(a, 5)
fmt.Println("appending 5 to c from a")
fmt.Println("addr of c:", &c[0])
fmt.Println("a:", a)
fmt.Println("b:", b)
fmt.Println("c:", c)
// appending 5 to c from a
// addr of c: 0x44a180
// a: [0 0 0]
// b: [0 0 0 4]
// c: [0 0 0 5]
```
#### Example B
```Go
// Example B has a problem
i := make([]int, 3, 8)
fmt.Println("len of i:", len(i))
fmt.Println("cap of i:", cap(i))
// len of i: 3
// cap of i: 8

j := append(i, 4)
fmt.Println("appending 4 to j from i")
fmt.Println("j:", j)
fmt.Println("addr of j:", &j[0])
// appending 4 to j from i
// j: [0 0 0 4]
// addr of j: 0x454000

g := append(i, 5)
fmt.Println("appending 5 to g from i")
fmt.Println("addr of g:", &g[0])
fmt.Println("i:", i)
fmt.Println("j:", j)
fmt.Println("g:", g)
// appending 5 to g from i
// addr of g: 0x454000
// i: [0 0 0]
// j: [0 0 0 5]
// g: [0 0 0 5]
```
- The reason example B had a bug is due to what we said about [[#Append]] before
- Append only creates a new slice, if the underlying array has no more space left, but when we used `make()` in example B and declared a higher capacity than the slice's length, we made sure there is room for more elements, and the slice was able to grow within the confines of the underlying array, so both `j` and `g` ended up pointing at the same array, instead of becoming new slices for new arrays, which caused the overwritte on index 4
- This is why, we always use append, on the same slice. We can also copy the values from the other slice first if we want to introduce change it to it in a different array
```Go
mySlice := []int{1, 2, 3}
mySlice = append(mySlice, 4)

// Copy the original slice's contents
slice1 := []int{1,2,3,4}
slice2 := []int{}
slice2 = append(slice2, slice1...)
```
## Iterating over strings
- A string is a bytes array, but a single char in Go, unlike C can be larger than 1 byte, and would actually probably return its UTF/ASCII value
- If you want to instead get actual chars when indexing a string, you must use the `rune` type
- This is default behavior when using `range`, but we can also do the following
```Go
s := "Hello"
runes := []rune(s)

ch := runes[1] // this is a rune, 'e'
```
# Maps
- Oy, Go has objects, kinda, it has a data structure with key->value mapping, aptly called a map
- Also, make in't exclusive to slices, it can be used to make maps
```Go
ages := make(map[string]int)
ages["John"] = 37
ages["Mary"] = 24
ages["Mary"] = 21 // overwrites 24

// You can also use a literal
ages := map[string]int{
  "John": 37,
  "Mary": 21,
}
```
- You can also use `len()` on a map to get the total number of key->value pairs
## Mutations
- These are ways of manipulating the data
```Go
// Insert element
m[key] = elem

// Get element
elem = m[key]

// Delete element
delete(m, key)

// Check if a key exists
elem, ok := m[key]

// If `key` is in `m`, then `ok` is `true` and `elem` is the value as expected.
// If `key` is not in the map, then `ok` is `false` and `elem` is the zero value for the map's element type.
```
## Key types
- Values are quite lax, and allow you to use any type that you want, keys, not so much
- Keys must be of a type that's "comparable", meaning it can be used in a comparison with == such as boolean, numeric, string, pointer, channel, and interfaces. Also structs and arrays that contain these types
- Being able to use a struct as a key is interesting, but very useful
- For example, this map of maps could be used to tally web page hits by country
```Go
hits := make(map[string]map[string]int)
```
- This is a map of string to (map of string to int). Each key of the outer map is the path to a web page with its own inner map. Each inner map key is a two-letter country code. This expression retrieves the number of times an Australian has loaded the documentation page
```Go
n := hits["/doc/"]["au"]
```
- But this approach can become unwieldy when adding data since for any outer key you will need to check if the inner map exists firsts and create it if it doesn't
```Go
func add(m map[string]map[string]int, path, country string) {
    mm, ok := m[path]
    if !ok {
        mm = make(map[string]int)
        m[path] = mm
    }
    mm[country]++
}
add(hits, "/doc/", "au")
```
- If we were to use a struct key though
```Go
type Key struct {
    Path, Country string
}
hits := make(map[Key]int)

// Then checking if a vietnamese person checked the home page
hits[Key{"/", "vn"}]++

// Checking how many Swiss have read the spec
n := hits[Key{"/ref/spec", "ch"}]
```
- Here, the use of a struct allowed us to key data by multiple dimensions
## Count Instances
- What we did with `range` was not unique to ranges, but rather it's a feature in Go that allows us to assign variables within the `if block`. Technically, we already knew this though because that's the `INITIALIZATION` step
```Go
names := map[string]int{}
missingNames := []string{}

if _, ok := names["Denna"]; !ok {
    // if the key doesn't exist yet,
    // append the name to the missingNames slice
    missingNames = append(missingNames, "Denna")
}
```
# Pointers
- Same good ol' pointers from C, lets "C" what's new here, hehehe....
## References
- You can define a pointer without initializing it, making it's value (the address it holds) `nil`. This pointer is called a nil pointer
- That pointer isn't super useful though, so we can instead assign it a value normally
```Go
// nil pointer
var p *int

fmt.Printf("value of p: %v\n", p)
// value of p: <nil>

// Assigned pointer
myString := "hello"      // myString is just a string
myStringPtr := &myString // myStringPtr is a pointer to myString's address

fmt.Printf("value of myStringPtr: %v\n", myStringPtr)
// value of myStringPtr: 0x140c050
```
- Just like C, we can dereference a pointer to get the value of the variable who's address the pointer has as a value
```Go
*myStringPtr = "world"                              // set myString through the pointer
fmt.Printf("value of myString: %s\n", *myStringPtr) // read myString through the pointer
// value of myString: world
```
- Unlike C though, Go has no pointer arithmetic
## Pass by Reference
- Most variables in Go are passed to functions by value, except for some composite variables like slices and maps
- This, like in C, is one of the main uses of pointers, which is to pass by reference
```Go
// Pass by Value
func increment(x int) {
    x++
    fmt.Println(x)
    // 6
}


func main() {
    x := 5
    increment(x)
    fmt.Println(x)
    // 5
}

// Pass by Reference
func increment(x *int) {
    *x++
    fmt.Println(*x)
    // 6
}

func main() {
    x := 5
    increment(&x)
    fmt.Println(x)
    // 6
}
```
- Pointers to structs here have a different syntax than in C
```Go
// This doesn't work
msgTotal := *analytics.MessagesTotal

// This is how to access a pointer to a struct's fields
msgTotal := analytics.MessagesTotal

// But this would also work
msgTotal := (*analytics).MessagesTotal
```
- The above approach is the recommended simplest way to access struct fields in Go
- It's a shorthand for
```Go
(*analytics).MessagesTotal

// The above i equivalent to
analytics.MessagesTotal
```
- This is because of operator precedence in Go, where the dot `.` is interpreted before the dereference `*`, which I believe is also the same as in C
## Nil Pointers
- Remember when I said before a nil pointer is probably not very useful? Well, it's also dangerous
- If you try to dereference a nil pointer, it will cause a runtime error, and `panic` (I wonder if we can defer a recover just in case)
- So, just like in C, always check if a pointer is nil before actually dereferencing it
## Pointer Receivers
- In Go, a pointer receiver is more popular that value receivers for struct methods since you usually want to alter the value of the receiver
- Apparently, a method with pointer receivers doesn't need a pointer to be used when calling the method. The pointer will automatically be derived from the value
```Go
// Pointer receiver
type car struct {
	color string
}

func (c *car) setColor(color string) {
	c.color = color
}

func main() {
	c := car{
		color: "white",
	}
	c.setColor("blue")
	fmt.Println(c.color)
	// prints "blue"
}

// Value receiver
type car struct {
	color string
}

func (c car) setColor(color string) {
	c.color = color
}

func main() {
	c := car{
		color: "white",
	}
	c.setColor("blue")
	fmt.Println(c.color)
	// prints "white"
}

// Another example
type circle struct {
	x int
	y int
    radius int
}

func (c *circle) grow() {
    c.radius *= 2
}

func main() {
    c := circle{
        x: 1,
        y: 2,
        radius: 4,
    }

    // notice c is not a pointer in the calling function
    // but the method still gains access to a pointer to c
    c.grow()
    fmt.Println(c.radius)
    // prints 8
}
```
## Pointer Performance
- Lane's rule of thumb are:
	1. First, worry about writing clear, correct, maintainable code.
	2. If you have a performance problem, fix it.
- These rules really apply generally to code, we even had similar rules of thumb regarding [[PG SQL#Normalization]]
- This means, we should focus on using pointers when we need a shared reference to a value, rather than deciding to always use pointers because they don't create copies which should be faster
- When we actually have performance issues, we should consider:
	1. Stack vs Heap
	2. Copying
- Also, note that local non-pointer variables are actually faster to pass around than pointers, because they're stored on the stack which is faster to access than the heap, despite the fact copying is involved
- If the value being copied is so large though that it actually starts becoming such a big problem, then using a pointer to avoid copying might be the move, but since we will now have the value on the heap, the speed gain from not copying has to be greater than the speed loss from moving to the heap
- It's really a balancing act, and your understanding of the underlying systems at play, is how you optimize it
- The reason Go uses less memory than Java and C# is that Go tends to allocate more on the stack
# Packages and Modules
- Every Go program is made up of packages
- The "main" one, is the main file of the program, with `package main` at the top
- This file has its entry point at the `main()` function, and it's compiled into an executable program
- A package by any other name is called a library package, and wont have an entry point
- Libraries simply export functionality that can be used by other packages, like the following code, which is a main package, and imports code from the `fmt` and `math/rand` library packages
```Go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```
## Package Naming
- Conventionally, a package name is the same as the last element of its import path
- For example, the `math/rand` package comprises files that begin with
```Go
package rand
```
- Package names aren't required to match their import path. For example, a package path can be `github.com/textio/rand`, and be called `random`, however, that's discouraged for the sake of  consistency
## One Package / Directory
- A directory of Go code can have at most one package
- All `.go` files in a single directory must belong to the same package, otherwise an error will be thrown by the compiler
- This goes for both main and library packages
## Modules
- Go programs are organized into packages
- A package is a directory of Go code that's all compiled together
- Functions, types, variables, and constants defined in one source file are visible to all other source file within the same package (directory)
- A repository contains one or more modules, where a module is a collection of Go packages that relates together
- Module group the code and tracks the dependencies it uses
- It's created with the `go mod init <MODULE_PATH>` command
	- The module path can be a remote location such as a github repo, or a local one
![[go_module.png]]
## One Module Per Repo (Usually)
- A module is declared by a file named `go.mod` at the root of the project
- That file contains:
	- The module path
	- The version of Go the project requires
	- Optionally, any external package dependencies the project has
- The module path is the import path prefix for all the packages within the module
- Example `go.mod`
```
module github.com/bootdotdev/exampleproject

go 1.25.1

require github.com/google/examplepackage v1.3.0
```
- The module's path doesn't only serve as an import path prefix for the packages within, but also indicates where the go command should look to download it
- For example, to download `golang.org/x/tools`, the go command would search the repository at [https://golang.org/x/tools](https://golang.org/x/tools)
- An import path is the module's path + a package's subdirectory within the module
- A module can be defined locally, and not necessarily on a remote repository
## Go Environment
#### Directory Structure
- To recap how packages and modules work in your project directory structure:
	- You will have many git repositories on your machine (typically one per project).
	- Each repository is typically a single module.
	- Each module contains one or more packages
	- Each package consists of one or more Go source files in a single directory.
#### GOPATH
- `$GOPATH` is an environment variable that gets set by default somewhere on the machine, typically in the home directory `~/go`
- One should avoid working directly inside `$GOPATH/src`, as this is the old way of working with Go, and is now outdated, and can causes issues
## Go Run
- `go run` is a way to compile and run a Go package without saving the compiled binary
- Mainly used for local testing and debugging
## Go Build
- Go run, but permanent. `go build`
- It compiles the code into a single statically linked executable program
## Go Install
- The `go install` command compiles and installs a package, or packages on your local machine for your personal use
- It installs the package's compiled binary in the `GOBIN` directory
- Running `go install` inside your Go project will compile, and install it in `GOBIN` making it globally available on your machine
## Custom Package
- If we create a non-main package, maybe in a new module (yes a module can exist without a main package), and run `go build`, the package will be compiled and cached for future use
- The cache location can be found with `go env GOCACHE`, but generally you don't need to touch it
- Remember that a variable with a capital name is public, otherwise it's private
- To import packages from one local module into another you need to update the `go.mod` file as such
```Go
module github.com/SamuelAboelkhir/hellogo

go 1.25.1
// Order doesn't matter
replace github.com/SamuelAboelkhir/mystrings v0.0.0 => ../mystrings

require github.com/SamuelAboelkhir/mystrings v0.0.0
```
- In the course we had the hellogo module and mystrings module as sibling directories
- Then inside `hellogo/main.go`
```Go
package main

import (
	"fmt"
	"github.com/SamuelAboelkhir/mystrings"
)

func main() {
	fmt.Println(mystrings.Reverse("hello world"))
}
```
- The `replace` command in the `go.mod` file told Go to look for the imported package in `../mystrings` instead of the remote repository
## Remote Packages
- The use of `replace` is not really advised, as the proper way to create and use a dependency is to publish it to a remote repository, although this only matters in collaborative settings. Using `replace` should be fine for local-only development
- This time, we added a new directory `datetest`
```Go
package main

import (
	"fmt"
	"time"

	tinytime "github.com/wagslane/go-tinytime"
)

func main() {
	tt := tinytime.New(1585750374)
	tt = tt.Add(time.Hour * 48)
	fmt.Println("1585750374 converted to a tinytime is:", tt)
}

```
- We then ran `go get github.com/wagslane/go-tinytime` to get the required code
- `go get` did a few things though
	- It downloaded the module's code and added it to the cache
	- It updated go.mod, adding a require line for the module with its specific version
	- It updated a file called `go.sum` which Go uses to record a checksum for the downloaded modules so that it can verify their integrity in the future
## Clean Packages
#### Rules of Thumb
1. Hide Internal Logic
	- Similar to encapsulation from OOP
	- Oftentimes, applications will have complex logic that requires a lot of code, which can be exposed via an API
	- This means most of the dirty work can be kept within a package away from other applications that need the API
	- For example
	```Go
	package classifier

	// ClassifyImage classifies images as "hotdog" or "not hotdog"
	func ClassifyImage(image []byte) (imageType string) {
		if hasHotdogColors(image) && hasHotdogShape(image) {
			return "hotdog"
		} else {
			return "not hotdog"
		}
	}
	
	func hasHotdogShape(image []byte) bool {
		// internal logic that the application doesn't need to know about
		return true
	}
	
	func hasHotdogColors(image []byte) bool {
		// internal logic that the application doesn't need to know about
		return true
	}
	```
	- In the above code, only `ClassifyImage()` is public and exposed to the application-level, while the rest is kept private
	- This is because, the rest of the application, or other applications, don't need to know how images are being classified, just the result of the classification
2. Don't Change APIs
	- The private functions in the package can be changed for testing, refactoring, and fixing bugs
	- The exported function's signature though, shouldn't be changed to keep the API stable, otherwise, users will be constantly getting breaking changes with every change to the signature
3. Don't Export Functions from the Main Package
	- A `main` package isn't a library, you don't need to export functions from it
4. Packages Shouldn't Know About Dependents
	- A package, shouldn't have specific knowledge about a particular application that uses it
	- This just keeps the package generic and usable by all other applications that need it, instead of it being tailored towards any specific dependent
# Channels
## Concurrency
- To increase the speed of a program, you can either decrease the number of executions that it needs per second, or, get a better CPU that can execute more instructions per second
- Concurrency is the ability to perform multiple tasks at the same time
- Code is usually executed one line at a time. This is called, sequential execution, or synchronous execution
- If we have multiple CPU cores, we can execute multiple tasks at **exactly** the same time
- A single core can execute code at **almost** the same time by switching between tasks very quickly
- Go is concurrent by design, which is apparently a unique feature
- Go excels at performing many tasks simultaneously, and safely, using a simple syntax
- To use concurrency in Go, you simply use the `go` keyword when calling a function
```Go
go function()
```
- Here, the `go` keyword spawns a new `goroutine`, which is a lightweight thread of execution that is a part of the Go runtime alongside Go' garbage collector
## Channels
- Channels are a typed, [thread-safe](https://en.wikipedia.org/wiki/Thread_safety) (meaning it can be invoked and accessed concurrently by multiple threads without causing unexpected behavior) queue
- Channels allow different `goroutines` to communicate with each other
```GO
// Create a channel
ch := make(chan int)
```
- The `<-` is called the channel operator. Data flows in the direction of the arrow. This operation will [block](https://en.wikipedia.org/wiki/Blocking_(computing)) until another `goroutine` is ready to receive the value
```Go
// Send data to a channel
ch <- 69
```
- When data is received from a channel, the read value is removed from the channel, and is saved in the receiving variable. Again, this operation will block until there is a value in the channel to be read
```Go
// Receive data from a channel
v := <-ch
```
- Channels are reference types like maps and slices, so they are passed by reference by default
```GO
func send(ch chan int) {
    ch <- 99
}

func main() {
    ch := make(chan int)
    go send(ch)
    fmt.Println(<-ch) // 99
}
```
#### Blocking and Deadlocks
- A [deadlock](https://yourbasic.org/golang/detect-deadlock/#:~:text=yourbasic.org%2Fgolang,look%20at%20this%20simple%20example.) occurs when a group of `goroutines` are all blocking and none of them can continue
## Signals
- Sometimes, we don't care what's being passed through a channel, but when and if something is passed
- In that case, we can block and wait until something is sent on a channel
```Go
<-ch
```
- This code blocks until it pops a single item off the channel, and then continues to discard items
- Empty structs are usually used as unary values in these cases so that the sender communicates that this is only a signal and not actual data
```GO
func downloadData() chan struct{} {
	downloadDoneCh := make(chan struct{})

	go func() {
		fmt.Println("Downloading data file...")
		time.Sleep(2 * time.Second) // simulate download time

		// after the download is done, send a "signal" to the channel
		downloadDoneCh <- struct{}{}
	}()

	return downloadDoneCh
}

func processData(downloadDoneCh chan struct{}) {
	// any code here can run normally
	fmt.Println("Preparing to process data...")

	// block until `downloadData` sends the signal that it's done
	<-downloadDoneCh

	// any code here can assume that data download is complete
	fmt.Println("Data download complete, starting data processing...")
}

processData(downloadData())
// Preparing to process data...
// Downloading data file...
// Data download complete, starting data processing...
```
# Buffered Channels
- Note that the sending and receiving are expected to happen at the same time, which is the whole point of the `goroutine`s and the channels that establish communication between them
- If you try to send and receive on the same `goroutine` you will cause a deadlock, because this is sequential, not concurrent
- If the channel is buffered, and works as a queue, then it can hold data, and would actually work even on the same `goroutine` because a buffered channel only blocks when its queue is full for sending, or when its empty for receiving. An unbuffered channel holds no data, so send and receive must be simultaneous
- Similarly, if you send n times on one `goroutine` you must receive n times on the other `goroutine`, otherwise you will have a deadlock
- To buffer a channel
```Go
ch := make(chan int, 100)
```
## Closing Channels
- The sender can also explicitly close a channel
```Go
ch := make(chan int)

// do some stuff with the channel

close(ch)
```
- We can even use the same `ok` value used in maps to check if a channel is closed
```Go
v, ok := <-ch
```
- Attempting to send on a closed channel will cause a panic, which, when on the main `goroutine` will cause the program to crash
- A panic on any other `goroutine`, will cause only it to crash
- Closing a channel isn't necessary as they'll still be garbage collected anyway when unused, but closing a channel can indicate to a receiver that there's nothing else to be received
## Range
- We can range over channels just like with slices and maps
```Go
for item := range ch {
    // item is the next value received from the channel
}
```
## Select
- `select` is used when we have one `goroutine` listening to multiple channels, and we want to process data in the order it come through each channel
- Syntactically, `select` is similar to `switch`
```Go
select {
case i, ok := <-chInts:
	if ok {
		fmt.Println(i)
	}
case s, ok := <-chStrings:
	if ok {
		fmt.Println(s)
	}
}
```
- The first channel that has a value will execute first, and if multiple have values ready at the same time, one will be chose at random
- Adding a default case to `select` makes prevents it from blocking as it executes immediately if no other channel has a value ready
```Go
select {
case v := <-ch:
    // use v
default:
    // receiving from ch would block
    // so do something else
}
```
- In case you want to ignore the channel's value, you have two options
```Go
select {
case <-ch:
    // event received; value ignored
default:
    // so do something else
}

// Or
select {
case _ = <-ch:
    // event received; value ignored
default:
    // so do something else
}
```
## Tickers
- The `time` package in Go's standard library has multiple functions that return channels
- `time.Tick()` is a standard library function that returns a channel that sends a value on a given interval.
- `time.After()` sends a value once after the duration has passed.
- `time.Sleep()` blocks the current `goroutine` for the specified duration of time.
- These functions take a `time.Duration` ass an argument
```Go
time.Tick(500 * time.Millisecond)
```
- The functions here default to nanoseconds if you don't specify the units
## Read-Only Channels
- Channels can be marked a read-only if you cast it from `chan` to `<-chan` and the type
```Go
func main() {
    ch := make(chan int)
    readCh(ch)
}

func readCh(ch <-chan int) {
    // ch can only be read from
    // in this function
}
```
- Inversely, channels can be write-only, by moving the arrow's position
```Go
func writeCh(ch chan<- int) {
    // ch can only be written to
    // in this function
}
```
## A Few Closing Notes About Channels
- A declared but uninitialized channel is nil just like a slice
```Go
var s []int       // s is nil
var c chan string // c is nil

var s = make([]int, 5) // s is initialized and not nil
var c = make(chan int) // c is initialized and not nil
```
- A send to a nil channel blocks forever
```Go
var c chan string        // c is nil
c <- "let's get started" // blocks
```
- A receive from a nil channel blocks forever
```Go
var c chan string // c is nil
fmt.Println(<-c)  // blocks
```
- A send to a closed channel panics
```GO
var c = make(chan int, 100)
close(c)
c <- 1 // panic: send on closed channel
```
- A receive from a closed channel returns the zero value immediately
```Go
var c = make(chan int, 100)
close(c)
fmt.Println(<-c) // 0
```
- If a program exists before its `goroutine`s are completed, they will be killed silently
# Mutexes
- Mutexes allow us to lock access to data
- They ensure that we can control which `goroutine`s can access which data at which time
- The `sync.Mutex` type from the standard library provide us with two methods to achieve this, `.Lock()` and `.Unlock()`
```Go
func protected(){
    mu.Lock()
    defer mu.Unlock()
    // the rest of the function is protected
    // any other calls to `mu.Lock()` will block
}
```
- It's good practice to remember to defer an unblock for locked blocks
#### Maps Are Not Thread-Safe
- A map accessed by multiple `goroutine`s is a good example of something that you want to lock, as maps are not thread-safe
- Mutex is short for mutual exclusion because it excludes different threads (or `goroutine`s) from accessing the same data at the same time
- Mutex is used when multiple threads are accessing the same data, where maybe one is reading the shared data as the other thread is writing to it
- This could cause a panic if the reader is reading bad data that's being mutated in place
![[mutex.png]]
```Go
package main

import (
	"fmt"
)

func main() {
	m := map[int]int{}
	go writeLoop(m)
	go readLoop(m)

	// stop program from exiting, must be killed
	block := make(chan struct{})
	<-block
}

func writeLoop(m map[int]int) {
	for {
		for i := 0; i < 100; i++ {
			m[i] = i
		}
	}
}

func readLoop(m map[int]int) {
	for {
		for k, v := range m {
			fmt.Println(k, "-", v)
		}
	}
}
```
- The above code creates a map, then starts two `goroutine`s that have access to the map
- One routine is continuously mutating the values stored in the map while the other is printing values from the map
- This program on a multi-core machine could run into the following error: `fatal error: concurrent map iteration and map write`
- This is where mutexes step in
```Go
package main

import (
	"fmt"
	"sync"
)

func main() {
	m := map[int]int{}

	mu := &sync.Mutex{}

	go writeLoop(m, mu)
	go readLoop(m, mu)

	// stop program from exiting, must be killed
	block := make(chan struct{})
	<-block
}

func writeLoop(m map[int]int, mu *sync.Mutex) {
	for {
		for i := 0; i < 100; i++ {
			mu.Lock()
			m[i] = i
			mu.Unlock()
		}
	}
}

func readLoop(m map[int]int, mu *sync.Mutex) {
	for {
		mu.Lock()
		for k, v := range m {
			fmt.Println(k, "-", v)
		}
		mu.Unlock()
	}
}
```
- Now, in both routines, we can lock before reading/writing, do the operation, then unlock, making sure these two operation happen in sync and not at the same time
- No other thread can lock the mutex while it's already locked, and if another thread attempts to lock it, it will be blocked until the mutex is unlocked
## RW Mutex
- The standard library also offers us the `sync.RWMutex` which provides these two new methods `.RLock()` and `.RUnlock()`
- This mutex improves performance for read-intensive processes, and allows multiple `goroutine`s to read from the map simultaneously since multiple `Rlock()` calls can occur at the same time
- However, if a `goroutine` already has a `Lock()` then all other locks, including `RLocks()` will be blocked until the routine unlocks
- Maps are actually safe for concurrent read access, but not for concurrent read/write or write/write access
- With read/write, all the reader will have access to the map at the same time, but a writer will still lock out all the reader and writers until it's done
```Go
package main

import (
	"fmt"
	"sync"
)

func main() {
	m := map[int]int{}

	mu := &sync.RWMutex{}

	go writeLoop(m, mu)
	go readLoop(m, mu)
	go readLoop(m, mu)
	go readLoop(m, mu)
	go readLoop(m, mu)

	// stop program from exiting, must be killed
	block := make(chan struct{})
	<-block
}

func writeLoop(m map[int]int, mu *sync.RWMutex) {
	for {
		for i := 0; i < 100; i++ {
			mu.Lock()
			m[i] = i
			mu.Unlock()
		}
	}
}

func readLoop(m map[int]int, mu *sync.RWMutex) {
	for {
		mu.RLock()
		for k, v := range m {
			fmt.Println(k, "-", v)
		}
		mu.RUnlock()
	}
}
```
# Generics
- Go didn't always have generics, and as it doesn't have classes either, that meant that it wasn't a very DRY language, and you had to write different versions of the same function per type
```Go
func splitIntSlice(s []int) ([]int, []int) {
    mid := len(s)/2
    return s[:mid], s[mid:]
}

func splitStringSlice(s []string) ([]string, []string) {
    mid := len(s)/2
    return s[:mid], s[mid:]
}
```
- Generics allow us to use variables to refer to specific types
```Go
func splitAnySlice[T any](s []T) ([]T, []T) {
    mid := len(s)/2
    return s[:mid], s[mid:]
}
```
- This means now we can have more abstract functions that reduce code duplication
- `T` is the name of the type parameter for `splitAnySlice()`, and it must match the `any` constraint, meaning it can be anything
```GO
firstInts, secondInts := splitAnySlice([]int{0, 1, 2, 3})
fmt.Println(firstInts, secondInts)
```
- To create the zero value of a type
```Go
var myZeroInt int

// Generic
var myZero T
```
## Constraints
- Constraints like `any` are interfaces that allow us to write generics that operate within the constraints of a given interface
- `any` is the same as the empty interface for example because it means the type in question can be anything
- We can also create custom constraints
- For example, a `concat()` function takes a slice of values and concatenates them into a string
- This function should be able to accept any type that can be represented as a string, even if it isn't a string
- An example for this is a `user` struct with a `.String()` method that returns the user's name and age as a string
```Go
type stringer interface {
    String() string
}

func concat[T stringer](vals []T) string {
    result := ""
    for _, val := range vals {
        // this is where the .String() method
        // is used. That's why we need a more specific
        // constraint instead of the any constraint
        result += val.String()
    }
    return result
}
```
- Based on boots, the benefit of using a generic in place of the interface itself, is that it still enforces the underlying concrete type, and doesn't consider all types to be the interface's type
- So if typeA and typeB implement interfaceA. Using generics `func [T interfaceA]()` and saying `func(typeA)` means that the function is running with knowledge that it's using typeA, instead of believing it's using interfaceA
## Interface Type Lists
- The release of generics brought about a new way for writing interfaces too
- Interfaces as we've seen so far are method-based, where a type satisfies the interface if it has all of its methods
- Now, we also have **(type-set) interfaces**
- These interfaces list the concrete (underlying) types that are allowed, instead of the methods
- They are intended to be used mainly as constraints on type parameters
- For example, to use `<` or `>` on a type parameter `T`, the compiler must know `T` is ordered
- A type-list interface would spell out exactly which types count as ordered
```Go
// Ordered matches any type that supports <, <=, >, and >=.
type Ordered interface {
    ~int | ~int8 | ~int16 | ~int32 | ~int64 |
        ~uint | ~uint8 | ~uint16 | ~uint32 | ~uint64 | ~uintptr |
        ~float32 | ~float64 |
        ~string
}

// Because T is constrained by Ordered, the compiler knows
// that < is valid for any T used with this function.
func Min[T Ordered](a, b T) T {
    if a < b {
        return a
    }
    return b
}
```
## Parametric Constraints
- Interface definitions can also accept type parameters
```Go
// The store interface represents a store that sells products.
// It takes a type parameter P that represents the type of products the store sells.
type store[P product] interface {
	Sell(P)
}

type product interface {
	Price() float64
	Name() string
}

type book struct {
	title  string
	author string
	price  float64
}

func (b book) Price() float64 {
	return b.price
}

func (b book) Name() string {
	return fmt.Sprintf("%s by %s", b.title, b.author)
}

type toy struct {
	name  string
	price float64
}

func (t toy) Price() float64 {
	return t.price
}

func (t toy) Name() string {
	return t.name
}

// The bookStore struct represents a store that sells books.
type bookStore struct {
	booksSold []book
}

// Sell adds a book to the bookStore's inventory.
func (bs *bookStore) Sell(b book) {
	bs.booksSold = append(bs.booksSold, b)
}

// The toyStore struct represents a store that sells toys.
type toyStore struct {
	toysSold []toy
}

// Sell adds a toy to the toyStore's inventory.
func (ts *toyStore) Sell(t toy) {
	ts.toysSold = append(ts.toysSold, t)
}

// sellProducts takes a store and a slice of products and sells
// each product one by one.
func sellProducts[P product](s store[P], products []P) {
	for _, p := range products {
		s.Sell(p)
	}
}

func main() {
	bs := bookStore{
		booksSold: []book{},
	}

    // By passing in "book" as a type parameter, we can use the sellProducts function to sell books in a bookStore
	sellProducts[book](&bs, []book{
		{
			title:  "The Hobbit",
			author: "J.R.R. Tolkien",
			price:  10.0,
		},
		{
			title:  "The Lord of the Rings",
			author: "J.R.R. Tolkien",
			price:  20.0,
		},
	})
	fmt.Println(bs.booksSold)

    // We can then do the same for toys
	ts := toyStore{
		toysSold: []toy{},
	}
	sellProducts[toy](&ts, []toy{
		{
			name:  "Lego",
			price: 10.0,
		},
		{
			name:  "Barbie",
			price: 20.0,
		},
	})
	fmt.Println(ts.toysSold)
}
```
- The name `T` is a variable name for the type parameter, which means, it could have been called anything, kinda like how the receiver in a struct method is conventionally the first character of the struct name
# Enums
## Lack of Enums
- Go's type system is the least powerful thing about it, being closer to C than Rust or Typescript. As such, Go has no enums, sum types, tagged unions, etc...
## Error handling
- `%w` is the verb (yes they're called verbs instead of format specifiers now) is a wrapper for error types in particular
```Go
user, err := getUser()
if err != nil {
    return fmt.Errorf("failed to get user: %w", err)
}
// do something with user
```
## Type definitions
- Since Go has no union types, we have to instead resort to other solutions, like type definitions
```Go
type sendingChannel string

const (
    Email sendingChannel = "email"
    SMS   sendingChannel = "sms"
    Phone sendingChannel = "phone"
)

func sendNotification(ch sendingChannel, message string) {
    // send the message
}
```
- This is a bit safer than using straight strings, but still not perfect
```Go
// This will be prevented
sendingCh := "slack"
sendNotification(sendingCh, "hello") // string is not sendingChannel

// But not this
// "slack" is automatically implied as a sendingChannel
sendNotification("slack", "hello")

// We can also still do this
sendingCh := "slack"
convertedSendingCh := sendingChannel(sendingCh)
sendNotification(convertedSendingCh, "hello")
```
## Iota
- Iota is a keyword that creates a sequence of numbers
- It tart at 0 and increment by 1 for each constant in a `const` block
```Go
type sendingChannel int

const (
    Email sendingChannel = iota
    SMS
    Phone
)
```
- Note that Iota is not an `enum`, and doesn't provide the benefits of an `enum`, such as type safety, as you can still assign any number to `sendingChannel`, even if it's outside the 3 defined values here
- It is however the closest thing we have, and still would create a list of numbered items that fall under `sendingChannel`
# Files
- We can interact with files in Go using the `os` package
- Example interactions are:
```Go
// Create a file
	dst, err := os.Create(assetDiskPath)
	if err != nil {
		respondWithError(w, http.StatusInternalServerError, "Unable to create file on server", err)
		return
	}
	defer dst.Close()

// Create a temp file
	tempDir, err := os.CreateTemp("", "tubely-upload.mp4")
	if err != nil {
		respondWithError(w, http.StatusInternalServerError, "Unable to create file on server", err)
		return
	}
	defer os.Remove(tempDir.Name())
	defer tempDir.Close()

// Reset file pointer after reading from a file
	if _, err = io.Copy(tempDir, file); err != nil {
		respondWithError(w, http.StatusInternalServerError, "Error saving file", err)
		return
	}

	_, err = file.Seek(0, io.SeekStart)
	if err != nil {
		respondWithError(w, http.StatusInternalServerError, "Failed to reset file pointer", err)
		return
	}
```
- All examples are taken from `handler_upload_video.go` and `handler_upload_thumbnail.go` from the `learn-file-storage-s3-golang-starter` boot.dev course
# Go Proverbs
```
Don't communicate by sharing memory, share memory by communicating.

Concurrency is not parallelism.

Channels orchestrate; mutexes serialize.

The bigger the interface, the weaker the abstraction.

Make the zero value useful.

interface{} says nothing.

Gofmt's style is no one's favorite, yet gofmt is everyone's favorite.

A little copying is better than a little dependency.

Syscall must always be guarded with build tags.

Cgo must always be guarded with build tags.

Cgo is not Go.

With the unsafe package there are no guarantees.

Clear is better than clever.

Reflection is never clear.

Errors are values.

Don't just check errors, handle them gracefully.

Design the architecture, name the components, document the details.

Documentation is for users.

Don't panic.
```
# More resources
- Go's official docs [https://pkg.go.dev/](https://pkg.go.dev/)