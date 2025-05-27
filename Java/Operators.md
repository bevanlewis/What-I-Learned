# Java Operators

Here's a code block demonstrating some fundamental Java operators:

```java
// Math Operations
int a = 5;
int b = 3;

System.out.println(a + b); // Addition: 8
System.out.println(a - b); // Subtraction: 2
System.out.println(a * b); // Multiplication: 15
System.out.println(a / b); // Division: 1
System.out.println(a % b); // Modulus: 2

int c = 10;
c += 5; // Compound assignment: c is now 15
System.out.println(c);

System.out.println(Math.sqrt(16)); // Square root: 4.0
System.out.println(Math.abs(-7)); // Absolute value: 7
System.out.println(Math.round(3.7)); // Rounding: 4

// Not A Number (NaN) is handled differently in Java
double nanValue = 0.0 / 0.0;
System.out.println(nanValue); // NaN
System.out.println(Double.isNaN(nanValue)); // true
```

## Order of Operations

The order of operations in Java is the same as in mathematics.

| Order | Operation                                   |
| ----- | ------------------------------------------- |
| 1     | Parentheses                                 |
| 2     | Exponents                                   |
| 3     | Multiplication and Division (left to right) |
| 4     | Addition and Subtraction (left to right)    |

The order of operations in mathematics is often remembered by the acronym PEMDAS.

## Assignment Operators

Assignment operators are used to assign values to variables.

```java
// Normal assignment
int x = 10;

// Addition assignment
x += 5; // x is now 15

// Subtraction assignment
x -= 3; // x is now 12

// Multiplication assignment
x *= 2; // x is now 24

// Division assignment
x /= 4; // x is now 6

// Modulus assignment
x %= 4; // x is now 2

// Exponentiation assignment is not directly available in Java
x = (int) Math.pow(x, 3); // x is now 8

System.out.println(x); // Output: 8
```

## Increment and Decrement Operators

Increment and decrement operators are used to increase or decrease the value of a variable by 1.

```java
// Increment operators
int a = 5;
int b = 5;

System.out.println(a++); // Outputs 5, then increments a to 6
System.out.println(a); // Outputs 6

System.out.println(++b); // Increments b to 6, then outputs 6
System.out.println(b); // Outputs 6

// Decrement operators
int c = 5;
int d = 5;

System.out.println(c--); // Outputs 5, then decrements c to 4
System.out.println(c); // Outputs 4

System.out.println(--d); // Decrements d to 4, then outputs 4
System.out.println(d); // Outputs 4
```

## Logical Operators

Logical operators are used to perform logical operations on boolean values.

```java
boolean x = true;
boolean y = false;

System.out.println(x && y); // Logical AND: false
System.out.println(x || y); // Logical OR: true
System.out.println(!x); // Logical NOT: false
System.out.println(x ^ y); // Logical XOR: true
```

## Math Class

The Math class in Java provides a set of methods and properties for performing mathematical operations. Here are some examples of how to use the Math class:

```java
System.out.println(Math.PI); // Outputs the value of PI
System.out.println(Math.sqrt(16)); // Outputs the square root of 16
System.out.println(Math.abs(-5)); // Outputs the absolute value of -5
System.out.println(Math.round(3.7)); // Outputs the rounded value of 3.7
System.out.println(Math.ceil(3.2)); // Outputs the smallest integer greater than or equal to 3.2
System.out.println(Math.floor(3.8)); // Outputs the largest integer less than or equal to 3.8
System.out.println(Math.pow(2, 3)); // Outputs 2 raised to the power of 3
System.out.println(Math.random()); // Outputs a random number between 0 and 1
```
