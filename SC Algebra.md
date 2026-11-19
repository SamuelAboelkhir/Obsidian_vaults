---
tags:
- Math
MOC: Sciences
Course: "https://www.coursera.org/specializations/algebra-elementary-to-advanced"
---
[[_0000 Home|Home]] | [[_0004 Sciences MOC|Back to Sciences MOC]] | [[SC Mathematics index|Back to index]]
# The structure of numbers
**NOTE**: If you see repeated explanations for the same things, that's because you decided mid way to start copying the actual course notes from coursera, but didn't want to remove the notes you took yourself at first. The way this works now, is the coursera notes act as the base, then you decided to supplement them whenever they didn't feel sufficient
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
- **Note:** Sometimes, the natural numbers N are defined to include 0 as well as the positive counting numbers. Beginning with the count 0 rather than 1 is common especially in computer science. Since the definition is not unanimous, it is common in math to specify whether or not 0 is considered a natural number in context. _For the purposes of this course,_ 0 _is not included in the natural numbers_ N.

![[Pasted image 20261101004454.png|457]]

The set of real numbers contains both the rational numbers and the irrational numbers. Every real number is either rational or irrational.

The set of rational numbers contains the set of integers, which in turn contains the set of whole numbers, which contains the set of natural numbers..

This means:

- All natural numbers are also whole numbers, integers, rational numbers, and real numbers.
- All whole numbers are also integers, rational numbers, and real numbers.
- All integers are also rational numbers and real numbers.
- All rational numbers are real numbers.
- All irrational numbers are real numbers.
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
# Closure of Real Numbers - Course notes

**Closure Property (Addition and Multiplication).** If $a,b ∈ R$, then $(a+b)∈R$.

That is , real numbers are _closed_ under addition and multiplication.

It also follows that real numbers are closed under subtraction and division. Not that closure under division holds for all real numbers except 0, since division by 0 is not defined.

## Proof of closure under subtraction

If $a,b∈R$, then $a−b=a+(−b)$.

−b is also a real number, and real numbers are closed under addition.

Therefore, $(a−b)∈R$.

## Proof of closure under division

If $a,b∈R$ and $b≠0$, then $a÷b=a⋅\frac{1}{b}$

The reciprocal $\frac{1}{b} is also a real number, and real numbers are closed under multiplication.

Therefore, $(a÷b)∈R$.

## Properties of Addition and Multiplication

Addition and multiplication satisfy the following properties. In the table below, a, b, and c are all real numbers.

| Property                               | Equation          | Description                                                                                                                  |
| -------------------------------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Commutative Property of Addition       | a+b=b+a           | Changing the order in which numbers are added does not change their sum.                                                     |
| Commutative Property of Multiplication | a⋅b=b⋅a           | Changing the order in which numbers are multiplied does not change their product.                                            |
| Associative Property of Addition       | (a+b)+c = a+(b+c) | When three or more numbers are added, changing the grouping or placement of parentheses does not change the sum.             |
| Associative Property of Multiplication | (a⋅b)⋅c = a⋅(b⋅c) | When three or more numbers are multiplied, changing the grouping or placement of parentheses does not change the product.    |
| Distributive Property                  | a⋅(b+c) = a⋅b+a⋅c | Multiplying a number by a sum gives the same result as multiplying that number by each of the addends and then summing them. |

## The Order of Operations

The **order of operations** is the sequence in which an expression should be evaluated.

1. **Parentheses/Brackets:** Perform operations written inside parentheses/brackets first.
2. **Exponents/Orders:** Calculate powers and roots next.
3. **Multiplication and Division:** Next, multiply and/or divide, going left to right.
4. **Addition and Subtraction:** Finally, add and/or subtract, going left to right.

**Common acronyms:** PEMDAS, BODMAS, BEDMAS
# Linear equations
- Linear means of the first degree, or of power 1
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
$\frac{12}{3}​=4$, $\frac{x}{3}=4$, $4a^2−1=0$, and $6x+5y=10$ are all examples of equations.
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
While $y = mx + b$ (the slope-intercept form) is a correct line equation, it's not the most accurate

