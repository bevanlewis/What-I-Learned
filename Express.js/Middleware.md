## Middleware in Express.js

Middleware functions are functions that have access to the request object (req), the response object (res), and the next middleware function in the application's request-response cycle, commonly denoted by a variable named `next`.

Here's an example of how to use middleware in Express:

```javascript
const express = require("express");
const app = express();

// Custom middleware function
const myMiddleware = (req, res, next) => {
  console.log("This is a middleware function");
  next(); // Call next() to pass control to the next middleware function
};

// Use the middleware
app.use(myMiddleware);

// Route handler
app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```
