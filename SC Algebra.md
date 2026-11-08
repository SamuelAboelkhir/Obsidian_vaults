---
tags:
- math
MOC: Sciences
---
[[_0000 Home|Home]] | [[_0004 Sciences MOC|Back to Sciences MOC]] | [[SC Mathematics index|Back to index]]
# The structure of numbers
## Real Numbers
- Natural numbers start from 1 and onward, denoted `ℕ`
- Whole numbers start from 0. denoted `𝕎`
- Adding negative numbers turns the set to integers, denoted `ℤ` from the German word "Zahlen"
- To show a number's membership to a set, we use `ϵ`, as in 1 ϵ Z, which means 1 is an integer
- The inverse is ~~ϵ~~
- Rational numbers are ratios or fractions of integers $\mathbb{Q}=\left\{ \frac{a}{b} \;\middle|\; a,b \in \mathbb{Z},\ b \neq 0 \right\}$ the | symbol means "such that"
- The rational numbers set is denoted ℚ from "quotients" (the result of a division)
- Not all numbers are rational, and a famous example is `π`, which is considered "irrational" π ~~ϵ~~ ℚ, so are `e` and $\sqrt2$
- Real numbers denoted `ℝ` are, simply put, everything mentioned so far
### The structure of real numbers
- Every set mentioned so far is a layer below the one that comes after it, which means that `ℕ` are part of `𝕎` which is part of `ℤ` an so on
- This can also be represented like so `ℕ ⊆ 𝕎 ⊆ ℤ ⊆ ℚ ⊆ ℝ`
- There is another layer which I'm not getting into here, because the course didn't get into it, called complex numbers, which includes `i`, and `i^2` = -1
### Sets of numbers

| Name               | Symbol | Description                                                                                                                                                                                                                                                                                          |
| ------------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Natural numbers    | ℕ      | The **counting** numbers, {1,2,3,…}{1,2,3,…}left brace, 1, comma, 2, comma, 3, comma, dots, right brace                                                                                                                                                                                              |
| Whole numbers      | 𝕎     | The natural numbers, plus zero: {0,1,2,3,…}{0,1,2,3,…}left brace, 0, comma, 1, comma, 2, comma, 3, comma, dots, right brace                                                                                                                                                                          |
| Integers           | ℤ      | All numbers that can be written without a fractional part, the natural numbers in addition to 000 and the negative numbers, {…−3,−2,−1,0,1,2,3,…}{…−3,−2,−1,0,1,2,3,…}left brace, dots, minus, 3, comma, minus, 2, comma, minus, 1, comma, 0, comma, 1, comma, 2, comma, 3, comma, dots, right brace |
| Rational Numbers   | ℚ      | All numbers that can be written as a **ratio** or **quotient** of two integers, the integers in addition to all fractions                                                                                                                                                                            |
| Irrational Numbers | none   | All number that cannot be written as a ratio of two integers. For example, π and ​$\sqrt2$ are irrational numbers.                                                                                                                                                                                   |
| Real Numbers       | ℝ      | All numbers that are either rational or irrational.                                                                                                                                                                                                                                                  |
- 0 Is commonly included in the natural numbers set in computer science, but in math the inclusion has to be explicit

![[Pasted image 20261101004454.png|457]]

## Square roots
For $a\geq0$ the square root of a is the non-negative number b ϵ ℝ such that $b^2$ = a

When it comes to irrational numbers, rounding is approximation. So $\sqrt3$ for example is 1.732... and it goes on, but stopping at 1.732 is just an approximation, not the real value of $\sqrt3$
## Properties of addition and multiplication
- A real number is any number with a decimal expansion
- These come in two options:
	- Numbers with repetition, which can be infinite like 1/3 which is 0.333333... forever, or $0.\overline{3}$, or finite such as 1/2, as the 0.5 is in fact 0.50000... but we don't write the zeros, nor give it an overline like with 0.3
	- The other type has no repetitions, like with 𝛑 which is 3.14159..., or $\sqrt2$ which is 1.414213... or e, which is 2.718...
- As mentioned before, the ones with repetitions are called rational numbers, which are members of the ℚ set, and they can be written as fractions, while the ones that have no repetition are irrational and can't be written as fractions. They can also be thought of as all the real numbers that are not ℚ
- Also, note that properties apply to operations, within the context of a number set, not the set itself
### Closures
Let a, b ϵ R. Then a + b ϵ ℝ, a . b ϵ ℝ

