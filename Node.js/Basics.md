# Node.js Basics

Node.js is a powerful JavaScript runtime built on Chrome's V8 JavaScript engine. It allows you to run JavaScript on the server-side, enabling the development of scalable network applications.

## Key Features

1. **Asynchronous and Event-Driven:** Node.js uses non-blocking, event-driven architecture, making it efficient and lightweight.
2. **Fast Execution:** Built on the V8 JavaScript engine, Node.js offers fast code execution.
3. **Single-Threaded but Highly Scalable:** Uses a single-threaded model with event looping.
4. **No Buffering:** Node.js applications never buffer any data.
5. **NPM (Node Package Manager):** Huge ecosystem of open-source libraries.

## Getting Started

1. Install Node.js from the official website: https://nodejs.org/

2. Verify installation by checking the version:

```bash
   node --version
   npm --version
```

3. Create a simple Node.js application:

```javascript
// app.js
console.log("Hello, Node.js!");
```

4. Run the application:

```bash
   node app.js
```

## Working with Modules

Node.js uses a module system to organize and reuse code. You can create your own modules or use built-in modules:

```javascript
// Built-in module
const fs = require("fs");

// Reading a file
fs.readFile("example.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Custom module (math.js)
exports.add = (a, b) => a + b;
exports.subtract = (a, b) => a - b;

// Using custom module
const math = require("./math");
console.log(math.add(5, 3)); // Output: 8
```

## npm init

You can initialize a new Node.js project using the `npm init` command:

```bash
npm init
```

### npm install

You can install Node.js packages using the `npm install` command:

```bash
npm install express
```
