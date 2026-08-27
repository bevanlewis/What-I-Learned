# JavaScript Errors

Errors represent failures that prevent an operation from producing a valid result.

## Throwing errors

Throw an `Error` object rather than a string so callers receive a message, name, and stack trace.

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new RangeError("The divisor cannot be zero");
  }

  return a / b;
}
```

Useful built-in error types include:

- `TypeError` for a value of the wrong type.
- `RangeError` for a value outside an allowed range.
- `SyntaxError` for invalid syntax or parsing.
- `Error` for a general failure.

## `try`, `catch`, and `finally`

```javascript
try {
  const result = divide(10, 0);
  console.log(result);
} catch (error) {
  if (error instanceof RangeError) {
    console.error(error.message);
  } else {
    throw error;
  }
} finally {
  console.log("This runs whether the operation succeeds or fails");
}
```

Catch an error only when the current layer can recover, add context, translate it, or perform cleanup. Re-throw unexpected errors instead of hiding them.

## Errors in async functions

A thrown error in an `async` function becomes a rejected promise.

```javascript
async function loadSettings() {
  const response = await fetch("/api/settings");

  if (!response.ok) {
    throw new Error(`Failed to load settings: ${response.status}`);
  }

  return response.json();
}

try {
  const settings = await loadSettings();
  console.log(settings);
} catch (error) {
  console.error("Unable to load settings", error);
}
```

## Custom errors

Custom types let callers handle expected failure categories without comparing message text.

```javascript
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

function validateUser(user) {
  if (typeof user?.name !== "string" || user.name.trim() === "") {
    throw new ValidationError("A name is required", "name");
  }
}
```

## Adding context while preserving the cause

```javascript
async function loadUser(id) {
  try {
    return await fetchJson(`/api/users/${id}`);
  } catch (error) {
    throw new Error(`Unable to load user ${id}`, { cause: error });
  }
}
```

The `cause` property keeps the original error available for debugging.

## Expected results versus exceptions

Use a normal return value for an expected outcome such as "no matching user." Use an exception when the function cannot meet its contract.

```javascript
function findUser(users, id) {
  return users.find((user) => user.id === id) ?? null;
}
```

## Error-handling boundaries

A useful separation is:

1. Low-level functions throw detailed technical errors.
2. Application logic decides whether to recover, retry, or propagate.
3. The UI or API boundary converts the failure into an appropriate user-facing message or response.

Do not expose secrets, stack traces, SQL, tokens, or internal server details to users.

## Common mistakes

- Throwing strings instead of `Error` objects.
- Catching every error and returning `undefined`.
- Comparing error message strings when a type or code is available.
- Displaying sensitive internal errors to users.
- Using exceptions for ordinary control flow.
- Logging an error and then pretending the operation succeeded.
