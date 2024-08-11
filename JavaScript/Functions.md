# JavaScript Functions

Functions are used to group a set of statements together to perform a specific task.

## Creating a function

To create a function, use the `function` keyword followed by the name of the function.

```javascript
function sayHello() {
  console.log("Hello!");
}
sayHello(); // "Hello!"
```

## Passing arguments to a function

To pass arguments to a function, you can use the `function(argument [, argument... ])` syntax.

```javascript
function greet(name) {
  console.log(`Hello, ${name}!`);
}
greet("Alice"); // "Hello, Alice!"
geet("Bob"); // "Hello, Bob!"
```

### Default Parameters

Default parameters allow you to set a default value for a parameter.

```javascript
function greet(name = "World") {
  console.log(`Hello, ${name}!`);
}
greet(); // "Hello, World!"
greet("Alice"); // "Hello, Alice!"
```

## Returning values from a function

To return a value from a function, use the `return` keyword.

```javascript
function sum(a, b) {
  return a + b;
}
console.log(sum(1, 2)); // 3
```

### Returning functions from a function

You can return a function from a function.

```javascript
function createAdder(a) {
  return function (b) {
    return a + b;
  };
}
let add5 = createAdder(5);
console.log(add5(2)); // 7
```

## Arrow Functions

Arrow functions are a new way to write functions.

```javascript
let sayHello = () => console.log("Hello!");
sayHello(); // "Hello!"
```

## Scope

Scope determines where variables are accessible.

### Global Scope

Variables defined outside of a function have global scope.

```javascript
let name = "John";
function sayHello() {
  console.log(`Hello, ${name}!`);
}
sayHello(); // "Hello, John!"
```

### Local Scope

Variables defined inside a function have local scope.

```javascript
function sayHello() {
  let name = "Alice";
}
sayHello(); // ReferenceError: name is not defined
```

### Block Scope

Variables defined inside a block have block scope.

```javascript
if (true) {
  let name = "Alice";
}
console.log(name); // ReferenceError: name is not defined
```

## Methods

Methods are functions that are properties of an object.

```javascript
let person = {
  sayHello: function () {
    console.log("Hello!");
  },
  // or
  sayBye() {
    console.log("Bye!");
  },
};
person.sayHello(); // "Hello!"
person.sayBye(); // "Bye!"
```
