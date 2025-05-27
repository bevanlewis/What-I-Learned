## Error Handling

Express comes with a built-in error handler that takes care of any errors that might be encountered in the app. Here's a basic example of error handling:

```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  throw new Error("Something went wrong!");
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```
