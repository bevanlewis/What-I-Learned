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

| Order | Operation                                   |
| ----- | ------------------------------------------- |
| 1     | Brackets                                    |
| 2     | Orders (exponents or roots)                 |
| 3     | Division and Multiplication (left to right) |
| 4     | Addition and Subtraction (left to right)    |

The order of operations in mathematics is often remembered by the acronym BODMAS

## Assignment variables

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
