# JavaScript Objects

Objects are used to store collections of data in key-value pairs.

## Creating Objects

You can create an object using object literal notation.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
};
```

## Accessing Object Properties

You can access object properties using dot notation or bracket notation.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
};
console.log(person.name); // "John"
console.log(person["age"]); // 30
```

## Updating Object Properties

You can update object properties by assigning a new value to them.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
};
person.age = 31;
console.log(person.age); // 31
```

## Adding New Properties

You can add new properties to an object by assigning a value to a new property name.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
};
person.newProperty = "new value";
console.log(person.newProperty); // "new value"
```

## Removing Properties

You can remove properties from an object using the delete operator.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
}:
delete person.age;
console.log(person.age); // undefined
```

## Object Iteration

You can iterate over an object using a `for...in` loop.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
};
for (let key in person) {
  console.log(key); // "name", "age", "isStudent"
}
```

## Object Destructuring

Object destructuring allows you to extract values from objects and assign them to variables.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
};
let { name, age } = person;
console.log(name); // "John"
console.log(age); // 30
```

### Object Destructuring with Spread Operator

You can use object destructuring with the spread operator to extract values from objects and assign them to variables.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
};
let { name, ...rest } = person;
console.log(name); // "John"
console.log(rest); // { age: 30, isStudent: true }
```

## Object Methods

You can extract the keys and values from an object using the `Object.keys()` and `Object.values()` methods.

```javascript
let person = {
  name: "John",
  age: 30,
  isStudent: true,
};
console.log(Object.keys(person)); // ["name", "age", "isStudent"]
console.log(Object.values(person)); // ["John", 30, true]
```
