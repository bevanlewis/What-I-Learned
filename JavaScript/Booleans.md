# JavaScript Booleans

Booleans represent true or false values.

```javascript
let isTrue = true;
let isFalse = false;
```

## Comparison Operators

Comparison operators are used to compare values and return a boolean value.

| Operator | Description                                 |
| -------- | ------------------------------------------- |
| ==       | Equal to                                    |
| ===      | Equal value and type                        |
| !=       | Not equal                                   |
| !==      | Not equal value or type                     |
| >        | Greater than                                |
| <        | Less than                                   |
| >=       | Greater than or equal to                    |
| <=       | Less than or equal to                       |
| ?        | Ternary operator (condition ? true : false) |

## Logical Operators

Logical operators are used to combine or negate boolean values.

| Operator | Description |
| -------- | ----------- |
| &&       | Logical AND |
| \|\|     | Logical OR  |
| !        | Logical NOT |

### Truth Tables

#### AND (&&)

| A     | B     | Result |
| ----- | ----- | ------ |
| true  | true  | true   |
| true  | false | false  |
| false | true  | false  |
| false | false | false  |

#### OR (||)

| A     | B     | Result |
| ----- | ----- | ------ |
| true  | true  | true   |
| true  | false | true   |
| false | true  | true   |
| false | false | false  |

#### NOT (!)

| A     | Result |
| ----- | ------ |
| true  | false  |
| false | true   |

## Short-Circuit Evaluation

JavaScript evaluates expressions from left to right. If the value of an expression can be determined based on the first operand, the second operand is not evaluated.

```javascript
let a = 0;
let b = 1;
// b is not evaluated because a is truthy
let result = a || b; // result is 0
```

## Truthy and Falsy Values

| Truthy Values | Falsy Values |
| ------------- | ------------ |
| true          | false        |
| 1             | 0            |
| "hello"       | ""           |
| []            | null         |
| {}            | undefined    |
| -1            | NaN          |

```javascript
// A value can be created into it's boolean form using the Boolean() function
let x = 10;
let y = "hello";
let z = [];
console.log(Boolean(x)); // true
console.log(Boolean(y)); // true
console.log(Boolean(z)); // true
```

## Type Coercion

JavaScript automatically converts values to booleans in certain contexts, such as in conditional statements.

```javascript
if (1) {
  console.log("1 is truthy");
}

if ("") {
  console.log("This won't be executed");
}
```

## Boolean in Conditional Statements

Booleans are commonly used in conditional statements to control program flow.

```javascript
let isLoggedIn = true;

if (isLoggedIn) {
  console.log("Welcome back!");
} else {
  console.log("Please log in.");
}
```

## Logical Operators with Non-Boolean Values

Logical operators can also be used with non-boolean values, which can lead to unexpected results if not understood properly.

```javascript
console.log("hello" && "world"); // "world"
console.log("" || "default"); // "default"
console.log(null && "value"); // null
```
