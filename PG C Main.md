---
tags:
- C
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
- Structs's usefulness expands beyond just grouping data, as C only allows us to return one value from a function, unlike languages like python or [[PG Go Main|Go]]. We can however return a struct that includes multiple values
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
- Pointers are how we pass by reference in C and [[PG Go Main#Pointers|Go]]
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

## Union
- This is a combination of the structs and enums concepts
```C
typedef union AgeOrName {
  int age;
  char *name;
} age_or_name_t;
```
- The above union is basically a type that can hold either an `int`, or a `char *`, but not both at at the same time. If it could hold both, it would be a struct
```C
age_or_name_t lane = { .age = 29 };
printf("age: %d\n", lane.age);
// age: 29
```
- We can still try to access the name field in the above example, but we would get nothing, due to that counting as undefined behavior
- Since a union can only hold one of its possible types at one time, when defined, it will reserve as much memory as its biggest type only
- The reason for the undefined behavior encountered when trying to access a field that's not the one that was defined, is that all of the fields are sharing the same memory address, but if we use the `int` field and assign a value, then try to access the name field, then we're trying to read from the the memory block that has an `int` in it, and trying to interpret it as a `char` which results in us getting garbage, that's interpreted as nothing
- This fact is also the downside of  unions
```C
typedef union IntOrErrMessage {
  int data;
  char err[256];
} int_or_err_message_t;
```
- The above union is designed to hold an `int` most of the time, yet, every single instance of `IntOrErrMessage` is going to have a size of 256 bytes due to the possibility of a `char err[256]`
- A good and logical use case for a union, is the following
```C
typedef union Color {
  struct {
    uint8_t r;
    uint8_t g;
    uint8_t b;
    uint8_t a;
  } components;
  uint32_t rgba;
} color_t;
```
- Only 4 bytes are used. And, unlike in 99% of scenarios, it makes sense to both set _and_ get values from this union through both the `components` and `rgba` fields! Both fields in the union are exactly 32 bits in size, which means that we can "safely" (?) access the entire set of colors through the `.rgba` field, or get a single color component through the `.components` field.
## Stack and Heap
### Stack
- The stack in C is an OS controlled memory that acts as its name implies, as a stack
- It houses all the local variables are stored
- It also houses function calls in what's known as a stack frame, which is created to store the function's parameters and local variables and the reason it acts as a stack, is because it actually follow LIFO, and pushes and pops items, so when the function returns, the entire stack frame gets deallocated
```C
void create_typist(int uses_nvim) {
  int wpm = 150;
  char name[4] = {'t', 'e', 'e', 'j'};
}
```
![[empty_stack_memory.png]]
- Once called, the stack pointer is moved to make room for:
	- The return address (used to pickup execution after the function returns)
	- Arguments to the function
	- Local variables in the function body
![[stack_pointer_and_return_address.png]]
- The local variables will now be stored in the stack frame
![[stack_memory_with_stack_frame.png]]
- After the function returns, the stack frame gets deallocated by resetting the stack pointer to where the frame began
#### Why is the stack good?
- The stack is actually the preferred construct for memory allocation
- It's faster and simpler than the heap
- **Efficient Pointer Management:** Stack "allocation" is just a quick increment or decrement of the stack pointer, which is extremely fast. Heap allocations require more complex bookkeeping.
- **Cache-Friendly Memory Access:** Stack memory is stored in a contiguous block, enhancing cache performance due to spatial locality. Related values live next to each other in memory, the CPU can load and access them more quickly.
- **Automatic Memory Management:** Stack memory is managed automatically as functions are called and as they return.
- **Inherent Thread Safety:** Each thread has its own stack. Heap allocations require synchronization mechanisms when used concurrently, potentially introducing overhead. Meanwhile the heap is shared between all threads
- Our beloved friend [[PG Go Main|Go]] primarily uses stack allocation for variables, at least whenever possible. The Go compiler performs escape analysis to decide if a variable can be allocated on the stack. Languages like [[PG Python|Python]] though allocate most of their objects on the heap, which impacts their performance
- The stack does have a limited size, which means that eventually it can reach its limit, run out of memory, and cause a stack overflow, which is a very famous error in recursion if the function runs forever
### Heap
- The heap however has nothing to do with the actual data structure called heap
- It's a heap, because it's a heap of addresses that are dynamically allocated based on user input via `malloc`, `calloc` and `realloc`
- It's a pool of long-lived memory shared across the entire program
- The heap stores data that outlive the functions that created them
```C
int *new_int_array(int size) {
  int *new_arr = malloc(size * sizeof(int)); // Allocate memory
  if (new_arr == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    exit(1); // Exit if allocation fails
  }
  return new_arr;
}
```
- The above function is used to dynamically create an array based on user input
- This is because the size of the array wont be known at compile time, so it can't be put on the stack
- The function took a number of bytes to allocate as an argument, and returned a pointer to the allocated memory.
	- That pointer will be of type `void *` then converts to `int *` when assigned
![[memory_allocation_on_the_heap.png]]
- The function itself will be allocated to the stack when called, quite normally, and with the provided input, it will call `malloc` and allocate the requested memory on the heap
- The function's output `new_arr` is the pointer to the memory address on the heap, and that's allocated on the stack
```C
int* arr_of_6 = new_int_array(6);
arr_of_6[0] = 69;
arr_of_6[1] = 42;
arr_of_6[2] = 420;
arr_of_6[3] = 1337;
arr_of_6[4] = 7;
arr_of_6[5] = 0;
```
![[data_on_the_heap.png]]
- It's important to always `free` dynamically allocated memory that we no longer need
```C
free(arr_of_6);
```
![[freed_memory.png]]
- Free will deallocate the memory that we reserved earlier, allowing it to be reused and overwritten by new data
- The pointer itself will still exist though, but it's now a "dangling pointer" that's pointing to the address of the deallocated memory.
	- The variable holding the pointer should normally be reassigned to `null`
#### Malloc
- The malloc function which stands for "memory allocation" is the standard library function used to allocate memory on the heap. 
- It returns a pointer to the allocated memory
- The new memory is uninitialized, and can contain whatever data was there previously
- This is because, the act of freeing the data, means the memory block is no longer reserved by the system, but the just like how the previous pointer, now a dangling pointer, would still point to that data, the data itself will still exist until overwritten by other data
	- Not only that, but if the pointer was an int pointer, and the data gets overwritten by int data, dereferencing the pointer would still show what's there
	- If the data is overwritten by something else, like a char, then CPU may fail to interpret it as an int though
- It's is important to remember that with C, it's up to us to initialize the allocated memory (kinda like cleaning a newly rented apartment after the old tenants), and of course, freed once we're done with it
- `calloc` is the function of choice when we want to be sure the memory allocated is also initialized
- The initialized memory is normally set to all zeroes
	- Note that by returning all zeroes, this really means that all bytes are set to the zero value of the pointer's type, so `0` for ints `0.0` for floats and `\0` for char
```C
// Allocates memory for an array of 4 integers
int *ptr = malloc(4 * sizeof(int));
if (ptr == NULL) {
  // Handle memory allocation failure
  printf("Memory allocation failed\n");
  exit(1);
}
// use the memory here
// ...
free(ptr);
```
- Do note that not freeing memory means it will not be returned to the OS until the program actually exists, at which point the OS itself will clean the remnants of the terminated process, but we shouldn't count on the OS cleaning after us
- Being able to manage memory manually gives a decent performance bump as a trade off to how risky and error-prone it can get
#### Free
- Forgetting to free, is not just a matter of letting the OS clean after us, it can actually lead to memory leaks, because the occupied memory stays reserved and can't be reused, so the program may eventually run out of memory and crash
- This is also a very difficult bug to track, so it's best to not forget to free the memory
#### Big Endian and Little Endian
- Endianness is the order in which bytes are stored in memory
##### Big endian
- In big-endian systems, the most significant byte is stored first, at the lowest memory address
- The most significant byte, is the biggest part of the number
- Taking `0x12345678` as an example
![[big_endian.png]]
- The most significant byte here was `0x12`
##### Little endian
- In little-endian systems, the least significant byte, or the smallest part of the number, is stored first
![[little_endian.png]]
- So this time, it was `0x78`
- It's good to know that endnainess is mainly important when working with binary files, and in networking, but not really when writing code
- Most systems do use little-endian though, an the compiler handles the rest
## Pointer-pointers
- This is a pointer, that points toooooo "drum roll" a pointer
```C
int value;
int *pointer;
int **pointer_pointer;
```
- It's theoretically possible to use a pointer to a pointer to a pointer to a pointer infinitely, and then follow the path of pointers until you finally reach the data
![[pointer_to_pointer.png]]
### Array of pointers
- This is an array of `int`
```C
int *int_array = malloc(sizeof(int) * 3);
int_array[0] = 1;
int_array[1] = 2;
int_array[2] = 3;
```
- This is an array of pointers
```C
char **string_array = malloc(sizeof(char *) * 3);
string_array[0] = "foo";
string_array[1] = "bar";
string_array[2] = "baz";
```
- We've already been doing this more or less, since a string is a pointer to an array of `char`, so anytime we added strings to an array, that was technically a pointer array
### Void pointers
- In the context of void, this is a function that returns nothing `void update_soldier(soldier_t *s)`, and this is a function that takes no arguments `soldier_t new_soldier(void)`
- A void pointer `void *` is a pointer that could be pointing at anything, (C's any type basically), and therefore, this is also know as a "generic pointer"
	- These pointers can't be directly dereferenced since they have no specific type, nor can they be used in pointer arithmetic, unless we cast them first to another pointer type
#### Casting to void pointers
- The idea of casting to and from a void pointer is a unique concept to C
- This is because void pointers are type-agnostic, so when casting a specific pointer to a void one, no type info is retained, allowing the void pointer to point at anything, before we cast a type to it again to be able to dereference it
```C
int number = 42;
void *generic_ptr = &number;

// This doesn't work
printf("Value of number: %d\n", *generic_ptr);

// This works: Cast to appropriate type before dereferencing
printf("Value of number: %d\n", *(int*)generic_ptr);
```
- A common practice is to store generic data in one variable, and their type in another variable, which is useful when we want to move data around without necessarily knowing their type at compile time
```C
typedef enum DATA_TYPE {
  INT,
  FLOAT
} data_type_t;

void printValue(void *ptr, data_type_t type) {
  if (type == INT) {
    printf("Value: %d\n", *(int*)ptr);
  } else if (type == FLOAT) {
    printf("Value: %f\n", *(float*)ptr);
  }
}

int number = 42;
printValue(&number, INT);

float decimal = 3.14;
printValue(&decimal, FLOAT);
```
#### Generic swap
- Looking at this example first
```C
cool_person = "Lane"
uncool_person = "TJ"
cool_person, uncool_person = uncool_person, cool_person
print(cool_person)  # TJ
print(uncool_person)  # Lane
# (get rekt lane)
```
- Here, we knew the type of the data being swapped before hand, and therefore the compiler knew the sizes of the data that we wanted to swap
- For a generic swap to work, we have to tell the C compiler about the size of the data ourselves
```C
void swap(void *vp1, void *vp2, size_t size);
```
- We also can't assign pointer values directly with void `*ptr1=*ptr2`
- Instead, we can use `memcpy` from the `string.h` library
```C
void *memcpy(void *destination, void* source, size_t size);
```
- So to move the data
```C
memcpy(ptr1, ptr2, size);
```
#### Stack implementation (dynamic array)
- To use the previous concepts and implement a stack in C
- stack.c
```C
#include "snekstack.h"
#include <stdlib.h>

stack_t *stack_new(size_t capacity) {
  stack_t *stack = malloc(sizeof(stack_t));
  if (stack == NULL) {
    return NULL;
  }

  stack->count = 0;
  stack->capacity = capacity;
  stack->data = malloc(stack->capacity * sizeof(void *));
  if (stack->data == NULL) {
    free(stack);
    return NULL;
  }

  return stack;
}
```
- stack.h
```C
#include <stddef.h>

typedef struct Stack {
  size_t count;
  size_t capacity;
  void **data;
} stack_t;

stack_t *stack_new(size_t capacity);
void stack_push(stack_t *stack, void *obj);
void *stack_pop(stack_t *stack);
void stack_free(stack_t *stack);
```
- The reason we're using `void **data` is because our dynamic array is a an array of void pointers itself, so when we dereference the first layer to access the data, any index inside would still be a `void *` while `void *` directly, would mean that we're accessing one untyped and void construct as a whole
##### Count and capacity
- So what happens when the count and capacity are eventually equal after enough pushes to the array?
	- We have to make room for more data
- A simple approach to solve this, is to double the size of the array every time we reach the max capacity
```C
#include "snekstack.h"
#include <assert.h>
#include <stddef.h>
#include <stdlib.h>

void stack_push(stack_t *stack, void *obj) {
  if (stack->count == stack->capacity) {
    stack->capacity *= 2;
    void **temp = realloc(stack->data, stack->capacity * sizeof(void *));
    if (temp == NULL) {
      stack->capacity /= 2;
      return;
    }
    stack->data = temp;
  }
  stack->data[stack->count] = obj;
  stack->count++;
  return;
}
```
- Now, `realloc`, is supposed to take a pointer and memory size, and move every thing in that pointer, to a new pointer, with the requested size, so: `int *ptr2 = realloc(ptr1, size);`
- Remember though that ptr1 must have been `malloc`ed first
- Also note that this operation doesn't have a consistent predictable result. Basically, it might fail, and nothing changes, it might work in place where you will have 2 ptrs pointing at the same address with the increased memory, or, a new memory block will be allocated to the new ptr with the copied data, and the original ptr will be freed
- Then we also need to be able to pop from the array
```C
void *stack_pop(stack_t *stack) {
  if (stack->count == 0) {
    return NULL;
  }
  stack->count--;
  return stack->data[stack->count];
}
```
- We will also need a function to free all the memory allocated to the stack once we're done with it
```C
void stack_free(stack_t *stack) {
  if (stack == NULL) {
    return;
  }

  if (stack->data != NULL) {
    free(stack->data);
  }

  free(stack);
}
```
- Also, since the stack is made up of void pointers, we can literally store anything we want inside, so we can push none pointers, or chars or anything, as long as we cast it to `void *`
	- However, that would generally be a pretty bad idea
	- It's bad in our case anyway, as we would have to create all sorts of support functions and structs to hold type information to make this work, like how dynamic languages handle this exact same thing
	- In our case, that would cause problems when we try to dereference/free a non-pointer cast to a `void *` for example, but actually adding a pointer to any data type is generally fine, until we get to the point where we want to use it, and need to cast it back to its original type, which we no longer know, so we will again need some metadata about the underlying type of what the void pointer is even pointing at for this to work
## Objects in C
### Objects
- While C itself doesn't have any objects, what's to stop us from making our own?
- We can make objects by defining some higher level data structures that holds some metadata about itself such as
	- The type of data it holds
	- The size of the data
	- The data itself
	- How many references to itself exist (this is useful for garbage collection)
```C
typedef enum SnekObjectKind { INTEGER } snek_object_kind_t;

typedef union SnekObjectData {
  int v_int;
} snek_object_data_t;

typedef struct SnekObject {
  snek_object_kind_t kind;
  snek_object_data_t data;
} snek_object_t;
```
- Starting with integers, our integer will be allocated on the heap, and as mentioned, will be able to store metadata about itself
- To create it
```C
#include "snekobject.h"
#include <stdlib.h>

snek_object_t *new_snek_integer(int value) {
  snek_object_t *obj = malloc(sizeof(snek_object_t));
  if (obj == NULL) {
    return NULL;
  }

  obj->kind = INTEGER;
  obj->data.v_int = value;
  return obj;
}
```
- Next, we're adding floats
- We can extend our existing `enum` and `union` to include floats (and every other type we want to add)
```C
typedef enum SnekObjectKind {
  INTEGER,
  FLOAT,
} snek_object_kind_t;

typedef union SnekObjectData {
  int v_int;
  float v_float;
} snek_object_data_t;

snek_object_t *new_snek_integer(int value);
snek_object_t *new_snek_float(float value);
```
```C
#include "snekobject.h"
#include <stdlib.h>

snek_object_t *new_snek_float(float value) {
  snek_object_t *obj = malloc(sizeof(snek_object_t));
  if (obj == NULL) {
    return NULL;
  }

  obj->kind = FLOAT;
  obj->data.v_float = value;
  return obj;
}
```
- Strings will be a little different
- For ints and floats, it was easy to store the data in the object itself
- With strings, which are arrays of characters that can be of any length, we need to allocate memory for the string itself dynamically, separately from the object
```C
#include "snekobject.h"
#include <stdlib.h>
#include <string.h>

snek_object_t *new_snek_string(char *value) {
  snek_object_t *obj = malloc(sizeof(snek_object_t));
  if (obj == NULL) {
    return NULL;
  }

  int len = strlen(value);
  char *dst = malloc(len + 1);
  if (dst == NULL) {
    free(obj);
    return NULL;
  }

  strcpy(dst, value);

  obj->kind = STRING;
  obj->data.v_string = dst;
  return obj;
}
```
- The updated `.h` file
```C
typedef enum SnekObjectKind {
  INTEGER,
  FLOAT,
  STRING,
} snek_object_kind_t;

typedef union SnekObjectData {
  int v_int;
  float v_float;
  char *v_string;
} snek_object_data_t;

snek_object_t *new_snek_integer(int value);
snek_object_t *new_snek_float(float value);
snek_object_t *new_snek_string(char *value);
```
- Some objects can hold references to other objects
- In our case, that will be a `vector3`
- A `vector3` is a type of tuple, that can hold 3 elements
```C
#include "snekobject.h"
#include <stdlib.h>
#include <string.h>

snek_object_t *new_snek_vector3(snek_object_t *x, snek_object_t *y,
                                snek_object_t *z) {
  if (x == NULL || y == NULL || z == NULL) {
    return NULL;
  }

  snek_object_t *obj = malloc(sizeof(snek_object_t));
  if (obj == NULL) {
    return NULL;
  }

  obj->kind = VECTOR3;
  obj->data.v_vector3 = (snek_vector_t){.x = x, .y = y, .z = z};
  return obj;
}
```
- And the `.h` additions
```C
typedef struct SnekObject snek_object_t;

typedef struct {
  snek_object_t *x;
  snek_object_t *y;
  snek_object_t *z;
} snek_vector_t;

typedef enum SnekObjectKind {
  INTEGER,
  FLOAT,
  STRING,
  VECTOR3
} snek_object_kind_t;

typedef union SnekObjectData {
  int v_int;
  float v_float;
  char *v_string;
  snek_vector_t v_vector3;
} snek_object_data_t;

snek_object_t *new_snek_integer(int value);
snek_object_t *new_snek_float(float value);
snek_object_t *new_snek_string(char *value);
snek_object_t *new_snek_vector3(snek_object_t *x, snek_object_t *y,
                                snek_object_t *z);
```
- The last object for now will be an array, that will be dynamically sized
- This first part will include creating just the empty array
```C
#include "snekobject.h"
#include <stdlib.h>
#include <string.h>

snek_object_t *new_snek_array(size_t size) {
  snek_object_t *obj = malloc(sizeof(snek_object_t));
  if (obj == NULL) {
    return NULL;
  }

  snek_object_t **elements = calloc(size, sizeof(snek_object_t *));
  if (elements == NULL) {
    free(obj);
    return NULL;
  }

  obj->kind = ARRAY;
  obj->data.v_array = (snek_array_t){.size = size, .elements = elements};
  return obj;
}
```
- And our final `.h` file structure
```C
#include <stdbool.h>
#include <stddef.h>

typedef struct SnekObject snek_object_t;

int snek_length(snek_object_t *obj);
snek_object_t *snek_add(snek_object_t *a, snek_object_t *b);

typedef struct {
  size_t size;
  snek_object_t **elements;
} snek_array_t;

typedef struct {
  snek_object_t *x;
  snek_object_t *y;
  snek_object_t *z;
} snek_vector_t;

typedef enum SnekObjectKind {
  INTEGER,
  FLOAT,
  STRING,
  VECTOR3,
  ARRAY,
} snek_object_kind_t;

typedef union SnekObjectData {
  int v_int;
  float v_float;
  char *v_string;
  snek_vector_t v_vector3;
  snek_array_t v_array;
} snek_object_data_t;

typedef struct SnekObject {
  snek_object_kind_t kind;
  snek_object_data_t data;
} snek_object_t;

snek_object_t *new_snek_integer(int value);
snek_object_t *new_snek_float(float value);
snek_object_t *new_snek_string(char *value);
snek_object_t *new_snek_vector3(snek_object_t *x, snek_object_t *y,
                                snek_object_t *z);

snek_object_t *new_snek_array(size_t size);

bool snek_array_set(snek_object_t *array, size_t index, snek_object_t *value);

snek_object_t *snek_array_get(snek_object_t *array, size_t index);
```
### Functions
- We can now make helper functions that will allow us to get and set values to the array
- We will create `snek_array_set` which can set a value at a specific index in the array, and `snek_array_get` which can get a value from a specific index in the array
```C
#include "snekobject.h"
#include <stdlib.h>
#include <string.h>

bool snek_array_set(snek_object_t *snek_obj, size_t index,
                    snek_object_t *value) {
  if (snek_obj == NULL || value == NULL) {
    return false;
  }

  if (snek_obj->kind != ARRAY) {
    return false;
  }

  if (index >= snek_obj->data.v_array.size) {
    return false;
  }

  snek_obj->data.v_array.elements[index] = value;
  return true;
}

snek_object_t *snek_array_get(snek_object_t *snek_obj, size_t index) {
  if (snek_obj == NULL) {
    return NULL;
  }

  if (snek_obj->kind != ARRAY) {
    return NULL;
  }

  if (index >= snek_obj->data.v_array.size) {
    return NULL;
  }

  return snek_obj->data.v_array.elements[index];
}
```
- Next up, we will create a function that can give us the length of any of our objects
```C
#include "snekobject.h"
#include <stdlib.h>
#include <string.h>

int snek_length(snek_object_t *obj) {
  if (obj == NULL) {
    return -1;
  }

  switch (obj->kind) {
  case INTEGER:
  case FLOAT:
    return 1;
  case STRING:
    return strlen(obj->data.v_string);
  case VECTOR3:
    return 3;
  case ARRAY:
    return obj->data.v_array.size;
  default:
    return -1;
  }
}
```
- Finally, we want a way to be able to add different objects together
- This is how we will be able to perform addition for example with two int objects, or how we can concatenate two different array objects into one
```C
#include "snekobject.h"
#include <stdlib.h>
#include <string.h>

snek_object_t *snek_add(snek_object_t *a, snek_object_t *b) {
  if (a == NULL || b == NULL) {
    return NULL;
  }

  switch (a->kind) {
  case INTEGER:
    switch (b->kind) {
    case INTEGER:
      return new_snek_integer(a->data.v_int + b->data.v_int);
    case FLOAT:
      return new_snek_float((float)a->data.v_int + b->data.v_float);
    default:
      return NULL;
    }
  case FLOAT:
    switch (b->kind) {
    case FLOAT:
      return new_snek_float(a->data.v_float + b->data.v_float);
    default:
      return snek_add(b, a);
    }
  case STRING:
    switch (b->kind) {
    case STRING: {
      int a_len = strlen(a->data.v_string);
      int b_len = strlen(b->data.v_string);
      int len = a_len + b_len + 1;
      char *dst = calloc(len, sizeof(char));

      strcat(dst, a->data.v_string);
      strcat(dst, b->data.v_string);

      snek_object_t *obj = new_snek_string(dst);
      free(dst);

      return obj;
    }
    default:
      return NULL;
    }
  case VECTOR3:
    switch (b->kind) {
    case VECTOR3:
      return new_snek_vector3(
          snek_add(a->data.v_vector3.x, b->data.v_vector3.x),
          snek_add(a->data.v_vector3.y, b->data.v_vector3.y),
          snek_add(a->data.v_vector3.z, b->data.v_vector3.z));
    default:
      return NULL;
    }
  case ARRAY:
    switch (b->kind) {
    case ARRAY: {
      size_t a_len = (size_t)snek_length(a);
      size_t b_len = (size_t)snek_length(b);
      size_t length = a_len + b_len;

      snek_object_t *array = new_snek_array(length);

      for (int i = 0; i < a_len; i++) {
        snek_array_set(array, i, snek_array_get(a, i));
      }

      for (int i = 0; i < b_len; i++) {
        snek_array_set(array, i + a_len, snek_array_get(b, i));
      }

      return array;
    }
    default:
      return NULL;
    }
  default:
    return NULL;
  }
}
```
## Garbage collection
- A garbage collection is a program that automatically frees memory that's no longer in use. In other words, that's automatic memory management
- This is very convenient to have, but usually comes at the cost of performance
- This is why C, C++, Rust, and Zig don't use garbage collectors, and are great choices when every bit of performance matters
### Refcounting
- This is the simplest form of garbage collection
- In this algorithm, what we do is
	- Objects keep track of a `reference_count` int
	- When an object is referenced, its reference count increments
	- When an object is garbage collected, the reference count of any object that used to reference it, will decrement
	- If an object's reference count reaches zero, it will be garbage collected
- We will be building on the previous `objects` section
```C
snek_object_t *_new_snek_object() {
  snek_object_t *obj = calloc(1, sizeof(snek_object_t));
  if (obj == NULL) {
    return NULL;
  }

  obj->refcount = 1;

  return obj;
}
```
- In `.h`, we add the `refcount` to our objects
```C
typedef struct SnekObject {
  int refcount;
  snek_object_kind_t kind;
  snek_object_data_t data;
} snek_object_t;
```
- Now we can add the increment function, which will increment an object's `refcount` every time another object references it
```C
void refcount_inc(snek_object_t *obj) {
  if (obj == NULL) {
    return;
  }

  obj->refcount++;
  return;
}
```
- Next are the decrement and free functions
- The goal again, is to automatically free the allocated memory for an object that's no longer referenced
- Starting with ints, floats, and strings
```C
void refcount_dec(snek_object_t *obj) {
  if (obj == NULL) {
    return;
  }
  obj->refcount--;
  if (obj->refcount == 0) {
    refcount_free(obj);
  }
  return;
}

void refcount_free(snek_object_t *obj) {
  switch (obj->kind) {
  case INTEGER:
  case FLOAT:
    break;
  case STRING:
    free(obj->data.v_string);
    break;
  default:
    exit(1);
  }

  free(obj);
}
```
- Updating for our `vector3`
```C
snek_object_t *new_snek_vector3(snek_object_t *x, snek_object_t *y,
                                snek_object_t *z) {
  if (x == NULL || y == NULL || z == NULL) {
    return NULL;
  }
  snek_object_t *obj = _new_snek_object();
  if (obj == NULL) {
    return NULL;
  }
  obj->kind = VECTOR3;
  obj->data.v_vector3 = (snek_vector_t){.x = x, .y = y, .z = z};
  refcount_inc(x);
  refcount_inc(y);
  refcount_inc(z);
  return obj;
}

void refcount_free(snek_object_t *obj) {
  switch (obj->kind) {
  case INTEGER:
  case FLOAT:
    break;
  case STRING:
    free(obj->data.v_string);
    break;
  case VECTOR3: {
    refcount_dec(obj->data.v_vector3.x);
    refcount_dec(obj->data.v_vector3.y);
    refcount_dec(obj->data.v_vector3.z);
    break;
  }
  default:
    assert(false);
  }

  free(obj);
}
```
- Now updating the functions again to account for arrays
```C
bool snek_array_set(snek_object_t *snek_obj, size_t index,
                    snek_object_t *value) {
  if (snek_obj == NULL || value == NULL) {
    return false;
  }
  if (snek_obj->kind != ARRAY) {
    return false;
  }
  if (index >= snek_obj->data.v_array.size) {
    return false;
  }
  refcount_inc(value);
  if (snek_obj->data.v_array.elements[index] != NULL) {
    refcount_dec(snek_obj->data.v_array.elements[index]);
  }
  snek_obj->data.v_array.elements[index] = value;
  return true;
}

void refcount_free(snek_object_t *obj) {
  switch (obj->kind) {
  case INTEGER:
  case FLOAT:
    break;
  case STRING:
    free(obj->data.v_string);
    break;
  case VECTOR3: {
    snek_vector_t vec = obj->data.v_vector3;
    refcount_dec(vec.x);
    refcount_dec(vec.y);
    refcount_dec(vec.z);
    break;
  }
  case ARRAY: {
    snek_array_t array = obj->data.v_array;
    for (size_t i = 0; i < array.size; i++) {
      refcount_dec(array.elements[i]);
    }
    free(array.elements);
    break;
  }
  default:
    assert(false);
  }
  free(obj);
}
```
- And the full `add` function now is
```C
snek_object_t *snek_add(snek_object_t *a, snek_object_t *b) {
  if (a == NULL || b == NULL) {
    return NULL;
  }

  switch (a->kind) {
  case INTEGER:
    switch (b->kind) {
    case INTEGER:
      return new_snek_integer(a->data.v_int + b->data.v_int);
    case FLOAT:
      return new_snek_float((float)a->data.v_int + b->data.v_float);
    default:
      return NULL;
    }
  case FLOAT:
    switch (b->kind) {
    case FLOAT:
      return new_snek_float(a->data.v_float + b->data.v_float);
    default:
      return snek_add(b, a);
    }
  case STRING:
    switch (b->kind) {
    case STRING: {
      int a_len = strlen(a->data.v_string);
      int b_len = strlen(b->data.v_string);
      int len = a_len + b_len + 1;
      char *dst = calloc(len, sizeof(char));

      strcat(dst, a->data.v_string);
      strcat(dst, b->data.v_string);

      snek_object_t *obj = new_snek_string(dst);
      free(dst);

      return obj;
    }
    default:
      return NULL;
    }
  case VECTOR3:
    switch (b->kind) {
    case VECTOR3:
      return new_snek_vector3(
        snek_add(a->data.v_vector3.x, b->data.v_vector3.x),
        snek_add(a->data.v_vector3.y, b->data.v_vector3.y),
        snek_add(a->data.v_vector3.z, b->data.v_vector3.z)
      );
    default:
      return NULL;
    }
  case ARRAY:
    switch (b->kind) {
    case ARRAY: {
      size_t a_len = (size_t)snek_length(a);
      size_t b_len = (size_t)snek_length(b);
      size_t length = a_len + b_len;

      snek_object_t *array = new_snek_array(length);

      for (int i = 0; i <= a_len; i++) {
        snek_array_set(array, i, snek_array_get(a, i));
      }

      for (int i = 0; i <= b_len; i++) {
        snek_array_set(array, i + a_len, snek_array_get(b, i));
      }

      return array;
    }
    default:
      return NULL;
    }
  default:
    return NULL;
  }
}
```
### Mark and sweep
- The main drawback of the simple refcount GC is this
```C
snek_object_t *first = new_snek_array(1);
snek_object_t *second = new_snek_array(1);
// refcounts: first = 1, second = 1
snek_array_set(first, 0, second);
// refcounts: first = 1, second = 2
refcount_dec(first);
// refcounts: first = 0, second = 1
refcount_dec(second);
// refcounts: first = 0, second = 0
// all free!
```
- In the above example, everything works as expected, but trouble arises if `first` contained `second`, and `second` also contained `first` creating a cycle
- The issue here is that `first`'s reference count will drop to 1 after decrementing it, which won't trigger the GC, and because `first` never got freed, `second` also still keeps a reference count of 1, so neither of them can be automatically freed
```C
snek_object_t *first = new_snek_array(1);
  snek_object_t *second = new_snek_array(1);
  // refcounts: first = 1, second = 1
  snek_array_set(first, 0, second);
  // refcounts: first = 1, second = 2
  snek_array_set(second, 0, first);
  // refcounts: first = 2, second = 2
  refcount_dec(first);
  // refcounts: first = 1, second = 2
  refcount_dec(second);
  // refcounts: first = 1, second = 1
  
 void refcount_dec(snek_object_t *obj) {
  if (obj == NULL) {
    return;
  }
  obj->refcount--;
  if (obj->refcount == 0) {
    // this doesn't happen when refcount is 1
    return refcount_free(obj);
  }
  return;
} 
```
#### Pros and Cons
- The pros of MaS are:
	- It can detect cylces, and thus prevent memory leaks in certain cases
	- It has less on-demand bookkeeping
	- Reduces potential performance degradation in highly multi threaded programs, while refcounting would require atomic updates for thread safety
- Cons of MaS:
	- It's more complex to implement
	- Can cause "stop-the-world" pauses when lots of objects need to be freed, resulting in poor performance
	- Higher memory overhead
	- Less predictable performance
#### Stack Frames
- To implement MaS, we will use a struct called `vm_t` Virtual Machine Type
- This struct will simulate what would normally be tracked by a fully functional interpreted language
- stack.h
```C
#include <stddef.h>
#include <stdlib.h>

typedef struct Stack {
  size_t count;
  size_t capacity;
  void **data;
} stack_t;

stack_t *stack_new(size_t capacity);

void stack_push(stack_t *stack, void *obj);
void *stack_pop(stack_t *stack);

void stack_free(stack_t *stack);
void stack_remove_nulls(stack_t *stack);
```
- vm.h
```C
#include "stack.h"

typedef struct VirtualMachine {
  stack_t *frames;
  stack_t *objects;
} vm_t;

vm_t *vm_new();
void vm_free(vm_t *vm);
```
- The `frames` field holds a stack of frames which are pushed and popped as we enter and exit from different scopes
```C
msg1 = "This is in scope 1"
def outer_func():
    msg2 = "This is in scope 2"
    def inner_func():
        msg3 = "This is in scope 3"
        return
    return
```
- At each scope (function calls in our case), a new stack frame is pushed onto the `frames` stack, and when we exit a scope (function return), we pop the stack frame off the `frames` stack
- Since we use `void *` when working with generics in C, we wont know the data type held by `stack_t`, so we will need wrapper functions to help us make sure we don't push the wrong kinds of data into our stacks by mistake
- While `frames` is a stack for frames, `objects` is a stack for object pointers
- vm.c
```C
#include "vm.h"

vm_t *vm_new() {
  vm_t *vm = malloc(sizeof(vm_t));
  if (vm == NULL) {
    return NULL;
  }

  vm->frames = stack_new(8);
  if (vm->frames == NULL) {
    free(vm);
    return NULL;
  }

  vm->objects = stack_new(8);
  if (vm->objects == NULL) {
    stack_free(vm->frames);
    free(vm);
    return NULL;
  }

  return vm;
}

void vm_free(vm_t *vm) {
  if (vm == NULL) {
    return;
  }

  stack_free(vm->frames);
  stack_free(vm->objects);

  free(vm);
}
```
- stack.c
```C
#include "stack.h"
#include "munit.h"
#include <stdio.h>

void stack_push(stack_t *stack, void *obj) {
  if (stack->count == stack->capacity) {
    // Double stack capacity to avoid reallocing often
    stack->capacity *= 2;
    stack->data = realloc(stack->data, stack->capacity * sizeof(void *));
    if (stack->data == NULL) {
      // Unable to realloc, just exit :) get gud
      exit(1);
    }
  }

  stack->data[stack->count] = obj;
  stack->count++;

  return;
}

void *stack_pop(stack_t *stack) {
  if (stack->count == 0) {
    return NULL;
  }

  stack->count--;
  return stack->data[stack->count];
}

void stack_free(stack_t *stack) {
  if (stack == NULL) {
    return;
  }

  if (stack->data != NULL) {
    free(stack->data);
  }

  free(stack);
}

void stack_remove_nulls(stack_t *stack) {
  size_t new_count = 0;

  // Iterate through the stack and compact non-NULL pointers.
  for (size_t i = 0; i < stack->count; ++i) {
    if (stack->data[i] != NULL) {
      stack->data[new_count++] = stack->data[i];
    }
  }

  // Update the count to reflect the new number of elements.
  stack->count = new_count;

  // Optionally, you might want to zero out the remaining slots.
  for (size_t i = new_count; i < stack->capacity; ++i) {
    stack->data[i] = NULL;
  }
}

stack_t *stack_new(size_t capacity) {
  stack_t *stack = malloc(sizeof(stack_t));
  if (stack == NULL) {
    return NULL;
  }

  stack->count = 0;
  stack->capacity = capacity;
  stack->data = malloc(stack->capacity * sizeof(void *));
  if (stack->data == NULL) {
    free(stack);
    return NULL;
  }

  return stack;
}
```
- We now need to add some type safe functions for interacting with our stacks
```C
// The C compiler won't stop us :'(
stack_push(vm->frames, (void *)7);
stack_push(vm->frames, (void *)"uh oh");
```
- We will also add wrapper functions to help us make sure we only push `frame_t *` types onto `vm->frames`
```C
#include "vm.h"

void vm_frame_push(vm_t *vm, frame_t *frame) { stack_push(vm->frames, frame); }

frame_t *vm_new_frame(vm_t *vm) {
  frame_t *frame = malloc(sizeof(frame_t));
  frame->references = stack_new(8);

  vm_frame_push(vm, frame);
  return frame;
}

void frame_free(frame_t *frame) {
  stack_free(frame->references);
  free(frame);
}
```
#### Tracking Objects
- Our VM needs to be able to track every new object that we create
- Instead of tracking how many times an object is referenced, we will only check at GC time if each object is still referenced at all
- In vm.c
```C
void vm_track_object(vm_t *vm, snek_object_t *obj) {
  stack_push(vm->objects, obj);
}
```
- In sneknew.c
```C
snek_object_t *_new_snek_object(vm_t *vm) {
  snek_object_t *obj = calloc(1, sizeof(snek_object_t));
  if (obj == NULL) {
    return NULL;
  }
  vm_track_object(vm, obj);
  return obj;
}
```
#### Free
- We will also rewrite out freeing logic for MaS
- We no longer need `refcount_dec` since the VM is tracking the objects already
- In snekobject.c
```C
#include "snekobject.h"

void snek_object_free(snek_object_t *obj) {
  switch (obj->kind) {
  case INTEGER:
  case FLOAT:
    break;
  case STRING:
    free(obj->data.v_string);
    break;
  case VECTOR3: {
    break;
  }
  case ARRAY: {
    snek_array_t *array = &obj->data.v_array;
    free(array->elements);
    break;
  }
  }
  free(obj);
}
```
- in vm.c
```C
void vm_free(vm_t *vm) {
  for (size_t i = 0; i < vm->frames->count; i++) {
    frame_free(vm->frames->data[i]);
  }
  stack_free(vm->frames);
  for (size_t i = 0; i < vm->objects->count; i++) {
    snek_object_free(vm->objects->data[i]);
  }
  stack_free(vm->objects);
  free(vm);
}
```
#### Frame References
- We also need for each stack frame to know about all the objects that it references
- in vm.c
```C
void frame_reference_object(frame_t *frame, snek_object_t *obj) {
  stack_push(frame->references, obj);
}
```
#### Mark and Sweep
- The mark and sweep algorithm works in two phases
	- **Mark Phase:** Traverses the object graph, marking all reachable objects
	- **Sweep Phase:** Scan memory, collecting all unmarked objects, which are considered garbage
- We no longer care here about how many times an object is referenced, we instead just keep track of which objects are referenced in each stack frame, and then we traverse our container objects looking for any other referenced objects
- In snekobject.h
```C
typedef struct SnekObject {
  bool is_marked;

  snek_object_kind_t kind;
  snek_object_data_t data;
} snek_object_t;
```
- In sneknew.c
```C
snek_object_t *_new_snek_object(vm_t *vm) {
  snek_object_t *obj = calloc(1, sizeof(snek_object_t));
  if (obj == NULL) {
    return NULL;
  }
  obj->is_marked = false;
  vm_track_object(vm, obj);
  return obj;
}
```
#### Mark
- Different MaS implementations have different ways of marking the root objects, which are the objects referenced by the stack frame itself, but in our example, we will mark all the directly referenced objects
- In vm.c
```C
void mark(vm_t *vm) {
  for (size_t i = 0; i < vm->frames->count; i++) {
    frame_t *frame = vm->frames->data[i];
    for (size_t j = 0; j < frame->references->count; j++) {
      snek_object_t *obj = frame->references->data[j];
      obj->is_marked = true;
    }
  }
}
```
#### Trace
- With that, marking is done, and we can get into tracing
- In tracing, we go through all of our objects to determine which ones are connected to the roots
```python
def get_list():
  a = 5
  return [a]

print(get_list())
```
- If we run the above code, it will return a list with the integer `a` inside it
- Our current `mark` function will mark that list, but not the `a` integer inside it
- This means that when we go to sweep the memory, integer `a` will be freed, while it's being actively used, and the OS will fill the memory with something else
- We also have another problem
```python
def get_list():
    a = []
    a.append(a)
    return [5]

print(get_list())
```
- The above list references itself, then returns a completely different list
- Our trace function will consider this `a` list alive since it's referenced somewhere, even though that somewhere is itself, and `a` is not even reachable since `get_list` doesn't return it
- Tracing (which is part of the Mark phase) solves these 3 problems
- In vm.c
```C
void trace(vm_t *vm) {
  stack_t *gray_objects = stack_new(8);
  if (gray_objects == NULL) {
    return;
  }

  for (size_t i = 0; i < vm->objects->count; i++) {
    snek_object_t *obj = vm->objects->data[i];
    if (obj->is_marked) {
      stack_push(gray_objects, obj);
    }
  }

  while (gray_objects->count > 0) {
    void *top = stack_pop(gray_objects);
    trace_blacken_object(gray_objects, top);
  }

  stack_free(gray_objects);
}

void trace_blacken_object(stack_t *gray_objects, snek_object_t *obj) {
  switch (obj->kind) {
  case INTEGER:
  case FLOAT:
  case STRING:
    break;
  case VECTOR3: {
    snek_vector_t vec = obj->data.v_vector3;
    trace_mark_object(gray_objects, vec.x);
    trace_mark_object(gray_objects, vec.y);
    trace_mark_object(gray_objects, vec.z);
    break;
  }
  case ARRAY: {
    for (size_t i = 0; i < obj->data.v_array.size; i++) {
      trace_mark_object(gray_objects, obj->data.v_array.elements[i]);
    }
    break;
  }
  }
}

void trace_mark_object(stack_t *gray_objects, snek_object_t *obj) {
  if (obj == NULL || obj->is_marked) {
    return;
  }

  stack_push(gray_objects, obj);
  obj->is_marked = true;
}
```
#### Sweep
- Now, with every object having an `is_marked` field, we can use it to determine if an object is reachable or not
- All we need to do now is iterate over all the objects in the VM, and free any object that's not marked, and once it's freed, we can remove it from the VM completely
- In vm.c
```C
void vm_collect_garbage(vm_t *vm) {
  mark(vm);
  trace(vm);
  sweep(vm);
}

void sweep(vm_t *vm) {
  for (int i = 0; i < vm->objects->count; i++) {
    snek_object_t *obj = vm->objects->data[i];
    if (obj->is_marked) {
      obj->is_marked = false;
    } else {
      snek_object_free(obj);
      vm->objects->data[i] = NULL;
    }
  }

  stack_remove_nulls(vm->objects);
}
```