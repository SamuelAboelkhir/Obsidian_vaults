---
tags:
- C
- General
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG C index|Back to index]]

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