Consider the following image
![[standard_form.png|204]]
In this image, there is no slope, making the line equation we're familiar with non-compatible

There is actually another form of the equation that's the most generic one $Ax+By+c=0$ which is the correct one here
The point-slope equation looks like this $y - y_1 = m (x - x_1)$, and is an easy way to find the line equation if you know at least 1 point, as well as the slope
This form is called the standard form

The slope-intercept from or point-slope is derived from this previous equation $y = -\frac{A}{B}x-\frac{C}{B}$

The slope of a line is equal to the rise over the run, or more formally, the change in y over the change in x $\frac{\Delta y}{\Delta x}$ meaning that $m = \frac{y2-y1}{x2-x1}$

Parallel lines obviously have the same slope, while for perpendicular lines, if line 1 has slope m, line 2 will have slope $-\frac{1}{m}$
## Lines in the Plane

Recall that a **linear equation** is an equation with degree one. In this section, we will work with **linear equations in two variables**.

For example, $x+2y=4$ is a linear equation in two variables.

Any two letters may be used as variables. The variables x and y are the most common.

The x, y **-plane** consists of the x **-axis** (the **horizontal axis**) and the y **-axis** (the **vertical axis**). The x and y axes intersect at the **origin**, (0,0).

The graph of a linear equation in two variables in the plane is a **line**.

For example, the linear equation $x+2y=4$ is graphed below.
![[Pasted image 20261111002814.png|306]]
The x **-intercept** of a line is the point where the line crosses the x-axis. The x-intercept always has the form (a, 0) for some number x. Thus we can either specify the x-intercept by its coordinates (a,0), or say that the x-intercept is at x = a.

The **slope** of a line describes how steep the line is. The bigger the slope, the steeper the line.

Lines with a positive slope slant upwards, and lines with a negative slope slant downwards (considered left-to-right).

A **horizontal line** has slope 0. The slope of a **vertical line** is undefined.
![[Pasted image 20261111003312.png]]
### Calculating Slope

If you know the coordinates of two points on a line, then you can find its slope.

The slope of the line that contains the points (x1,y1) and (x2​,y2​) is given by the formula

$m=\frac{y_2-y_1}{x_2-x_1}$

The slope is equal to the change in y-coordinates (the vertical change) over the change in x-coordinates (the horizontal change). For this reason, the slope formula is sometimes summarized as

$slope=\frac{rise}{run}$

Finally, note that when you are computing the slope, it does not matter which of the two points you designate as (x1,y1) or (x2,y2). It only matters that the coordinates of the corresponding points are in the _same position_ in the numerator and denominator in the formula.

For example, we can use the points (0,2) and (4,0) on the line $x+2y=4$ graphed above to calculate its slope.

If we let $(x1,y1)=(0,2)$ and $(x2,y2)=(4,0)$, we get $m=\frac{y2−y1}{x2−x1}=\frac{0−2}{4−0}=\frac{−2}{4}=−\frac{1}{2}$.

If we were to let $(x1,y1)=(4,0)$ and $(x2,y2)=(0,2)$, we would get the same answer: 
$m=\frac{2−0}{0−4}=\frac{2}{−4}=−\frac{1}{2}$.

If we accidentally switched the order in the numerator without switching the order in the denominator, we would get a slope with the wrong sign. For this reason, it is important to remember what a line with positive slope looks like and what a line with negative slope looks like. That way, you can see if the answer you get from the slope formula fits the line, or if you made an error.

### Parallel and Perpendicular Lines

**Parallel lines** are lines that never intersect. Parallel lines have the same slope.

A line that is parallel to a line of slope mmm has slope mmm as well.

**Perpendicular lines** are lines that intersect at a right angle (90∘90∘90, degrees).

A line that is perpendicular to a line of slope mmm has slope −1m−m1​minus, start fraction, 1, divided by, m, end fraction.

## Forms for Equations of Lines

There are three main forms used to write two variable linear equations: **standard form**, **slope-intercept form** and **point-slope form**.

You can use any form to describe a line. Depending on the information you have about a line, one form may be quicker to write than the other.

