# Asynchronous Javascript

Asynchronous programming is a programming paradigm that allows you to write non-blocking code.

## Callback Function

A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.

# setInterval and setTimeout

The `setInterval()` method executes a function or evaluates an expression at specified intervals (in milliseconds).
The `setTimeout()` method calls a function or evaluates an expression after a specified number of milliseconds.

## Promises

A promise is an object representing the eventual completion (or failure) of an asynchronous operation.

### Async and Await

The async and await keywords enable asynchronous, promise-based behavior to be written in a cleaner style, avoiding the need to explicitly configure promise chains.

A function declared with async automatically returns a promise. The await keword is used when we want to wait for a promise to resolve before continuing our code execution.

```javascript
async function doSomething() {
  await somethingElse();
  console.log("something");
}
doSomething();
```

### then, catch and finally

The `then()` method returns a Promise and deals with fulfilled results.
The `catch()` method returns a Promise and deals with rejected results.
The `finally()` method returns a Promise and deals with both fulfilled and rejected results.

```javascript
function getSomething() {
  // Writing our own Promise
  return new Promise((resolve, reject) => {
    if (/* everything turned out fine */) {
      resolve("We got the data");
    else {
        reject(Error("It broke"));
      }
    }
  }
}

// Calling the function which returns a promise
getSomething()
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.log(error);
  }).finally(() => console.log("Promise"));
```

# Dealing with API's and JSON

### Parsing Json

`JSON.parse(data)` is used to convert a JSON string into a JSON object.

`JSON.stringify(data)` is used to convert a JSON object into a JSON string.

## Fetch API

The Fetch API provides an interface for fetching resources (including across the network). It will seem familiar to anyone who has used XMLHttpRequest, but the new API provides a more powerful and flexible feature set.

### GET Request

```javascript
fetch("https://api.github.com/users/github")
  .then((response) => response.json())
  .then((data) => console.log(data));
```

### POST Request

```javascript
const data = {
  name: "github",
  age: 30,
  city: "San Francisco",
};
fetch("https://api.github.com/users/github", {
  method: "POST",
  body: JSON.stringify(data),
  headers: {
    "Content-type": "application/json; charset=UTF-8",
  },
});
```

## Fetch

The Fetch API provides an interface for fetching resources (including across the network).

```javascript
fetch("https://api.github.com/users/github")
  .then((response) => response.json()) // Parse the response as JSON
  .then((data) => console.log(data)); // Log the data to the console
```
