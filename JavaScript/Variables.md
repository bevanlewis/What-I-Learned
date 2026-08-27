# JavaScript variables and data types

Variables are containers for storing data values.

```javascript
const name = "Alice"; // string
const age = 30; // number
const isStudent = true; // boolean
const emptyValue = null; // null
let notDefined; // undefined
```

Use the `typeof` operator to check a value's primitive type:

```javascript
console.log(typeof name); // "string"
console.log(typeof null); // "object" (a historical JavaScript quirk)
```

## Declaring variables

```javascript
// Variables can be declared with `let` or `const`

let name = "John Doe";
const age = 25;

// Variables can be declared with `var`, but it's not recommended in modern JavaScript
var isStudent = true;

// Example of using let for a variable that might change
let score = 0;

// Example of using const for a variable that should not be reassigned
const PI = 3.14159;

// Example of declaring multiple variables in one line
let x = 5,
  y = 10,
  z = 15;

// Example of using let with an initially undefined value
let futureValue;

// Example of using const with an object (the object's properties can still be modified)
const person = { firstName: "Alice", lastName: "Smith" };
```

| Keyword | Definition/Usage                                  |
| ------- | ------------------------------------------------- |
| `let`   | Block-scoped, can be reassigned                   |
| `var`   | Function-scoped and hoisted; avoid in modern code |
| `const` | Block-scoped, cannot be reassigned after creation |

`const` prevents reassignment of the variable. It does not make an object or array immutable.

## Primitive and reference values

JavaScript has seven primitive types: string, number, bigint, boolean, undefined, symbol, and null. Objects, arrays, and functions are reference values.

```javascript
const original = { count: 1 };
const alias = original;

alias.count = 2;
console.log(original.count); // 2
```

## Converting values

Prefer explicit conversion at system boundaries:

```javascript
const count = Number("42"); // 42
const label = String(42); // "42"
const enabled = Boolean(1); // true

console.log(Number.isNaN(Number("unknown"))); // true
```

`parseInt("12px", 10)` returns `12`, while `Number("12px")` returns `NaN`. Choose based on whether partial parsing is intended.