### Standard Form

$Ax+By=C$

$A,B,C$ are constants. Standard form is most commonly seen in systems of linear equations, which we study in Module 4. It is less popular in the context of graphing. To get more information about the line to graph it, convert from standard form to slope-intercept form.

### Slope-Intercept Form

$y=mx+b$
m is the slope of the line.
$(0,b)$ y-intercept of the line.

### Point-Slope Form

$y−y_0=m(x−x_0)$
m is the slope of the line.
$(x0,y0)$ are the coordinates of any chosen point on the line.
### Relationship of Slope-Intercept and Point-Slope Form

What happens if we write the point-slope form equation of a line using the y-intercept (0,b) as our chosen point?

Letting m be the slope and (x0,y0)=(0,b), we get

$y-y_0=m(x-x_0)$
$y-b=m(x-0)$
$y-b=mx$
$y=mx+b$

which is the slope-intercept form.

Thus, we can think of the slope-intercept form as a specific version of the point-slope form.
# Linear inequalities in one variable
## Notations
### Inequality symbols

There are four different types of inequality. Two (> and <) are _strict inequalities_, which means they do not include the possibility of equality. In _nonstrict inequalities_ (≥ and ≤), the two quantities that are being compared may be equal.

| Symbol | Meaning                  |
| ------ | ------------------------ |
| >      | (strictly) greater than  |
| <      | (strictly) less than     |
| ≥      | greater than or equal to |
| ≤      | less than or equal to    |
### Interval Notation

**Intervals** are subsets of the real numbers that consist of all real numbers that between two fixed values, called the **endpoints**.

Intervals may or may not include their endpoints. If an endpoint is not included in an interval, it is denoted with a parenthesis, ( or ). If an endpoint is included in an interval, it is denoted with a square bracket, [or].

Intervals that contain neither endpoint are called **open**. Intervals that contain both endpoints are called **closed**.

| Type of Interval | Interval Notation | Inequality Description                   |
| ---------------- | ----------------- | ---------------------------------------- |
| Open             | (a,b)             | All real numbers xxx such that a < x < b |
| Closed           | [a,b]             | All real numbers x such that a ≤ x ≤ b   |
| Half-open        | (a,b]             | All real numbers xxx such that a < x ≤ b |
| Half-open        | [a,b)             | All real numbers x such that a ≤ x < b   |
#### **Special Intervals**
- (−∞,∞) includes **all real numbers**, R.
- (0,∞) includes all **positive numbers**.
- (−∞,0) includes all **negative numbers**.

Remember that ∞ and −∞ are _symbols, not numbers_. As such, they cannot be included as endpoints of an interval. When you write ∞ or −∞ in an interval, it _always goes with an open parenthesis_, ( or )
## Properties of Inequality

The following properties are important for working with inequalities.

The list below uses strict inequality, but can also be applied to nonstrict inequalities.

Suppose a, b, and c are all real numbers.
### Transitive Property of Inequality

If a < b and b < c, is less than, c, then a < c.

In other words, we can combine two inequalities.
### Addition and Subtraction

If a < b, then

- a + c < b + c
- a − c < b − c

Just like with equations, we can add or subtract the same number to both sides of an inequality, and the inequality remains true.

### Multiplication and Division

The rules for multiplication and division in an inequality change, depending on whether that number is positive or negative.

#### **Multiplying/Dividing by a Positive Number**

If a < b and c > 0>, then

- ca < cb
- a/c < b/c

In other words, if c is positive, then we can multiply or divide both sides of an inequality by c, and the inequality remains true.

#### **Multiplying/Dividing by a Negative Number**

If a < b and c < 0, then

- ca > cb
- a/c > b/c 

In other words, if c is negative, then when we multiply or divide both sides of an inequality by c, we switch the direction of the inequality sign.

### Reciprocals

If a and b have the same sign and a < b, then $\frac{1}{a}>\frac{1}{b}$​.

Taking the reciprocal of both sides of an inequality also changes the direction of the inequality sign.

## Solving Inequalities

