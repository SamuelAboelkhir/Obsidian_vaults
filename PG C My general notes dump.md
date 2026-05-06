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
## Pointers
- A pointer is a variable who's value is a memory address for a spot in memory that's holding the value of another variable. In essence, a pointer, points, at a location in memory
- The address of a variable can be printed by prefixing the variable with the address operator `&`
- The `%p` formatter is used to print a pointer (memory address)
- Since a pointer is just a memory address, that's holding the address to another memory location, that's holding actual data, there is nothing stopping us from even having a pointer to a pointer
- Pointers are how we pass by reference in C and [[PG Go note dump#Pointers|Go]]
	- This is because accessing data via the address of where it actually is located means that modifying the value held by the address will apply the modification to every single pointer that's pointing at said address, unlike copying the data to a completely new address (pass by value)
- To declare a variable as a pointer, we use `*`
```C
int meaning_of_life = 42;
int *pointer_to_mol = &meaning_of_life;
int value_at_pointer = *pointer_to_mol;
// value_at_pointer = 42
```
- Here, we declared an int variable that was assigned a memory address
- Then, we pointed at that address with an int pointer, since we're pointing at an int, by declaring the variable as a pointer using `*` and passing it the address of the variable with `&`
- Using the pointer as is will just print its value, which obviously is an address, but to use the value the pointer is pointing at, or in other words, to use the address, we can dereference the pointer, also with `*`
- If this was a pointer to a pointer, we would use `**`, because the first time, we dereference the first pointer's address giving us the address held by the 2nd pointer, and the 2nd `*` will dereference that 2nd address to the actual value at the end of the chain
- A pointer can also point to a struct, but accessing the fields of a struct through a pointer has a slightly different syntax than normal access
```C
// Normal access
coordinate_t point = {10, 20, 30};
printf("X: %d\n", point.x); // X: 10

// Pointer access
coordinate_t point = {10, 20, 30};
coordinate_t *ptrToPoint = &point;
printf("X: %d\n", ptrToPoint->x); // X: 10

// Or, dereference the pointer first
coordinate_t point = {10, 20, 30};
coordinate_t *ptrToPoint = &point;
printf("X: %d\n", (*ptrToPoint).x); // X: 10
```
- Note that the `.` operator has higher precedence that `*`, which is why the parentheses are necessary, to make sure the pointer is dereferenced before the field is accessed
### Virtual memory
- Physical memory refers to the actual RAM stick in the computer, while virtual memory is an abstraction over the physical memory that programs use
- The OS manages access to the physical memory
- A running program is called a process, and is given access to a chunk of virtual memory
- If the software in question is a firmware that runs without an OS, then it will use physical memory directly
- The reason for the existence of virtual memory is:
	1. **Isolation**: One process can't access the memory of another process.
	2. **Security**: The operating system can prevent processes from accessing certain parts of memory.
	3. **Simplicity**: Developers don't have to worry about managing physical memory and the memory of other processes.
	4. **Performance**: The operating system can optimize memory access depending on the hardware and needs of the program. For example, by moving data between physical memory and the hard drive.
- The idea to remember here is that, virtual memory is just an encapsulation over a process, where the process believes it has access to contiguous memory, starting from 0, but in reality, every address in the virtual memory may very well be scattered all over the physical stick
- Also, RAM is paginated, and virtual memory addresses are mapped to physical addresses using a paging table
### Arrays
- An array in C is a fixed size ordered collection of elements, that's indexed by ints starting at zero, and can only hold elements of the same type
- They are stored in contiguous memory just like structs
- Iterating over an array in C is only dooable via loops
```C
#include <stdio.h>

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};

    // Iterate and print each element
    for (int i = 0; i < 5; i++) {
        printf("%d ", numbers[i]);
    }
    printf("\n");

    return 0;
}
```
- To update an array element
```C
#include <stdio.h>

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};

    // Update some values
    numbers[1] = 20;
    numbers[3] = 40;

    // Print updated array
    for (int i = 0; i < 5; i++) {
        printf("%d ", numbers[i]);
    }
    printf("\n");

    return 0;
    
    // Output
    // 1 20 3 40 5
}
```
- An array in C is technically a pointer to the first element of the array, therefore, array indexing and pointer arithmetic can be used interchangeably to access array elements
```C
int numbers[5] = {1, 2, 3, 4, 5};
int *numbers_ptr = numbers;
// Access the third element (index 2)
int value = numbers[2];
// This is the same as
int value = *(numbers + 2);

```
- Here, `numbers + 2` computes the address of the third element, and `*` dereferences it to get the value
### Pointer arithmetic
- When we add an int to a pointer, the resulting pointer will be offset by that integer times the size of the data type
```C
int *p = numbers + 2;  // p points to the third element
int value = *p;        // value is 3
```

| Address | Element    | Value |
| ------- | ---------- | ----- |
| 0x1000  | numbers[0] | 1     |
| 0x1004  | numbers[1] | 2     |
| 0x1008  | numbers[2] | 3     |
| 0x100C  | numbers[3] | 4     |
| 0x1010  | numbers[4] | 5     |
- Accessing elements from the above table using pointers
	- `numbers + 0` or `&numbers[0]` points to `0x1000`
	- `numbers + 1` or `&numbers[1]` points to `0x1004`
	- `numbers + 2` or `&numbers[2]` points to `0x1008`
	- `numbers + 3` or `&numbers[3]` points to `0x100C`
	- `numbers + 4` or `&numbers[4]` points to `0x1010`
```C
#include <stdio.h>

int main() {
  int numbers[5] = {1, 2, 3, 4, 5};

  // Accessing elements using array indexing
  printf("numbers[2] = %d\n", numbers[2]);  // Output: 3

  // Accessing elements using pointers
  printf("*(numbers + 2) = %d\n", *(numbers + 2));  // Output: 3

  // Pointer arithmetic
  int *ptr = numbers;
  printf("Pointer ptr points to numbers[0]: %d\n", *ptr);  // Output: 1
  ptr += 2;
  printf("Pointer ptr points to numbers[2]: %d\n", *ptr);  // Output: 3

  return 0;
}
```
- We can also create arrays of structs
```C
typedef struct Coordinate {
  int x;
  int y;
  int z;
} coordinate_t;

coordinate_t points[3] = {
  {1, 2, 3},
  {4, 5, 6},
  {7, 8, 9}
};

printf("points[1].x = %d, points[1].y = %d, points[1].z = %d\n",
  points[1].x, points[1].y, points[1].z
);
// points[1].x = 4, points[1].y = 5, points[1].z = 6

coordinate_t *ptr = points;
printf("ptr[1].x = %d, ptr[1].y = %d, ptr[1].z = %d\n",
  (ptr + 1)->x, (ptr + 1)->y, (ptr + 1)->z
);
// ptr[1].x = 4, ptr[1].y = 5, ptr[1].z = 6
```
- Here is the memory layout:

| Address  | Element       | Value | Offset (bytes) |
| -------- | ------------- | ----- | -------------- |
| `0x2000` | `points[0].x` | 1     | 0              |
| `0x2004` | `points[0].y` | 2     | 4              |
| `0x2008` | `points[0].z` | 3     | 8              |
| `0x200C` | `points[1].x` | 4     | 12             |
| `0x2010` | `points[1].y` | 5     | 16             |
| `0x2014` | `points[1].z` | 6     | 20             |
| `0x2018` | `points[2].x` | 7     | 24             |
| `0x201C` | `points[2].y` | 8     | 28             |
| `0x2020` | `points[2].z` | 9     | 32             |
### Array casting
- Since arrays in most cases are just pointers, if we have an array of 3 structs full of ints, since arrays are contiguous in memory, we can actually cast it into another array of ints
```C
coordinate_t points[3] = {
  {5, 4, 1},
  {7, 3, 2},
  {9, 6, 8}
};

int *points_start = (int *)points;

for (int i = 0; i < 9; i++) {
  printf("points_start[%d] = %d\n", i, points_start[i]);
}
/*
points_start[0] = 5
points_start[1] = 4
points_start[2] = 1
points_start[3] = 7
points_start[4] = 3
points_start[5] = 2
points_start[6] = 9
points_start[7] = 6
points_start[8] = 8
*/
```
### Pointer size
- The size of a pointer is independent of the size of the data it "points" at
- The size of a pointer is actually determined by the system's architecture, whether it's 32-bit, 64-bit or something else
- This is of course because the pointer just hold the hex number representing the data's memory address
- This is unlike the size of an array, which is a contiguous block of memory where the data are of a specific size
```C
int *intPtr;
char *charPtr;
double *doublePtr;
printf("Size of int pointer: %zu bytes\n", sizeof(intPtr));
printf("Size of char pointer: %zu bytes\n", sizeof(charPtr));
printf("Size of double pointer: %zu bytes\n", sizeof(doublePtr));
// Size of int pointer: 4 bytes
// Size of char pointer: 4 bytes
// Size of double pointer: 4 bytes

int intArray[10];
char charArray[10];
double doubleArray[10];
printf("Size of int array: %zu bytes\n", sizeof(intArray));
printf("Size of char array: %zu bytes\n", sizeof(charArray));
printf("Size of double array: %zu bytes\n", sizeof(doubleArray));
// Size of int array: 40 bytes
// Size of char array: 10 bytes
// Size of double array: 80 bytes
```
### Array decays to pointers
- Arrays are like pointers, but they're not actually pointers
- As we said before, an array allocates memory for all the elements that it holds, while a pointer just holds the address of the data
- The reason arrays behave like pointers, is because the array's name can decay into a pointer to the first element of the array
#### When do arrays decay
- Arrays decay when used in expressions containing pointers
```C
int arr[5];

// 'arr' decays to 'int*' because that's the type of 'ptr'
int *ptr = arr;

// 'arr' decays to 'int*' to perform pointer arithmetic
int value = *(arr + 2);
```
- They also decay when passed to functions, so arrays are always passed by reference
#### When do arrays not decay
- Arrays don't decay when the `sizeof` operator is used on them, as it will return the size of the full array, not just the pointer
- Taking the address of an array with `&` actually gives a pointer to the full array, not just the first element
	- The `&arr` type is a pointer to the whole array. For example `int (*)[5]` is an int array with 5 elements
- After initialization the array is fully allocated to the memory without decaying
### C strings
```C
char *msg = "ssh terminal.shop for the best coffee";
```
- In the above example, `char` is a pointer to the first character in the string
- A C string is
	- Any number of chars terminated by a null character `('\0')`
	- A pointer to the first element of a char array
- Most string manipulation in C is done using pointers to move around the array
- The null terminator is very critical for determining the end of a string
	- The null terminator is usually added automatically to the end of a char array
- C strings don't store their length, which instead is determined by the position of the null terminator
- A function like `strlen` calculates the length of a string by iterating through the characters until the null terminator is found
- The fact C doesn't store string lengths, means that if we're not careful, it's possible to cause buffer overflows, and off-by-one errors during string operations
- A C string can be declared using arrays or pointers
```C
char str1[] = "Hi";
char *str2 = "Snek";
printf("%s %s\n", str1, str2);
// Output: Hi Snek
```
- Memory wise
```C
// notice we aren't using all 50 characters
char first[50] = "Snek";
char *second = "lang!";
strcat(first, second);
printf("Hello, %s\n", first);
// Output: Hello, Sneklang!
```
- `strcat` appends the 2nd argument to the first
- First in memory might look like this

| 'S'    | 'n'    | 'e'    | 'k'    | '\0'   | ????   | ... | ????   |
| ------ | ------ | ------ | ------ | ------ | ------ | --- | ------ |
| 0x3000 | 0x3001 | 0x3002 | 0x3003 | 0x3004 | 0x3005 | ... | 0x3031 |
- Second

| 'l'    | 'a'    | 'n'    | 'g'    | '!'    | '\0'   |
| ------ | ------ | ------ | ------ | ------ | ------ |
| 0x4000 | 0x4001 | 0x4002 | 0x4003 | 0x4004 | 0x4005 |
- First and second concatenated

| 'S'    | 'n'    | 'e'    | 'k'    | 'l'    | 'a'    | 'n'    | 'g'    | '!'    | '\0'   | ????   | ... | ????   |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | --- | ------ |
| 0x3000 | 0x3001 | 0x3002 | 0x3003 | 0x3004 | 0x3005 | 0x3006 | 0x3007 | 0x3008 | 0x3009 | 0x300A | ... | 0x3031 |
- Even though the array `first` had a lot more space left, `strcat` was able to determine the end of first using the null terminator, and appended second after it
#### C string library
- The C standard library provides a set of functions to manipulate strings in the `<string.h>` header file
- Some of the most commonly used ones are
- [`strcpy`](https://en.cppreference.com/w/c/string/byte/strcpy): Copies a string to another.    
```c
char src[] = "Hello";
char dest[6];
strcpy(dest, src);
// dest now contains "Hello"
```
- [`strncpy`](https://en.cppreference.com/w/c/string/byte/strncpy): Copies a _specified number of characters_ from one string to another.
```c
char src[] = "Hello";
char dest[6];
strncpy(dest, src, 3);
// dest now contains "Hel"
dest[3] = '\0';
// ensure null termination
```
- [`strcat`](https://en.cppreference.com/w/c/string/byte/strcat): Concatenates (appends) one string to another.
```c
char dest[12] = "Hello";
char src[] = " World";
strcat(dest, src);
// dest now contains "Hello World"
```
- [`strncat`](https://en.cppreference.com/w/c/string/byte/strncat): Concatenates a _specified number of characters_ from one string to another.
```c
char dest[12] = "Hello";
char src[] = " World";
strncat(dest, src, 3);
// dest now contains "Hello Wo"
```
- [`strlen`](https://en.cppreference.com/w/c/string/byte/strlen): Returns the length of a string (excluding the null terminator).

```c
char str[] = "Hello";
size_t len = strlen(str);
// len is 5
```
- [`strcmp`](https://en.cppreference.com/w/c/string/byte/strcmp): Compares two strings lexicographically.

```c
char str1[] = "Hello";
char str2[] = "World";
int result = strcmp(str1, str2);
// result is negative since "Hello" < "World"
```
- [`strchr`](https://en.cppreference.com/w/c/string/byte/strchr): Finds the first occurrence of a character in a string.

```c
char str[] = "Hello";
char *pos = strchr(str, 'l');
// pos points to the first 'l' in "Hello"
```
- [`strstr`](https://en.cppreference.com/w/c/string/byte/strstr): Finds the first occurrence of a substring in a string.

```c
char str[] = "Hello World";
char *pos = strstr(str, "World");
// pos points to "World" in "Hello World"
```
## Forward declaration
- Sometimes a struct needs to reference itself, like in the case of a node struct in a linked list
```C
typedef struct Node {
  int value;
  node_t *next;
} node_t;
```
- Node here is not defined yet, and the compiler will complain about using declaring it this way
- We can solve this using a forward declaration
```C
typedef struct Node node_t;

typedef struct Node {
  int value;
  node_t *next;
} node_t;
```
- This lets the compiler know about the existence of the struct even before it's fully defined
- The forward declaration must match the eventual definition
- The following isn't allowed
```C
typedef struct Node node_t;

typedef struct BadName {
  int value;
  node_t *next;
} node_t;
```
### Mutual structs
- If two structs reference each other in a circular reference, that's another use case for forward declaration
```C
typedef struct Computer computer_t;
typedef struct Person person_t;

struct Person {
  char *name;
  computer_t *computer;
};

struct Computer {
  char *brand;
  person_t *owner;
};
```
## Enums
- To define an enum in C
```C
typedef enum DaysOfWeek {
  MONDAY,
  TACO_TUESDAY,
  WEDNESDAY,
  THURSDAY,
  FRIDAY,
  SATURDAY,
  FUNDAY,
} days_of_week_t;
```
- The `typedef` and `days_of_week_t` parts are of course still optional, but conventional
- To then use the enum
```C
typedef struct Event {
  char *title;
  days_of_week_t day;
} event_t;

// Or if you don't want to use the alias:

typedef struct Event {
  char *title;
  enum DaysOfWeek day;
} event_t;
```
- Although enums by default are enumerated starting from 0, we can still give them explicit values
```C
typedef enum {
  EXIT_SUCCESS = 0,
  EXIT_FAILURE = 1,
  EXIT_COMMAND_NOT_FOUND = 127,
} ExitStatus;

// Alternatively, you can define the first value and let the compiler fill in the rest (incrementing by 1):
typedef enum {
  LANE_WPM = 200,
  PRIME_WPM, // 201
  TEEJ_WPM,  // 202
} WordsPerMinute;
```
### Switch
- C also supports switch, which pairs very well with enums
```C
switch (logLevel) {
  case LOG_DEBUG:
    printf("Debug logging enabled\n");
    break;
  case LOG_INFO:
    printf("Info logging enabled\n");
    break;
  case LOG_WARN:
    printf("Warning logging enabled\n");
    break;
  case LOG_ERROR:
    printf("Error logging enabled\n");
    break;
  default:
    printf("Unknown log level: %d\n", logLevel);
    break;
}

// You can allow fallthrough
switch (errorCode) {
  case 1:
  case 2:
  case 3:
    // 1, 2, and 3 are all minor errors
    printf("Minor error occurred. Please try again.\n");
    break;
  case 4:
  case 5:
    // 4 and 5 are major errors
    printf("Major error occurred. Restart required.\n");
    break;
  default:
    printf("Unknown error.\n");
    break;
}
```
### Sizeof enum`
- Since enums are at the end of the day, just ints, the size of an enum is the same as an int
- In case a very large enum that surpasses the size of a normal int is used, the C compiler will assign it a larger int type
- 
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
