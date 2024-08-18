# JavaScript Errors

Errors are used to handle errors that occur during the execution of a program.

## try...catch Statement

The `try...catch` statement is used to handle errors that occur during the execution of a program.

```javascript
try {
  // code that may throw an error
  catch (error) {
    // code to handle the error
    }
}
```

## throw Statement

The `throw` statement is used to throw an error.

```javascript
throw new Error("Something went wrong!");
```

## Error Object

The `Error` object is used to create a new error.

```javascript
let error = new Error("Something went wrong!");
```

## Custom Error

Extnding the Error object allows you to create custom error types.

```javascript
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = "CustomError";
  }
}
throw new CustomError("Something went wrong!");
```