Just as when we are solving equations, our goal is to isolate the variable. We follow the same basic steps as we do when solving equations:

1. Simplify both sides of the inequality, using the order of operations.
2. Move all constant terms to one side of the inequality. (use the Addition/Subtraction properties of inequality.)
3. Move all variable terms to the other side of the inequality. (Use the Addition/Subtraction properties of inequality.)
4. Divide both sides by the coefficient of the variable. (Use the Multiplication/Division properties of inequality.)

In the last step, some extra caution is needed. When we are working with inequalities as opposed to equations, _multiplying or dividing by a negative number changes the direction of the inequality_.

## Compound Inequalities

Sometimes, we want to solve inequalities where x is between two different numbers, for example −1 < 2x + 5 < 7.

In this case, we follow the same steps as above. The only difference is that there is no longer a left-hand side and right-hand side. Instead, we apply all the same operations to all _three parts_ of the inequality (reversing the direction when necessary), and isolate the variable in the middle of the inequality, with constants on the left and right.

## Absolute value inequalities

One special case where you will encounter compound inequalities or multiple inequalities is in absolute value inequalities.

Recall that an absolute value equation generates two different equations. ∣x∣ = a when x = a or x = −a.

Similarly, absolute value inequalities generate two different inequalities.

The form of these inequalities differs, depending on whether the absolute value is less than or greater than the given number.

The table below lists the alternate forms (removing the absolute value signs) for absolute value inequalities.

| Absolute value inequality | Alternate form  |
| ------------------------- | --------------- |
| ∣x∣ < a                   | −a < x < a      |
| ∣x∣ > a                   | x < −a or x > a |
| ∣x∣ ≤ a                   | −a ≤ x ≤ a      |
| ∣x∣ ≥ a                   | x ≤ −a or x ≥ a |
As an example of intervals, consider (x-1)(x-3) > 0, the usual approach is FOIL (double distribution) basically taking the first factor of the first binomial (a polynomial expression composed of 2 terms) and multiply it by the two factors of the 2nd binomial, then do the same for the 2nd factor of the first binomial, and combine them. That gives us $x * x - x * 3 -1 * x -1 * -3 > 0$ or $x^2 -3x -x + 3 > 0$ or $x^2 -4x +3 > 0$

However, a better approach, is to think of this as an equality temporarily, even though its an inequality, $(x-1) (x-3) = 0$ as this allows us to solve for x as such $x=1, x=3$. Being equal to 0 means that one of the two binomials must be equal to 0, giving us a multiplication by 0, hence $x=1$ or $x=3$ as our possibilities

We then put both possibilities on a **sign graph** ![[Pasted image 20261111231721.png]] which lets us partition the real number line into segments
We can then pick any number we want as a test point. For example, 0, 2, or 4. Then we test $(0 - 1)(0 - 3) > 0?$ yes, so we try again $(2 - 1)(2 - 3) > 0?$ nope, and finally $(4 - 1)(4 - 3) > 0?$ yep, so for our interval, we know that when x was 0 it passed, and when it was 4 it passed, so the answer is the combination of the two test points where the inequality was satisfied, which is everything to the left of 1 starting from 0, and everything to the right of 3 starting from from 4, which is $(-∞,1)$ or $(3, ∞)$ where a parenthesis means starting from, or going towards, but not including. We can also say it's the union of both sets $(-∞,1) ∪ (3, ∞)$ 

For $(x-1)(x-3)(5-x) \leq 0$, we get x=1, x=3, and x=5, so the in real numbers would be 0, 2, 4, and 6. Then we test $(0-1)(0-3)(5-0) \leq 0?$ no, $(2-1)(2-3)(5-2) \leq 0?$ yes, $(4-1)(4-3)(5-4) \leq 0?$ no, $(6-1)(6-3)(5-6) \leq 0$ yes. So it's a yes for 2 and 6, and also 1, 3, and 5 actually would result in exactly 0 meaning $[1, 3]∪[5, ∞)$

