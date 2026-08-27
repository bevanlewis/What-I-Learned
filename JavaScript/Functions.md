# JavaScript Functions

Functions group reusable behaviour. Prefer small functions with clear inputs, outputs, and names.

## Function declarations and expressions

```javascript
function add(a, b) {
  return a + b;
}

const subtract = function (a, b) {
  return a - b;
};
```

Function declarations are hoisted, so they can be called earlier in the scope. A function expression cannot be used before its variable is initialized.

## Arrow functions

```javascript
const multiply = (a, b) => a * b;

const describeUser = (user) => ({
  id: user.id,
  label: `${user.name} (${user.role})`,
});
```

Parentheses are required when an arrow function implicitly returns an object literal.

Arrow functions do not define their own `this`, `arguments`, or `prototype`. Use a normal method when dynamic `this` is required:

```javascript
const person = {
  name: "Ada",
  greet() {
    return `Hello, ${this.name}`;
  },
};
```

## Parameters

### Default parameters

Defaults apply when an argument is omitted or is `undefined`, but not when it is `null`.

```javascript
function greet(name = "World") {
  return `Hello, ${name}!`;
}
```

### Rest parameters

Rest collects remaining arguments into an array:

```javascript
function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}
```

### Object parameters

An options object is clearer than several positional boolean arguments:

```javascript
function listUsers(users, { activeOnly = false, limit = 10 } = {}) {
  const selected = activeOnly
    ? users.filter((user) => user.active)
    : users;

  return selected.slice(0, limit);
}
```

## Return values and early returns

A function without an explicit `return` returns `undefined`. Early returns keep validation and exceptional cases easy to follow.

```javascript
function normalizeName(value) {
  if (typeof value !== "string") {
    return "";
  }

  return value.trim().toLowerCase();
}
```

## Callbacks and higher-order functions

A callback is passed to another function. A higher-order function accepts or returns a function.

```javascript
function selectUsers(users, predicate) {
  return users.filter(predicate);
}

const activeUsers = selectUsers(users, (user) => user.active);
```

Returning a function can configure reusable behaviour:

```javascript
function hasMinimumScore(minimum) {
  return (user) => user.score >= minimum;
}

const qualified = users.filter(hasMinimumScore(80));
```

## Closures

A closure lets a function retain access to variables from the scope in which it was created.

```javascript
function createCounter() {
  let count = 0;

  return () => {
    count += 1;
    return count;
  };
}

const next = createCounter();
console.log(next()); // 1
console.log(next()); // 2
```

Closures are useful for factories, private state, event handlers, and dependency injection.

## Scope

- Global scope is accessible throughout the program.
- Function scope is accessible inside a function.
- Block scope is accessible inside a `{}` block.
- `let` and `const` are block-scoped; `var` is function-scoped.

```javascript
function example() {
  const functionValue = "available in this function";

  if (true) {
    const blockValue = "available only in this block";
    console.log(functionValue, blockValue);
  }
}
```

## Pure functions and side effects

A pure function produces the same result for the same inputs and does not change external state.

```javascript
function addTax(price, rate) {
  return price * (1 + rate);
}
```

Keep data transformation pure where practical. Put network requests, file access, logging, and UI updates at clear boundaries. Pure helpers are easier to test.

## Function composition

Complex processing is easier to understand as named steps:

```javascript
const normalize = (value) => value.trim().toLowerCase();
const includesQuery = (name, query) => normalize(name).includes(normalize(query));

function searchActiveUsers(users, query) {
  return users
    .filter((user) => user.active)
    .filter((user) => includesQuery(user.name ?? "", query));
}
```

## Dependency injection

Passing dependencies into a function makes behaviour easier to test and reuse:

```javascript
async function loadUsers(request) {
  const response = await request("/api/users");
  return response.data;
}
```

Production code can pass an HTTP client; tests can pass a small deterministic function.

## Common mistakes

- Forgetting to return a value from a block-bodied arrow function.
- Using an arrow function as an object method when it needs its own `this`.
- Mutating an input when callers expect a pure transformation.
- Passing many positional flags instead of a readable options object.
- Combining validation, network access, transformation, and display in one function.
- Catching an error inside a helper and returning `undefined` without documenting that behaviour.
