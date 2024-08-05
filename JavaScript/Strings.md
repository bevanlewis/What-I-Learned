# JavaScript Strings

Strings are used for storing and manipulating text.

## Creating Strings

```javascript
let text = "Hello, world!";
let emptyString = "";
let multilineString = `This is a
multiline
string`;
```

## String Concatenation

```javascript
let firstName = "John";
let lastName = "Doe";
let fullName = firstName + " " + lastName;
console.log(fullName); // "John Doe"
```

### String Interpolation and Template Literals

String interpolation allows you to embed expressions inside string literals.

```javascript
let firstName = "John";
let lastName = "Doe";
let fullName = `${firstName} ${lastName}`;
console.log(fullName); // "John Doe"
```

console.log(fullName); // "John Doe"

## String Methods

String methods are used to manipulate strings.

```javascript
let text = "Hello, world!";
console.log(text.length); // 13
console.log(text.toUpperCase()); // "HELLO, WORLD!"
console.log(text.toLowerCase()); // "hello, world!"
console.log(text.indexOf("world")); // 7
console.log(text.substring(7, 12)); // "world"
console.log(text.replace("world", "JavaScript")); // "Hello, JavaScript!"
```

### Accessing a string character

You can access a string character using the square bracket notation or the charAt() method.

```javascript
let text = "Hello, world!";
console.log(text[0]); // "H"
console.log(text.charAt(0)); // "H"
```
