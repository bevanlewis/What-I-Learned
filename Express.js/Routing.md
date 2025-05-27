## Routing

Express provides a powerful routing mechanism to define how an application responds to client requests to specific endpoints and HTTP methods. Here's an example of basic routing:

```javascript
const express = require("express");
const app = express();

// GET method route
app.get("/", (req, res) => {
  res.send("GET request to the homepage");
});

// POST method route
app.post("/", (req, res) => {
  res.send("POST request to the homepage");
});

// Route parameters
app.get("/users/:userId", (req, res) => {
  res.send(`User ID: ${req.params.userId}`);
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```
