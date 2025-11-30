---
tags:
- C
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG C index|Back to index]]

# Functions
- Every function is added to the stack as a **stack frame** when it's called
- The stack frame starts with the return address then the function's arguments, local variables and ends with the **stack pointer (SP)**
- When the function returns, it's popped from the stack
- When a frame is popped, whatever data it created is left there in the **free** memory until it's overwritten by something else, and if a pointer was originally pointing at it, it will become a **dangling pointer** that should no longer be accessed
# Writable vs read-only memory: 
- Think of them as different “regions” where your program stores data, each with different rules.
## Big picture: memory regions in a C program
- Roughly, a typical C program’s memory is split into:
1. Text/Code segment – your compiled machine code (instructions).
	- Read-only, executable.
2. Read-only data segment – constants, like many string literals.
	- Read-only, non-executable.
	- Example: "hello" as a literal.
3. Data segment (global/static variables) – globals with initial values.
	- Writable.
	- Example:
```C
int counter = 0;
```
4. BSS segment – globals/statics that start as zero.
	- Writable.
	- Example:
```C
int global_array[1000]; // implicitly zero-initialized
```
5. Stack – local variables inside functions.
	- Writable.
	- Example:
```C
void f() {
    int x = 5;      // on stack
    char s[] = "hi"; // array on stack, modifiable
}
```
6. Heap – dynamically allocated memory (malloc, free, etc.).
	- Writable (until you free it).
	- Example:
```c
char *buf = malloc(100);
```
## Read-only vs writable in practice
- Read-only memory
	- You can read from it, but attempting to write to it is undefined behavior: often a crash (segfault).
	- Example: many implementations put string literals here:
```c
const char *msg = "Hello"; // often stored in read-only segment
// msg[0] = 'h';  // not allowed
```
- Writable memory
	- You can both read and write.
	- Stack, heap, and most global variables live here.
	- Example:
```c
char buf[] = "Hello"; // array in writable memory
buf[0] = 'h';         // perfectly fine
```