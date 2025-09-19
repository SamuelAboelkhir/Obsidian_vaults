---
tags: 
- PG
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[PG C index|Back to index]]
# String interpolation
- The act of passing the value of a variable in a string
- Most languages have a method that's similar to C (the G.O.A.T)
	- C: `printf("Hello %s", name)`
	- JS/TS: 
		```
		const name = "Alice";
		const age = 30;
		
		// Template literals with ${}
		const message = `Hello, ${name}! You are ${age} years old.`;
		console.log(message); // "Hello, Alice! You are 30 years old."
		
		// Expressions in template literals
		const result = `2 + 2 = ${2 + 2}`;
		console.log(result); // "2 + 2 = 4"
		
		// Multi-line strings
		const multiline = `
		Name: ${name}
		Age: ${age}
		Year born: ${new Date().getFullYear() - age};
		```
	- Python: 
		```
	    name = "Alice"
	    age = 30
		  
		// F-strings
		message = f"Hello, {name}! You are {age} years old."
		print(message)  # "Hello, Alice! You are 30 years old.
	
		// Expressions in f-strings
		result = f"2 + 2 = {2 + 2}"
		print(result)  # "2 + 2 = 4"
	
		// Format specifiers
		pi = 3.14159
		formatted = f"Pi is approximately {pi:.2f}"
		print(formatted)  # "Pi is approximately 3.14"
		
		// .format() Method
		message = "Hello, {}! You are {} years old.".format(name, age)
		
		// Can also use named placeholders
		message = "Hello, {name}! You are {age} years old.".format(name=name, age=age)
		```
	- Java: 
		```
		String name = "Alice";
		int age = 30;
		
		// String.format()
		String message = String.format("Hello, %s! You are %d years old.", name, age);
		System.out.println(message);
		
		// System.out.printf() (direct printing)
		System.out.printf("Hello, %s! You are %d years old.%n", name, age);
		```