---
tags:
- Paradigms
- Programming-Language
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Paradigms index|Back to index]]

# Functional programming
## What Is Functional Programming?
- Functional programming is a style (or "paradigm" if you're pretentious) of programming where we compose functions instead of mutating state (updating the value of variables).
	- [Functional programming](https://en.wikipedia.org/wiki/Functional_programming) is more about declaring _what_ you want to happen, rather than _how_ you want it to happen.
	- [Imperative](https://en.wikipedia.org/wiki/Imperative_programming) (or procedural) programming declares both the _what_ and the _how_.

 **Example of imperative code:**
```python
car = create_car()
car.add_gas(10)
car.clean_windows()
```
 **Example of functional code:**
```python
return clean_windows(add_gas(create_car()))
```
- The important distinction is that in the functional example, we never change the value of the `car` variable, we just compose functions that return new values, with the outermost function, `clean_windows` in this case, returning the final result.