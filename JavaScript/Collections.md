# JavaScript Collections: `Set` and `Map`

Arrays and objects cover many use cases, but `Set` and `Map` make uniqueness and keyed lookups explicit.

## `Set`

A `Set` stores unique values and preserves insertion order.

```javascript
const ids = new Set([1, 2, 2, 3]);

console.log(ids.size); // 3
console.log(ids.has(2)); // true

ids.add(4);
ids.delete(1);
```

### Removing duplicates

```javascript
const values = ["open", "closed", "open"];
const uniqueValues = [...new Set(values)];

console.log(uniqueValues); // ["open", "closed"]
```

Objects are unique by reference, not by content:

```javascript
const records = new Set([{ id: 1 }, { id: 1 }]);
console.log(records.size); // 2
```

To deduplicate objects by a property, use a `Map`:

```javascript
const users = [
  { id: 1, name: "Old" },
  { id: 1, name: "New" },
  { id: 2, name: "Ada" },
];

const uniqueUsers = [...new Map(users.map((user) => [user.id, user])).values()];
```

Because later entries replace earlier entries with the same key, user `1` has the name `"New"`.

## `Map`

A `Map` stores key-value pairs. Unlike object keys, map keys can be values of any type.

```javascript
const usersById = new Map();

usersById.set(1, { id: 1, name: "Ada" });
usersById.set(2, { id: 2, name: "Grace" });

console.log(usersById.get(1)); // { id: 1, name: "Ada" }
console.log(usersById.has(3)); // false
console.log(usersById.size); // 2
```

### Building an index

Repeatedly searching an array is linear for each lookup. A `Map` provides direct lookup by key.

```javascript
const users = [
  { id: 1, name: "Ada" },
  { id: 2, name: "Grace" },
];

const usersById = new Map(users.map((user) => [user.id, user]));
const selected = usersById.get(2);
```

### Counting frequencies

```javascript
function countValues(values) {
  const counts = new Map();

  for (const value of values) {
    counts.set(value, (counts.get(value) ?? 0) + 1);
  }

  return counts;
}
```

### Iterating

```javascript
for (const [id, user] of usersById) {
  console.log(id, user.name);
}
```

## Choosing a collection

| Requirement | Collection |
| --- | --- |
| Ordered list, indexes, transformations | `Array` |
| Unique values or fast membership checks | `Set` |
| Keyed lookup with keys of any type | `Map` |
| JSON-shaped record with named fields | `Object` |

`Map` and `Set` are not represented directly by JSON. Convert them before serializing:

```javascript
const mapJson = JSON.stringify(Object.fromEntries(usersById));
const setJson = JSON.stringify([...ids]);
```

## Common mistakes

- Expecting `Set` to deduplicate separate objects with identical contents.
- Using `map[key]` instead of `map.get(key)`.
- Expecting `JSON.stringify()` to serialize `Map` or `Set` contents automatically.
- Building a `Map` for a collection that is only read once and does not need keyed lookup.
