# JavaScript Math

Here's a code block demonstrating some fundamental JavaScript math concepts:

```javascript
// Math Operations
let a = 5;
let b = 3;

console.log(a + b); // Addition: 8
console.log(a - b); // Subtraction: 2
console.log(a * b); // Multiplication: 15
console.log(a / b); // Division: 1.6666666666666667
console.log(a % b); // Modulus: 2
console.log(a ** b); // Exponentiation: 125

let c = 10;
c += 5; // Compound assignment: c is now 15
console.log(c);

console.log(Math.sqrt(16)); // Square root: 4
console.log(Math.abs(-7)); // Absolute value: 7
console.log(Math.round(3.7)); // Rounding: 4

// Not A number (NaN)
0 / 0; // NaN
typeof NaN; // "number"
```

## Order of Operations

The order of operations in JavaScript is the same as in mathematics.

| Order | Operation                                   |
| ----- | ------------------------------------------- |
| 1     | Brackets                                    |
| 2     | Orders (exponents or roots)                 |
| 3     | Division and Multiplication (left to right) |
| 4     | Addition and Subtraction (left to right)    |

The order of operations in mathematics is often remembered by the acronym BODMAS

## Assignment variables

Assignment operators are used to assign values to variables.

```javascript
// Normal assignment
let x = 10;

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

// Exponentiation assignment
x **= 3; // x is now 8

console.log(x); // Output: 8
```

## Increment and Decrement operators

Increment and decrement operators are used to increase or decrease the value of a variable by 1.

```javascript
// Increment operators
let a = 5;
let b = 5;

console.log(a++); // Outputs 5, then increments a to 6
console.log(a); // Outputs 6

console.log(++b); // Increments b to 6, then outputs 6
console.log(b); // Outputs 6

// Decrement operators
let c = 5;
let d = 5;

console.log(c--); // Outputs 5, then decrements c to 4
console.log(c); // Outputs 4

console.log(--d); // Decrements d to 4, then outputs 4
console.log(d); // Outputs 4
```

## Math Object

The Math object in JavaScript provides a set of methods and properties for performing mathematical operations. Here are some examples of how to use the Math object:

```javascript
console.log(Math.PI); // Outputs the value of PI
console.log(Math.sqrt(16)); // Outputs the square root of 16
console.log(Math.abs(-5)); // Outputs the absolute value of -5
console.log(Math.round(3.7)); // Outputs the rounded value of 3.7
console.log(Math.ceil(3.2)); // Outputs the smallest integer greater than or equal to 3.2
console.log(Math.floor(3.8)); // Outputs the largest integer less than or equal to 3.8
console.log(Math.pow(2, 3)); // Outputs 2 raised to the power of 3
console.log(Math.random()); // Outputs a random number between 0 and 1
```
