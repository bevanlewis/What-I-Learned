# JavaScript variables and data types

Variables are containers for storing data values.

```javascript
let name = "Alice"; // string
const age = 30; // number
var isStudent = true; // boolean
let emptyValue = null; // null
let notDefined; // undefined
```

How to check the type of a variable ?

use `typeof` operator

```javascript
console.log(typeof name); // "string"
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
| `var`   | Function-scoped, avoid in modern JavaScript       |
| `const` | Block-scoped, cannot be reassigned after creation |
