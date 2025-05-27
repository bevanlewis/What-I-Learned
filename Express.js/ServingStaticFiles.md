## Serving Static Files

Express provides a built-in middleware function `express.static()` to serve static files such as images, CSS files, and JavaScript files.

Here's how you can use it:

```javascript
const express = require('express');
const path = require('path');
const app = express();

// Serve static files from the 'public' directory
app.use(express.static(path.join(\_\_dirname, 'public')));

app.listen(3000, () => {
console.log('Server is running on port 3000');
});
```
