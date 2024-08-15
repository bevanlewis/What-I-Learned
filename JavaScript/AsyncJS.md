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

### then and catch

The `then()` method returns a Promise and deals with fulfilled results.
The `catch()` method returns a Promise and deals with rejected results.

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
  });
```
