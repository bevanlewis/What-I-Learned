# JavaScript Loops

Loops are used to execute a block of code repeatedly. JavaScript provides several types of loops:

## For Loop

The for loop is used to execute a block of code a specified number of times.

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

## While Loop

The while loop is used to execute a block of code repeatedly as long as a condition is true.

```javascript
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}
```

## Do-While Loop

The do-while loop is similar to the while loop, but it executes the block of code at least once before checking the condition.

```javascript
let i = 0;
do {
  console.log(i);
  i++;
} while (i < 5);
```

## Break and Continue

The break statement is used to exit a loop prematurely. The continue statement is used to skip the current iteration of a loop.

## For-In Loop

The for-in loop is used to iterate over the properties of an object.

```javascript
let person = {
  name: "John",
  age: 30,
  city: "New York",
};

for (let key in person) {
  console.log(key + ": " + person[key]);
  // Outputs: name: John, age: 30, city: New York
}
```

## For-Of Loop

The for-of loop is used to iterate over the values of an iterable object, such as an array or a string.

```javascript
let numbers = [1, 2, 3, 4, 5];
for (let number of numbers) {
  console.log(number); // Outputs: 1, 2, 3, 4, 5
}
```
