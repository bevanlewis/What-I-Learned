# Asynchronous JavaScript and APIs

Asynchronous JavaScript lets a program start work that finishes later without blocking all other work. Network requests, timers, and file operations commonly use promises.

## The event loop

JavaScript executes synchronous code on the call stack. Promise callbacks are queued as microtasks, while timers are queued as tasks. Microtasks run before the next task.

```javascript
console.log("start");

setTimeout(() => console.log("timer"), 0);
Promise.resolve().then(() => console.log("promise"));

console.log("end");
// start, end, promise, timer
```

## Promises

A promise is pending, fulfilled, or rejected.

```javascript
function delay(milliseconds) {
  return new Promise((resolve) => {
    setTimeout(resolve, milliseconds);
  });
}

delay(100)
  .then(() => "finished")
  .then(console.log)
  .catch(console.error)
  .finally(() => console.log("complete"));
```

Return a promise from a `.then()` callback when the next step depends on it. A thrown error or rejected promise skips to the nearest `.catch()`.

## `async` and `await`

An `async` function always returns a promise. `await` pauses only that async function, not the entire JavaScript runtime.

```javascript
async function getMessage() {
  await delay(100);
  return "finished";
}

const message = await getMessage();
```

Use `try...catch` when the current layer can recover, add useful context, or translate the error. Otherwise allow the rejection to reach the caller.

## Fetching JSON safely

`fetch()` rejects for network failures, but it does not reject merely because the server returns an HTTP error such as `404` or `500`. Check `response.ok` explicitly.

```javascript
async function fetchJson(url, options = {}) {
  const response = await fetch(url, options);

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`);
  }

  return response.json();
}
```

`response.json()` is also asynchronous and can reject when the body is not valid JSON.

### GET request and data processing

```javascript
async function getActiveUsers(searchTerm = "") {
  const data = await fetchJson("https://example.com/api/users");

  if (!Array.isArray(data)) {
    throw new TypeError("Expected an array of users");
  }

  const query = searchTerm.trim().toLowerCase();

  return data
    .filter((user) => user?.active === true)
    .filter((user) =>
      typeof user.name === "string" &&
      user.name.toLowerCase().includes(query),
    )
    .toSorted((a, b) => a.name.localeCompare(b.name));
}
```

### POST request

```javascript
async function createUser(user) {
  return fetchJson("https://example.com/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(user),
  });
}
```

## Axios

Axios rejects for non-successful HTTP status codes by default and makes parsed response data available on `response.data`.

```javascript
import axios from "axios";

async function getUsers() {
  const response = await axios.get("https://example.com/api/users");

  if (!Array.isArray(response.data)) {
    throw new TypeError("Expected an array of users");
  }

  return response.data;
}
```

An Axios error may contain `error.response` for a server response, `error.request` when no response arrived, or neither when setup failed. Avoid exposing internal server details directly to users.

## Sequential and parallel work

Use sequential awaits when each operation depends on the previous result:

```javascript
const user = await fetchJson("/api/user/1");
const company = await fetchJson(`/api/companies/${user.companyId}`);
```

Start independent operations together:

```javascript
const [users, projects] = await Promise.all([
  fetchJson("/api/users"),
  fetchJson("/api/projects"),
]);
```

`Promise.all()` rejects when any input rejects. Use `Promise.allSettled()` when every result must be inspected even if some operations fail.

```javascript
const results = await Promise.allSettled(requests);

const successfulValues = results
  .filter((result) => result.status === "fulfilled")
  .map((result) => result.value);
```

## Async array pitfalls

`forEach()` does not wait for async callbacks:

```javascript
// Incorrect: the outer code does not wait for these operations.
users.forEach(async (user) => {
  await saveUser(user);
});
```

For parallel work:

```javascript
await Promise.all(users.map((user) => saveUser(user)));
```

For sequential work:

```javascript
for (const user of users) {
  await saveUser(user);
}
```

## Timeouts and cancellation

Use `AbortController` to cancel a fetch:

```javascript
async function fetchWithTimeout(url, milliseconds = 5000) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), milliseconds);

  try {
    return await fetchJson(url, { signal: controller.signal });
  } finally {
    clearTimeout(timeoutId);
  }
}
```

## Retry decisions

Retries are appropriate only for temporary failures and idempotent operations. Do not automatically retry validation errors, authentication failures, or non-idempotent writes unless the API supports safe retrying.

A production retry strategy normally includes:

- A small maximum attempt count.
- Exponential backoff and jitter.
- Cancellation or an overall time limit.
- Logging or metrics.
- A decision about which status codes are retryable.

## JSON

```javascript
const json = JSON.stringify({ name: "Ada" });
const value = JSON.parse(json);
```

JSON parsing validates syntax, not the shape or types of the resulting data. Validate API data before using it.

## Common mistakes

- Forgetting to check `response.ok` with `fetch()`.
- Forgetting `await response.json()`.
- Running independent requests sequentially.
- Using async callbacks with `forEach()`.
- Catching an error and silently returning incomplete data.
- Assuming parsed JSON has the expected structure.
- Retrying every failure, including invalid input.
- Updating UI state after a request has been cancelled or superseded.