Again, to clarify, what we're doing with these values of x that we're plugging into the equation, is that we're trying to find out if we want the whole interval that contains that number. That's why, when 2 worked, we knew that we wanted the interval from 1 to 3, and it's inclusive because they too satisfied the inequality. And likewise when the 6 passed, it started at a partitioning point on the sign graph, then went off to infinity, the interval was 5 to infinity, and 5 satisfied the inequality too, that's why we got $[5, ∞)$
# Linear Inequalities in Two Variables
In the previous section, we studied linear inequalities in one variable. The solutions to such inequalities were sets of real numbers, which can be graphed on the real number line.

Now, we investigate **linear inequalities in two variables**: inequalities of the form ax + by > c, ax + by < c, and the non-strict versions of these.

For example, 2y − x > 6 in two variables.

The **solution** to a linear inequality in two variables is a set of ordered pairs. A point (x,y) is part of the solution if plugging in both values of the two variables at the same time results in a true inequality.

For example, the point (x,y) = (0,4) is part of the solution to the inequality 2y − x > 6, because plugging in x = 0 and y = 4 results in

2y − x > 6
2(4) − 0 > 6
      8 > 6

which is a true inequality.

The solution to a linear inequality in two variables can be graphed in the x, y-plane.

## Graphing the solution of a two-variable linear inequality

If we change the inequality sign to an equals sign, we get a linear equation in two variables. The graph of this equation is a line, which divides the plane in half. One half of the plane is the solution to the linear inequality. The line itself may or may not be included in the solution.

To graph the solution, we can use the following procedure:

1. Graph the corresponding line (the line obtained by changing the inequality sign to an equals sign).
2. Determine whether or not the line is included in the solution: If it is a nonstrict inequality (≤ or ≥), then the line is included, and drawn as a **solid line**. If it is a strict inequality (< or >), then the line is not included, and drawn as a **dotted line**.
3. Test a sample point that is on one side of the line to determine whether it is part of the solution. Try to choose a point that is easy to make calculations with, such as (x,y)=(0,0).
4. If the sample point is part of the solution, then shade that side of the line. If it is not part of the solution, then shade the other side of the line.

For example, consider again the linear inequality 2y − x > 6.

The corresponding linear equality is 2y − x = 6. If we add x to both sides of the equation, then divide both sides by 2, to rewrite the equation in slope-intercept form $y=\frac{1}{2}x+3$. The line has slope $\frac{1}{2}$ and y-intercept (0,3). Since the inequality is strict, we graph this line as a dotted line.

Before, we confirmed that (x,y) = (0,4) is part of the solution to this linear inequality. Therefore, we shade the half of the plane that contains the point (0,4).

![[Pasted image 20261114123903.png|444]]

