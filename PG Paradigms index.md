---
tags: 
- Index
- Paradigms
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]]
[[PG Object Oriented Programming]]
[[PG Functional Programming]]
# Coding paradigms
- **Declarative:**  
    “I **describe what I want** (result, constraints, relationships); a separate engine figures out the **how**.”  
    Often expressed as configs, queries, rule sets, or APIs that read more like data than like a recipe.
    
- **OOP:**  
    “I organize code as **objects** that hold both data and behavior, use methods, and rely on encapsulation and polymorphism.”
    
- **Imperative:**  
    “I write **commands** that change state step by step: assign, update, loop, branch.”
    
- **Procedural:**  
    “I write **functions/procedures** that operate on data; no objects owning both behavior and the data they work on.”
    
- **Functional:**  
    “I use **pure functions**, **avoid mutation**, and build logic by **composing** functions that map inputs to outputs.”

#### My interpretation:
so, basically, if you abstract the actual complex code, and expose an interface where you use flags, like switches to tell the code what you want to do, that's declarative. It relies heavily on abstraction, and potentially encapsulation

do the same thing while creating instances of a specific object, and that's OOP.

Explicitly define variables, and mutate them from one state to another. That's imperative, and if you don't use any objects, just functions in a sequence, then you're also doing it procedurally

Don't mutate variables, and instead make new instances of the same object, but with changes applied. That's functional