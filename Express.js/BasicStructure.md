## Basic Express.js Application Structure

```javascript
const express = require("express");
const app = express();
const port = 3000;

// Define routes
app.get("/", (req, res) => {
  res.send("Hello World!");
});

// Start the server
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

This basic structure sets up a simple Express.js server that listens on port 3000 and responds with "Hello World!" when accessed at the root URL.
