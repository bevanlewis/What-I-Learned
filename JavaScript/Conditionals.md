# JavaScript Conditionals

Conditionals are used to execute different blocks of code based on certain conditions.

Also check [Booleans](Booleans.md)

## if Statement

The `if` statement is used to execute a block of code if a condition is true.

```javascript
let age = 18;
if (age >= 18) {
  console.log("You are an adult.");
}
```

## if...else Statement

The `if...else` statement is used to execute one block of code if a condition is true and another block of code if the condition is false.

```javascript
let age = 16;
if (age >= 18) {
  console.log("You are an adult.");
} else {
  console.log("You are a minor.");
}
```

## if...else if...else Statement

The `if...else if...else` statement is used to execute one block of code if a condition is true, another block of code if another condition is true, and another block of code if none of the conditions are true.

```javascript
let age = 16;
if (age >= 18) {
  console.log("You are an adult.");
} else if (age >= 13) {
  console.log("You are a teenager.");
} else {
  console.log("You are a child.");
}
```

## Switch Statement

The `switch` statement is used to execute one block of code for each possible value of a variable.

```javascript
let day = "Monday";
switch (day) {
  case "Monday":
    console.log("It's the start of the week.");
    break;
  case "Tuesday":
  case "Wednesday":
  case "Thursday":
    console.log("It's a workday.");
    break;
  case "Friday":
    console.log("TGIF!");
    break;
  case "Saturday":
  case "Sunday":
    console.log("It's the weekend!");
    break;
  default:
    console.log("Invalid day.");
}
```

## Ternary Operator

The ternary operator is a shorthand way of writing an `if...else` statement.

```javascript
let age = 18;
let message = age >= 18 ? "You are an adult." : "You are a minor.";
```
