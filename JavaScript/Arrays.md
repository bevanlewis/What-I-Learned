# JavaScript Arrays

Arrays store ordered collections of values. Array indexes start at `0`.

```javascript
const fruits = ["apple", "banana", "orange"];

console.log(fruits[0]); // "apple"
console.log(fruits.length); // 3
```

## Mutating and non-mutating operations

Some array methods change the original array. This matters when the same array is used elsewhere, particularly in UI state.

| Mutates the array | Returns a new value or array |
| --- | --- |
| `push`, `pop`, `shift`, `unshift` | `concat`, `slice` |
| `splice`, `sort`, `reverse` | `map`, `filter`, `toSorted` |

```javascript
const original = [3, 1, 2];
const sorted = [...original].sort((a, b) => a - b);

console.log(original); // [3, 1, 2]
console.log(sorted); // [1, 2, 3]
```

`toSorted()` is a modern non-mutating alternative to `sort()`:

```javascript
const sorted = [3, 1, 2].toSorted((a, b) => a - b);
```

Use `[...array].sort(...)` when the runtime does not support `toSorted()`.

## Adding and removing values

```javascript
const fruits = ["banana"];

fruits.push("orange"); // Add to the end
fruits.unshift("apple"); // Add to the beginning

const last = fruits.pop(); // Remove from the end
const first = fruits.shift(); // Remove from the beginning
```

`slice(start, end)` copies part of an array without changing it. The end index is excluded.

```javascript
const values = ["a", "b", "c", "d"];
console.log(values.slice(1, 3)); // ["b", "c"]
console.log(values); // ["a", "b", "c", "d"]
```

`splice(start, deleteCount, ...items)` changes the original array.

```javascript
const values = ["a", "b", "c"];
const removed = values.splice(1, 1, "new");

console.log(removed); // ["b"]
console.log(values); // ["a", "new", "c"]
```

## Iterating and transforming

### `forEach`

Use `forEach()` for side effects such as logging. It always returns `undefined`.

```javascript
["a", "b"].forEach((value, index) => {
  console.log(index, value);
});
```

### `map`

Use `map()` when every input item should produce one output item.

```javascript
const users = [
  { id: 1, name: "Ada" },
  { id: 2, name: "Grace" },
];

const names = users.map((user) => user.name);
// ["Ada", "Grace"]
```

### `filter`

Use `filter()` to keep the items that satisfy a condition.

```javascript
const users = [
  { name: "Ada", active: true },
  { name: "Grace", active: false },
];

const activeUsers = users.filter((user) => user.active);
```

### `find` and `findIndex`

`find()` returns the first matching value or `undefined`. `findIndex()` returns its index or `-1`.

```javascript
const users = [{ id: 1 }, { id: 2 }];

console.log(users.find((user) => user.id === 2)); // { id: 2 }
console.log(users.findIndex((user) => user.id === 3)); // -1
```

### `some` and `every`

```javascript
const values = [2, 4, 6];

console.log(values.some((value) => value > 5)); // true
console.log(values.every((value) => value % 2 === 0)); // true
```

### `reduce`

`reduce()` combines an array into one result. Supply an initial accumulator value so empty arrays behave predictably.

```javascript
const prices = [10, 15, 20];
const total = prices.reduce((sum, price) => sum + price, 0);

console.log(total); // 45
```

It can also group or count values:

```javascript
const statuses = ["open", "closed", "open"];

const counts = statuses.reduce((result, status) => {
  result[status] = (result[status] ?? 0) + 1;
  return result;
}, {});

console.log(counts); // { open: 2, closed: 1 }
```

Prefer a simple loop when a complex `reduce()` would be difficult to read.

## Sorting correctly

Without a comparison function, `sort()` converts values to strings. This produces incorrect numeric ordering:

```javascript
console.log([2, 10, 3].sort()); // [10, 2, 3]
```

Use a comparator for numbers:

```javascript
const ascending = [2, 10, 3].toSorted((a, b) => a - b);
const descending = [2, 10, 3].toSorted((a, b) => b - a);
```

Use `localeCompare()` for strings:

```javascript
const users = [{ name: "Grace" }, { name: "ada" }];

const sorted = users.toSorted((a, b) =>
  a.name.localeCompare(b.name, undefined, { sensitivity: "base" }),
);
```

When values may be missing, choose an explicit fallback:

```javascript
const sorted = users.toSorted((a, b) =>
  (a.company?.name ?? "").localeCompare(b.company?.name ?? ""),
);
```

For deterministic results, add a secondary comparison when primary values are equal:

```javascript
const sorted = users.toSorted(
  (a, b) =>
    a.company.localeCompare(b.company) || a.name.localeCompare(b.name),
);
```

## Chaining data-processing methods

Each step should have one clear responsibility:

```javascript
function getActiveUserNames(users, searchTerm) {
  const query = searchTerm.trim().toLowerCase();

  return users
    .filter((user) => user.active === true)
    .filter((user) => (user.name ?? "").toLowerCase().includes(query))
    .toSorted((a, b) => a.name.localeCompare(b.name))
    .map((user) => user.name);
}
```

## Spread and destructuring

Spread creates a shallow copy. Nested objects are still shared references.

```javascript
const first = ["a", "b"];
const combined = [...first, "c"];
const [head, ...rest] = combined;

console.log(head); // "a"
console.log(rest); // ["b", "c"]
```

## Checking and comparing arrays

```javascript
console.log(Array.isArray([])); // true
console.log(["a", "b"].includes("b")); // true
console.log(["a", "b"].indexOf("missing")); // -1
```

Arrays are compared by reference, not contents:

```javascript
console.log([1, 2] === [1, 2]); // false
```

For simple one-dimensional arrays:

```javascript
function arraysEqual(a, b) {
  return a.length === b.length && a.every((value, index) => value === b[index]);
}
```

This is not a deep comparison for nested objects or arrays.

## Common mistakes

- Calling `sort()` directly on an array that must remain unchanged.
- Forgetting a numeric comparator.
- Expecting `forEach()` to return a transformed array.
- Omitting the initial value passed to `reduce()`.
- Assuming `find()` always returns a value.
- Accessing nested properties without handling missing data.
