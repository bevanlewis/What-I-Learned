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
console.log(text.includes("world")); // true
console.log("  hello  ".trim()); // "hello"
```

### Accessing a string character

You can access a string character using the square bracket notation or the charAt() method.

```javascript
let text = "Hello, world!";
console.log(text[0]); // "H"
console.log(text.charAt(0)); // "H"
```

## Converting Strings to Numbers

You can convert a string to a number using the Number() function or the parseInt() and parseFloat() functions.

```javascript
const integerText = "123";
const decimalText = "12.5";

console.log(Number(integerText)); // 123
console.log(Number.parseInt(integerText, 10)); // 123
console.log(Number.parseFloat(decimalText)); // 12.5
console.log(Number.isNaN(Number("unknown"))); // true
```

## Case-insensitive comparison and search

Normalize both values before comparing them:

```javascript
function includesIgnoreCase(value, searchTerm) {
  return value.toLowerCase().includes(searchTerm.toLowerCase());
}
```

Use `localeCompare()` for user-facing alphabetical sorting:

```javascript
const names = ["Grace", "ada"];
const sorted = names.toSorted((a, b) =>
  a.localeCompare(b, undefined, { sensitivity: "base" }),
);
```