This property tells us that any operation done on a real number, will output another real number, never a complex one. In other words, the set of real numbers is closed with respect to addition and multiplication. Although subtractions and divisions (as long as the divisor isn't zero) are also closed under real numbers, we mainly use additions and multiplications when talking about the various properties, because they're well behaved operations, and they seem to work with all the properties of a real number

An example of a set that's not closed would be whole numbers, since 2-3 would be -1, which is an integer, not a whole number
### Commutative
Order also matters here, since 3-2 would have been fine
a + b = b + a and a . b = b . a
This property tells us that order does not matter in real numbers, even though it did with the whole number example
But we're still focusing only on addition and multiplication, like we did with closures, because subtraction is actually not commutative, $2-3 \neq 3-2$ 
### Associative
a + (b + c) = (a + b) + c, same for multiplication. This means that the placement of parentheses doesn't matter, so regardless of which two variables we associate in addition or multiplication, it wont matter, but it does for subtraction and divison
### Identity
This one means that adding any number to zero returns that same number 0 + a = a + 0 = a, and multiplying any number by 1 returns the same number
### Distribution
a . (b + c) = ab + ac the distribution of a multiplier to the two sides of an associated addition
## Order of operations
Parentheses and brackets come first, then exponents and roots, then multiplication and division, then addition and subtraction. Each pair are equal and either member of the pair can come first as you go left to right

## Properties with rational numbers
### Adding or subtracting fractions
$\frac{a}{b} \pm \frac{c}{d} = \frac{ad \pm cb}{bd}$

So something like $\frac{1}{2} + \frac{3}{5} = \frac{5 + 6}{10} = \frac{11}{10}$

This is called the "bow tie" method
### Multiplication
That's much simpler, just $\frac{a}{b}.\frac{c}{d} = \frac{ac}{bd}$
### Division
$(\frac{a}{b})\div(\frac{c}{d}) = \frac{(a/b)}{(c/d)} = \frac{a}{b}.\frac{d}{c} = \frac{ad}{bc}$

We basically convert the division into the multiplication that we already know

## Examples
Can we prove that -1 = 1?

$\sqrt{a} . \sqrt{b} = \sqrt{ab}$
let a = b = -1
$\sqrt{-1}.\sqrt{-1} = \sqrt{(-1).(-1)} = \sqrt{1} = 1$
BUT, $\sqrt{-1}.\sqrt{-1}$ is the number squared that's equal to the thing under the radical, like saying $\sqrt{25} . \sqrt{25}$ which is 5 . 5 or $5^2$ which is 25, so $\sqrt{25} . \sqrt{25} = 25$, and $\sqrt{-1}.\sqrt{-1} = -1$ by the same definition
That makes -1 = 1, which obviously makes absolutely no sense

This is because we made the false assumption to begin with that a = b = -1 right after saying $\sqrt{a} . \sqrt{b} = \sqrt{ab}$, which already implies that $(a,b\geq0)$ because we're not allowed to talk about the square root of negative numbers because that's related to complex numbers
## Final notes on working with fractions
### Definitions

For a fraction $\frac{a}{b}$,

- The **numerator** is the term above the line, a.
- The **denominator** is the term below the line, b.
- The **reciprocal** is the fraction you get from flipping the numerator and denominator, $\frac{b}{a}$.
### Operations

#### Multiplication

To multiply two fractions, multiply their numerators and denominators separately.

Product of fractions: $\frac{a}{b}.\frac{c}{d}=\frac{a.c}{b.d}$
#### Division

Dividing by a fraction is equivalent to multiplying by its reciprocal.

To divide by a fraction, flip the numerator and denominator of that fraction, and then multiply.

Quotient of fractions: $\frac{a}{b}\div\frac{c}{d}=\frac{a}{b}.\frac{d}{c}=\frac{a.d}{b.c}$
#### Addition and Subtraction

Unlike multiplying or dividing fractions, in order to add or subtract fractions, we usually have to do an extra step first.

First, write the fractions with a common denominator. Then, add/subtract the numerators of the fractions only, and write the result over the same common denominator.

Sum of fractions: $\frac{a}{b}+\frac{c}{d}=\frac{a.d}{b.d}+\frac{c.b}{d.b}=\frac{a.d+c.b}{b.d}$
Difference of fractions: $\frac{a}{b}-\frac{c}{d}=\frac{a.d}{b.d}-\frac{c.b}{d.b}=\frac{a.d-c.b}{b.d}$
### Working with Fractions and Whole Numbers

Sometimes, you may have to do operations that combine a fraction and a whole number. To do this, you can write the whole number as a fraction with the denominator equal to 1:

$a = \frac{a}{1}$, equals, start fraction, a, divided by, 1, end fraction for any real number a.

Then apply the rules for the operations above.
# Linear equations
## Linear equations in one variable
Equality is a balancing act. It's like a scale where whatever happens on one side, has to happen on the other as well
### Properties of equality
- **Symmetric**: Meaning that it can be flipped, so a = b can be written as b = a
- **Reflexive**: That really just means that the variable is equal to itself, so a = a as long as its a real number
- **Transitive**: Means that we can infer information from the equality, so if a = b and b = c, that means a = c

As an example
$3x + x = \sqrt{10}$ so $4x = \sqrt{10}$, meaning that $x = \frac{\sqrt10}{4}$

### Absolute value
The easiest way to think about that is the distance of that number from 0. Taking 5 as an example, whether it's 5 or -5 the distance of a 5 from 0 is 5. So the absolute value is a distance

### Definitions
An **equation** is any mathematical expression that contains an equals sign (=).

Equations can involve numbers as well as variables.

A **coefficient** is a number that is multiplied by a variable.

For example, 4 is the coefficient of $a^2$ in $4a^2 - 1 = 0$.

The **solutions** to an equation are the values that can be substituted for variables to make the equation true.

For example, consider the equation $\frac{x}{3}=4$. Since we know $\frac{12}{3}=4$ is a true equation, we know that the equation $\frac{x}{3}=4$. has the solution $x=12$.

The **degree** of an equation is the highest power of a variable that appears in the equation.

For example, the equation $4a^2-1=a$ has degree 2, since the highest power that appears is $a^2$. The equations $\frac{x}{3}=4$ and $6x+5y=10$ both have degree 1.

A **linear equation** is an equation with degree 1.

$\frac{x}{3}=4$ is a linear equation with one variable. $6x+5y=10$ is a linear equation with two variables.
### Properties of Equality

Much like addition and multiplication, equality has its own properties. These properties will be important for solving equations.

In the following table, a, b, and c are any real numbers.

| Property       | Rule                                                   | Explanation                                                                                                                                                |
| -------------- | ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Symmetric      | If a = b then b = a                                    | If two things are equal, it does not matter which is written on the left-hand side (LHS) or right-hand side (RHS) of the equals sign.                      |
| Transitive     | If a = b and b = c, then a = c                         | If two values are equal, and one is also equal to a third value, then all three of the values are equal. We can write these equalities in any combination. |
| Addition       | If a = b, then a + c = b                               | If we add the same number to both sides of an equation, then the resulting equation is true.                                                               |
| Subtraction    | If a = b, then a - c = b -c                            | If we subtract the same number from both sides of an equation, then the resulting equation is true.                                                        |
| Multiplication | If a = b, then a. c = b . c                            | If we multiply both sides of an equation by the same number, then the resulting equation is true.                                                          |
| Division       | If a = b, then $a \div c = b \div c$ (when $c \neq 0$) | If we divide both sides of an equation by the same number, then the resulting equation is true.                                                            |

Pay special attention to the properties of Addition, Subtraction, Multiplication, and Division. As a group, these properties tell us that as long as we apply the same operation to **both sides** of an equation, the equation remains true.
### Solving a linear equation with one variable

We can follow this general process to find the solution to a linear equation in one variable.

1. Simplify both sides of the equation, using the order of operations.
2. Move all constant terms to one side of the equation. (use the Addition/Subtraction properties of equality.)
3. Move all variable terms to the other side of the equation. (Use the Addition/Subtraction properties of equality.)
4. Divide both sides by the coefficient of the variable. (Use the Multiplication/Division properties of equality.)
## Linear equations in two variables
- The Cartesian plane is the famous (x-y plane) we draw all of our graphs on
- It's made up of 4 quadrants
- It has a very special coordinate (0,0) which is the origin

y = -2 is just a line intercepting the y axis at -2, and is parallel to the x axis

y = 3x -1 is a line with a positive slope = 3 but then it has a -1 y intercept
To know where it intercepts the x we set the y value to 0, so 0 = 3x -1, in which case that's 3x = 1 or x = 1/3
We got the y intercept in the same way, by setting x to 0, which is why y = -1

$y = \frac{1}{20}x$ has no y intercept so it passes by the origin
That's because if we set y to 0 then $0 = \frac{1}{20}x$ means x = 0

### Linear equations
$y = mx + b$ where
- m = slope
- b = y-intercept
This equation is known as the point-slope equation of a line because it shows us both the intercept and slope
#### Standard form of a line
While $y = mx + b$ is a correct line equation, it's not the most accurate

Consider the following image
![[standard_form.png|204]]
In this image, there is no slope, making the line equation we're familiar with non-compatible

There is actually another form of the equation that's the most generic one $Ax+By+c=0$ which is the correct one here
The point-slope equation looks like this $y - y_1 = m (x - x_1)$, and is an easy way to find the line equation if you know at least 1 point, as well as the slope
This form is called the standard form

The slope intercept from or point-slope is derived from this previous equation $y = -\frac{A}{B}x-\frac{C}{B}$

The slope of a line is equal to the rise over the run, or more formally, the change in y over the change in x $\frac{\Delta y}{\Delta x}$ meaning that $m = \frac{y2-y1}{x2-x1}$

Parallel lines obviously have the same slope, while for perpendicular lines, if line 1 has slope m, line 2 will have slope $-\frac{1}{m}$