You can enter two-variable linear inequalities on [Desmos.com](https://www.desmos.com/calculator "Desmos") to generate the graphs of their solutions.
# Systems of Linear Equations
## Definitions

In latex, a systems of equation syntax can be written as
  - `$\left\{\begin{aligned}x-y &= 0 \\ x+2y &= 3\end{aligned}\right.$`
  - or `$​\begin{cases}x - y &= 0 \\ x + 2y &= 3\end{cases}$`

The first style with `aligned` is known to be more generic and more appropriate for a system of equations, but it doesn't render quite right, while the `cases` syntax is meant for piecewise definitions, whatever that means. It renders better though

A **system of linear equations** (in two variables)is a group of two or more linear equations involving the same variables.

For example,

$​\begin{cases}x - y &= 0 \\ x + 2y &= 3\end{cases}$

is a system of two linear equations of two variables.

A **solution** to a system of linear equations is a set of values for the variables (x,y) that satisfies all of the equations at the same time.

For example, (1,1) is a solution to the system of linear equations given above, because plugging x = 1, into both x − y = 0 and x + 2y = 3, results in a true equation.

On the other hand (0,0) is not a solution to the system of linear equations given above. Even though plugging x = 0, and y = 0 into the first equation x − y = 0 gives a true equation, plugging x = 0 and y = 0 into the second equation x + 2y = 3 results in a false equation. A solution a system must satisfy all of the equations in the system.

## Solving a System of Equations by Graphing

Remember that a linear equation of two variables is graphed as a straight line in the plane. Thus, a system of linear equations is graphed as two or more straight lines in the same coordinate plane.

Graphically, the solution to a system of linear equations is _any point that is shared by all of the lines in the system_.

**To solve a system of linear equations by graphing:**

1. Graph all of the lines given by the equations in the system in the same coordinate plane.
2. Find any point where **all** of the lines intersect.
3. The coordinates of any intersection point are a solution to the system of equations.

For example, the system of equations

$​\begin{cases}x - y &= 0 \\ x + 2y &= 3\end{cases}$

consists of two lines, graphed below.

The two lines intersect once, at the point (1,1). Thus, (1,1) is the one and only solution to the system.

![[Pasted image 20261114215011.png|380]]
### How many solutions?

For a system of two linear equations, there are three possibilities. First, the two equations might not describe distinct lines.

- **The lines are the same.** It is possible that the two linear equations in a system both describe the same exact line. In that case, every point on this line satisfies both equations, so we say that the system has **infinitely many solutions.**

Otherwise, for two distinct straight lines, there are two possibilities.

- **The lines are parallel.** Parallel lines never intersect. Therefore, a system of linear equations consisting of parallel lines has **no solutions.**
- **The lines are not parallel.** As long as two lines are not parallel, they intersect at some point. A system of two linear equations with two non-parallel lines has exactly **one solution.**

![[Pasted image 20261114221014.png|823]]

Some systems of equations have more than two equations.

A point is a solution to a system of linear equations if and only if **all** of the lines in the system pass through it.

Therefore, it is possible to have a system of three linear equations in which none of the lines are parallel, yet there is no solution.

![[Pasted image 20261114221030.png|556]]

## Solving a System of Equations by Substitution

One of the options to solve a system of equations without graphing is the **method of substitution**.

1. Use one equation to write y in terms of x.
2. Substitute this expression for y in the first equation. The result is an equation of xxx only.
3. Solve for x in the resulting equation.
4. Plug in this value of xxx to find the corresponding value of y.

(Note: remember that variables can be named any letter. You can also follow the same procedure switching y and x, or with any other letters.)

For example, let's return to the system

$​\begin{cases}x - y &= 0 \\ x + 2y &= 3\end{cases}$

We follow the steps of the method of substitution to solve this system.

- Consider the first equation. Add y to both sides of the equation to get x = y.
- Substitute for y in the second equation. $x+2y=3⇒x+2(x)=3⇒3x=3$
- Solve for x. Divide both sides by 3 to get x = 1.
- Plug in to find the corresponding value of y. We have y = x = 1.

Thus we obtain the solution we found earlier: (x,y) = (1,1).

## Solving a System of Equations by Elimination

Another option to solve a system without graphing is the **method of elimination**.

1. Multiply both sides of one equation by a number such that the coefficient of y is the same in both equations.
2. Subtract one equation from the other (subtract the right-hand side from the right-hand side, and the left-hand side from the left-hand side). The result is an equation of x only.
3. Solve for x in the resulting equation.
4. Plug in this value of x to find the corresponding value of y.

(Note: remember that variables can be named any letter. You can also follow the same procedure switching yyy and xxx, or with any other letters.)

For example, let's follow these steps to solve the same system of equations,

$​\begin{cases}x - y &= 0 \\ x + 2y &= 3\end{cases}$

1. If we multiply both sides of the first equation by −2, then the coefficient of y in both equations will be 2. Multiply the left-hand and right-hand sides of the first equation by −2, to obtain −2x + 2y = 0.
2. We subtract the first equation from the second equation, by subtracting the left-hand side of the first equation from the left-hand side of the second and the right-hand side of the first from the right-hand side of the second. $x+2y−(−2x+2y)=3−(0)⇒3x=3$.
3. Solve for xxx in the resulting equation. Divide both sides by 3 to get x = 1.
4. Plug x = 1 into either equation to find the corresponding value of y. In this case, using the first equation looks easier: $x−y=0⇒1−y=0⇒y=1$.

Again, we have gotten the one solution to the system of equations, (x,y) = (1,1).

### How many solutions?

When we solve a system by substitution and elimination, there are the same possibilities for the number of solutions: one, none, or infinitely many.

If the method of substitution or elimination gives us **specific numeric values** for the variables, then we know that the system has this **one solution**.

If the method of substitution or elimination results in an equation that is **always true**, then the system has **infinitely many solutions**.

For example, consider the system

$\begin{cases} x - y &= 0 \\ 2x - 2y &= 0 \end{cases}$

Using the method of substitution, we use the first equation to write x = y. Substituting for y in the second equation, we get

$2x−2y=0⇒2x−2(x)=0⇒0=0$.

The resulting equation is always true, and this system has infinitely many solutions.

Similarly, to use the method of elimination, we could multiply the first equation by 2 so that the coefficients of the variable y are the same in each equation. We would get

$2(x−y)=2(0)⇒2x−2y=0$.

Then, subtracting the first equation from the second gives us

$2x−2y−(2x−2y)=0−(0)⇒0=0$,

an equation that is always true.

If the method or substitution or elimination results in an equation that is **always false**, then the system has **no solutions**.

For example, consider the system

$\begin{cases}x - y &= 0 \\ x - y &= 4\end{cases}$

Using the method of substitution, we use the first equation to write x = y. Substituting for y in the second equation, we get

$x−y=4⇒x−(x)=4⇒0=4$.

The resulting equation is always false, and this system has no solutions.

Similarly, to use the method of elimination, we would subtract the first equation from the second. Doing this gives us

$(x−y)−(x−y)=4−(0)⇒0=4$,

which is always false.
# Systems of Linear Inequalities
## Definitions

The definitions for a system of linear inequalities and its solution are very similar to the definitions for a system of linear equations.

A **system of linear inequalities** (in two variables) is a group of two or more linear inequalities involving the same variables.

For example,

$\begin{cases}x - y &\geq 5 \\ 2x + 3y &< 0\end{cases}$

is a system of two linear inequalities of two variables.

A **solution** to a system of linear inequalities is a set of values for the variables (x,y) that satisfies all of the inequalities at the same time.

For example, (0,−6) is a solution to the system of linear inequalities given above, because plugging x = 0 and y =− 6y into both x − y ≥ 5x and 2x + 3y < 0 results in a true inequality.

On the other hand (6,0) is not a solution to the system of linear inequalities given above. Even though plugging x = 6  and y = 0 into the first inequality x − y ≥ 5 gives a true equation, plugging x = 6 and y = 0 into the second inequality results in 12 < 0, which is false. A point is part of the solution if and only if it satisfies all of the inequalities in the system.

## Solving a System of Inequalities by Graphing

Solving a system of linear inequalities graphically is also similar to solving a system of linear equations graphically. The solution to a system is the intersection of the solutions of the inequalities: the set of points that are shared by the solutions to all of the inequalities.

**To solve a system of two linear inequalities by graphing:**

**1.** Graph the solutions to each of the linear inequalities in the same coordinate plane.

**a.** Graph the boundary line for the first linear inequality and determine whether it is dashed (< or >) or solid (≤ or ≥).

**b.** Test a sample point. If the coordinates of the point satisfy the inequality, shade the plane on the side of the line that contains this point. If not, shade the other side of the plane.

**c.** Repeat the process for the second linear inequality.

**2.** The area that is shared by both of the solutions (the area that has been shaded twice) is the solution to the system.

Unlike a system of linear equations, a system of linear inequalities does not have exactly one solution. It can either have infinitely many solutions, or no solutions.

![[Pasted image 20261115210145.png|580]]

For example, the following system of inequalities is graphed below.

$​\begin{cases}x - y &\geq 5 \\ 2x + 3y &< 0\end{cases}$

The area that is shaded in red is the solution to the inequality x − y ≥ 5. The area that is shaded in blue is the solution to the inequality 2x + 3y < 0.

The solution to the system of inequalities is is the area at the bottom of the picture, that has been shaded in both red and blue.

![[Pasted image 20261115210159.png|277]]

```desmos-graph

```