# Testing JavaScript

Tests provide repeatable evidence that code meets its contract. Good tests cover observable behaviour, including normal cases, boundaries, and failures.

The examples below use Node.js's built-in test runner.

## A pure function test

```javascript
// users.js
export function searchUsers(users, query) {
  const normalizedQuery = query.trim().toLowerCase();

  return users.filter((user) =>
    (user.name ?? "").toLowerCase().includes(normalizedQuery),
  );
}
```

```javascript
// users.test.js
import test from "node:test";
import assert from "node:assert/strict";
import { searchUsers } from "./users.js";

test("matches names without regard to case", () => {
  const users = [{ name: "Ada" }, { name: "Grace" }];

  assert.deepEqual(searchUsers(users, "ADA"), [{ name: "Ada" }]);
});

test("handles a missing name", () => {
  assert.deepEqual(searchUsers([{ id: 1 }], "ada"), []);
});
```

Run the tests with:

```bash
node --test
```

## Arrange, act, assert

A readable test usually has three parts:

1. Arrange the input and dependencies.
2. Act by calling the behaviour under test.
3. Assert the observable result.

Keep each test focused on one behaviour and give it a name that describes the expected outcome.

## Testing errors

```javascript
test("rejects a non-array response", () => {
  assert.throws(
    () => parseUsers({ users: [] }),
    { name: "TypeError", message: "Expected an array of users" },
  );
});
```

For rejected promises:

```javascript
test("rejects when the request fails", async () => {
  await assert.rejects(
    () => loadUsers(),
    /Request failed/,
  );
});
```

## Testing async code with injected dependencies

Pass a request function into the code instead of making a real network call:

```javascript
export async function loadActiveUsers(request) {
  const users = await request("/api/users");

  if (!Array.isArray(users)) {
    throw new TypeError("Expected an array of users");
  }

  return users.filter((user) => user.active === true);
}
```

```javascript
test("returns only active users", async () => {
  const request = async () => [
    { id: 1, active: true },
    { id: 2, active: false },
  ];

  const result = await loadActiveUsers(request);

  assert.deepEqual(result, [{ id: 1, active: true }]);
});
```

This is a small test double, but the assertion still checks the real data-processing behaviour.

## Testing mutation

When a function promises not to change its input, test that contract:

```javascript
test("sorts users without mutating the input", () => {
  const users = [{ name: "Grace" }, { name: "Ada" }];
  const snapshot = structuredClone(users);

  const result = sortUsers(users);

  assert.deepEqual(users, snapshot);
  assert.deepEqual(result, [{ name: "Ada" }, { name: "Grace" }]);
});
```

## Useful edge cases

Choose cases that can change the implementation's result:

- Empty arrays and empty strings.
- One item and duplicate items.
- Mixed letter casing and surrounding whitespace.
- `null`, `undefined`, and missing properties.
- Zero, negative values, and numeric strings.
- Equal sort values and deterministic tie-breaking.
- Invalid data shapes.
- Network rejection and non-successful HTTP status.
- Input mutation.

## What to test at each level

- Unit tests cover pure helpers and validation quickly.
- Integration tests cover modules working together, such as an API handler and service.
- End-to-end tests cover a critical user flow through the running application.

Prefer many focused unit tests, fewer integration tests, and a small number of valuable end-to-end tests.

## Common mistakes

- Testing implementation details instead of observable behaviour.
- Writing only a successful-case test.
- Making real network calls in unit tests.
- Sharing mutable data between tests.
- Using vague test names such as `works`.
- Changing a failing test merely to match incorrect code.
- Trusting coverage percentage as proof that assertions are meaningful.
