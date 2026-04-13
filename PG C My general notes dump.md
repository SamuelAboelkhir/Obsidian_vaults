---
tags:
- C
- General
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG C index|Back to index]]

# C Basics
## Program Structure
- C programs require an entry point function
```C
#include <stdio.h>

int main() {
    printf("Program in C! \n");
    return 0;
}
```
## Comments
- Comments in C are written as follows
```C
#include <stdio.h>

int main() {
  /*
    Sneklang is for nvim enjoyers
    who want to write their own garbage
    collectors instead of using off-the-shelf
    solutions
  */
  printf("i use sneklang btw\n");
  printf("i use nvim btw\n");
  printf("i use arch btw\n");
  return 0;
}

// or like this
```
## Basic Types
- C brandishes the following data types
	- int
	- char
	- float
## Strings
- C doesn't have a built-in string type
- C strings are instead arrays of char
```C
char *msg_from_dax = "You still have 0 users";

//or

#include <stdio.h>

int main() {
 char *will_never_hear_again =
      "Hey TJ, when is the memory course in C gonna be done?";

  // don't touch below this line
  printf("%s\n", will_never_hear_again);
  return 0;
}
```
## Printing Variables
- In C, to do string interpolation, we have to specify the format the variable will be printed in
- For that we have to use specific format specifiers [[PG C Cheat Sheet#Format Specifiers]]
```C
#include <stdio.h>

int main() {
  int sneklang_default_max_threads = 8;
  char sneklang_default_perms = 'r';
  float sneklang_default_pi = 3.141592;
  char *sneklang_title = "Sneklang";
  // don't touch above this line

  printf("Default max threads: %d\nCustom perms: %c\nConstant pi value: %f\nSneklang title: %s \n", sneklang_default_max_threads, sneklang_default_perms, sneklang_default_pi, sneklang_title);

  return 0;
}
```
## Functions
- In C we have to specify the input and output types for functions
```C
float add(int x, int y) {
    return (float)(x + y);
}
```
- A function without input, or without output, can make use of the `void` type
```C
int get_integer(void) {
    return 42;
}

void print_integer(int x) {
    printf("this is an int: %d", x);
}
```
## Math Operators
- C has all of the usual math operators
	- `+`
	- `-`
	- `*`
	- `/`
- It also offers two other operators
	- `x++`: `+=1`
	- `x--`: `-=1`
- This is where C++ came from, it's the incremented version of C, or better C
- Both of the `++` and `--` operators can postfix or prefix a variable
```C
int a = 5;
int b = a++; // b is assigned 5, then a becomes 6

int a = 5;
int b = ++a; // a becomes 6, then b is assigned 6
```
- Postfix is the most common way of using the increment/decrement operators
## If Statements
- Nothing ground breaking, C was the originator of the if statement syntax used in other languages that use this same syntax like javascript
```C
if (x > 3) {
    printf("x is greater than 3\n");
} else if (x == 3) {
    printf("x is 3\n");
} else {
    printf("x is less than 3\n");
}

if (x > 3) printf("x is greater than 3\n");
```
## Type Sizes
- In C, the "size" (in memory) of a type is not guaranteed to be the same on all systems. That's because the size of a type is dependent on the system's architecture. 
- For example, on a 32-bit system, the size of an int is usually 4 bytes, while on a 64-bit system, the size of an int can sometimes be 8 bytes - of course, you never know until you run sizeof with the compiler you plan on using.
- However, some types are always guaranteed to be the same. Here’s a list of the basic C data types along with their typical sizes in bytes. 
- Note that sizes can vary based on the platform (e.g., 32-bit vs. 64-bit systems):
### Basic C Types and Sizes:
- char
	- Size: 1 byte
	- Represents: Single character.
	- Notes: Always 1 byte, but can be signed or unsigned.
- float
	- Size: 4 bytes
	- Represents: Single-precision floating-point number.
- double
	- Size: 8 bytes
	- Represents: Double-precision floating-point number.
- The actual sizes of these types can be determined using the sizeof operator in C for a specific platform, which we'll learn about next.
```C
#include <stdbool.h>
#include <stdio.h>

int main() {
  // Use %zu is for printing `sizeof` result
  printf("sizeof(char)   = %zu\n", sizeof(char));
}
```
- The return type of `sizeof` is `size_t`
- `size_t` is up to 4 bytes of 32-bit systems and 8 bytes of 64-bit systems
## Loops
- Not really worth covering, as again, they're the same as any other loop
- There is however one loop type, that I believe also exists in javascript that's interesting
```C
do {
    // Loop Body
} while (condition);
```
- This type of loop executes at least once before checking the condition unlike a normal `while` loop which would check the condition before ever running
## Pragma Once and Header Guards
- In C, we use special header files `.h` to basically make function declarations exposed to all other files
- They're kinda like `index.ts` files
- However, there is a caveat, as referencing the header file multiple times across a project would cause errors as you're technically redefining functions and structs that way
	- This is known as multiple inclusions
- One solution to avoid this is `#Pragma once` which when included at the top of a header file, tells the compiler to include this file only once even if it gets referenced multiple times in the program
```C
// my_header.h

#pragma once

struct Point {
    int x;
    int y;
};
```
- Another method, is the use of header guards, which define a unique macro (so it runs during the [[PG C compilation flow#Preprocessor|preprocessor]] phase) which would prevent a file from being processed again if it has already been processed once before
```C
#ifndef MY_HEADER_H
#define MY_HEADER_H

// some cool code

#endif
```
## Structs
- C is not object oriented, so instead of objects, we have structs, which are a way of grouping data
- Structs's usefulness expands beyond just grouping data, as C only allows us to return one value from a function, unlike languages like python or [[PG Go note dump|Go]]. We can however return a struct that includes multiple values
```C
struct Human {
    int age;
    char *name;
    int is_alive;
};
```
- We have multiple ways for initializing a struct
- Taking the following struct as an example
```C
struct City {
  char *name;
  int lat;
  int lon;
};
```
- We can initialize it, in the following ways
```C
// Zero Initalizer (sets all fields to 0)
int main() {
  struct City c = {0};
}

// Positional Initializer
int main() {
  struct City c = {"San Francisco", 37, -122};
}

// Designated Initializer
int main() {
  struct City c = {
    .name = "San Francisco",
    .lat = 37,
    .lon = -122
  };
}
```
- The Zero Initalizer behaves differently based on the data type it's initalizing
	- It's a `0` for ints floats and doubles
	- It's a `'\0` (null character, ASCII 0) for chars
	- It's `NULL` for pointers
	- It's `false` for `_Bool`
```C
struct Person {
  char *name;
  int age;
  char initial;
};

struct Person p = {0};
// p.name    == NULL
// p.age     == 0
// p.initial == '\0'
```
- We can access a struct's field with the `.`notation, similar to objects
```C
struct City c;
c.lat = 41; // Set the latitude
printf("Latitude: %d", c.lat); // Print the latitude
```
### Typedef
- When it comes to structs, we don't have to keep typing `struct Name` every time we want to mention the struct
- Instead, we can define a new type for the struct
```C
typedef struct Pastry {
    char *name;
    float weight;
} pastry_t;
```
- Now we can call `pastry_t` instead of `struct Pastry`
- the `_t` is a convention for indicating types, it's not forced naming
- We can even skip naming the struct alltogether
```C
typedef struct {
    char *name;
    float weight;
} pastry_t;

pastry_t muffin = {"Muffin", 0.3};
```
- This will work the same as the first example
- We can also check the size of a struct using `sizeof`
### Memory Layout
- Structs are stored contiguously in memory one field after another
- This means that for the following example
```C
typedef struct Coordinate {
    int x;
    int y;
    int z;
} coordinate_t;
```
- Here is the layout of the ints from the previous struct
![[memory_layout.png]]
- The previous layout is very neat, as the struct holds only one type
- For a mixed type struct
```C
typedef struct Human{
    char first_initial;
    int age;
    double height;
} human_t;
```
- The layout would look like this
![[memory_layout_mixed.png]]
- As we can see, there is a padding here equal to 3 bytes
- That's because the CPU can't access unaligned memory blocks, as it follows specific offsets based on the data in your struct
- This is due to the fact that you can have a array of those structs, and the offset is dependent on the largest type in the struct
- Hardware Efficiency: CPUs typically fetch data from memory in "chunks" (often 4 or 8 bytes at a time, known as a word). If a multi-byte value like an int or double is "split" across two of these chunks because it is unaligned, the CPU has to perform two memory reads instead of one and then do extra bit-shifting to piece the data back together. This is why alignment is so critical for performance.
- The Array Constraint: The compiler must ensure that if you have my_struct arr[2], the second element arr[1] starts at an address that satisfies the alignment of its most restrictive member (the one with the largest sizeof).
- Tail Padding: To satisfy the array constraint, the compiler adds "tail padding" to the end of a struct so that the total sizeof(struct) is a multiple of its largest member's alignment requirement.
- Member Ordering Matters: The amount of padding depends on the order of the fields. If you put a char between two ints, the compiler will add padding after the char to align the second int. If you put both ints first and the char last, you might only get tail padding.
- The Fetching Logic
	- When the CPU reads an int (4 bytes) from memory, it wants that int to start at an address that is a multiple of 4 (e.g., address 0, 4, 8, 12).
	- If you have an int followed by a char (5 bytes total), and you put them in an array, the second int in that array would start at offset 5. Address 5 is not a multiple of 4.
	- To "fix" this, the compiler adds 3 bytes of tail padding after the char. This makes the entire struct 8 bytes. Now, every int in an array of these structs will safely start at a multiple of 4 (0, 8, 16, etc.).
- Reading in Chunks
	- If that second int started at address 5, it would "straddle" the boundary between two 8-byte chunks (or words) of memory.
		1. The CPU would have to fetch the chunk containing addresses 0-7.
		2. Then fetch the chunk containing addresses 8-15.
		3. Then "mask" out the unwanted bits and "shift" the remaining bits together to reconstruct the 4-byte int.
	- By adding those 3 bytes of padding, the compiler ensures the CPU can grab the int in a single, clean architectural move.
# Stack and Heap
### Stack
- The stack in C is an OS controlled memory that acts as its name implies, as a stack
- It houses all the globally declared variables
- It also houses function calls in what's known as a stack frame, and the reason it acts as a stack, is because it actually follow LIFO, and pushes and pops items
### Heap
- The heap however has nothing to do with the actual data structure called heap
- It's a heap, because it's a heap of addresses that are dynamically allocated based on user input via `malloc`, `calloc` and `realloc`

# Realloc is unpredictable
- Now, realloc, is supposed to take a pointer and memory size, and move every thing in that pointer, to a new pointer, with the requested size, so: `int *ptr2 = realloc(ptr1, size);`
- Remember though that ptr1 must have been `malloc`ed first
- Also note that this operation doesn't have a consistent predictable result. Basically, it might fail, and nothing changes, it might work in place where you will have 2 ptrs pointing at the same address with the increased memory, or, a new memory block will be allocated to the new ptr with the copied data, and the original ptr will be freed

# Pointers
- You also have void pointers in C which are pointers with no specific type
- These pointers can't be dereferenced since they don't have a type to use to interpret the data in the address they are pointing to, so you need to cast them to a type first
- `Malloc` and `calloc` also actually return void pointers, but usually you assign them to a typed ptr so they end up being casted right away
- Why use a void ptr then? glad you asked "me", basically, they're useful for generic memory allocation, when you want to allocate memory to store data of any kind, and maybe cast it later. Like if you know want to be able to work with strings and ints, and you don't know in advance what will be passed to your function, so you just say the incoming data will be void, and then you check, well if type of data is "int" do this, if it's "str" do that

# Dynamic memory?
- You can actually create your own dynamic memory allocation with a struct
- Basically, say you want to make a stack struct, and it's supposed to theoretically, be able to hold an infinite number of elements, as many as the user pushes
- To do that, you can simply define a "Capacity" variable, and a "Count", and every time you push a new element to the new stack, increment your count by 1
- If count reaches capacity, well, now you know you're at the the end of the allocated stack size, but remember, C is not your friend, C will literally let you shoot yourself in the foot and be like "he's the dev, he knows what he's doing", so you have to add your own memory safety, that's why we're doing this, because otherwise, you can go out of bound, and potentially cause a "seg fault", so instead, when you reach the max capacity, now you can simply double your capacity, and keep going