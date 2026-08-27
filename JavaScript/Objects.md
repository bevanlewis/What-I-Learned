# JavaScript Objects

Objects store data as key-value pairs.

```javascript
const person = {
  name: "Ada",
  age: 36,
  active: true,
};
```

## Reading and updating properties

Use dot notation for known property names and bracket notation for dynamic names.

```javascript
console.log(person.name); // "Ada"

const property = "age";
console.log(person[property]); // 36

person.age = 37;
person.role = "engineer";
delete person.active;
```

## Checking whether a property exists

Checking the value against `undefined` cannot distinguish a missing property from a property explicitly set to `undefined`.

```javascript
const settings = { theme: undefined };

console.log(Object.hasOwn(settings, "theme")); // true
console.log(Object.hasOwn(settings, "language")); // false
```

Use `Object.hasOwn()` when inherited properties should not count.

## Optional chaining

Optional chaining returns `undefined` instead of throwing when an earlier value is `null` or `undefined`.

```javascript
const user = { company: null };

console.log(user.company?.name); // undefined
console.log(user.getDisplayName?.()); // undefined
```

It does not hide other errors. If `company` exists but `name` has an unexpected type, later operations can still fail.

## Nullish coalescing

`??` supplies a fallback only for `null` or `undefined`. `||` supplies a fallback for every falsy value, including `0`, `false`, and `""`.

```javascript
const pageSize = 0;

console.log(pageSize ?? 20); // 0
console.log(pageSize || 20); // 20
```

Use `??` when `0`, `false`, or an empty string is a valid value.

## Destructuring and defaults

```javascript
const user = {
  id: 1,
  profile: { name: "Ada" },
};

const {
  id,
  profile: { name },
  role = "user",
} = user;

console.log(id, name, role); // 1 "Ada" "user"
```

Destructuring defaults apply to `undefined`, but not to `null`.

## Rest and spread

```javascript
const user = { id: 1, name: "Ada", active: true };
const { id, ...details } = user;
const updated = { ...user, active: false };

console.log(details); // { name: "Ada", active: true }
console.log(updated); // { id: 1, name: "Ada", active: false }
```

Spread performs a shallow copy:

```javascript
const original = { profile: { name: "Ada" } };
const copy = { ...original };

copy.profile.name = "Grace";
console.log(original.profile.name); // "Grace"
```

Copy each changed nested level to update data immutably:

```javascript
const updated = {
  ...original,
  profile: {
    ...original.profile,
    name: "Grace",
  },
};
```

## Object utility methods

```javascript
const user = { id: 1, name: "Ada" };

console.log(Object.keys(user)); // ["id", "name"]
console.log(Object.values(user)); // [1, "Ada"]
console.log(Object.entries(user)); // [["id", 1], ["name", "Ada"]]
```

Transform entries back into an object with `Object.fromEntries()`:

```javascript
const query = Object.fromEntries([
  ["page", "1"],
  ["sort", "name"],
]);
```

## Iterating over objects

`Object.entries()` is usually clearer than `for...in` because it only includes the object's own enumerable properties.

```javascript
for (const [key, value] of Object.entries(user)) {
  console.log(key, value);
}
```

If using `for...in`, guard against inherited properties:

```javascript
for (const key in user) {
  if (Object.hasOwn(user, key)) {
    console.log(key, user[key]);
  }
}
```

## Computed property names

```javascript
function updateField(record, field, value) {
  return { ...record, [field]: value };
}
```

## Validating unknown object data

Data received from an API is not trustworthy merely because JSON parsing succeeded.

```javascript
function isUser(value) {
  return (
    typeof value === "object" &&
    value !== null &&
    typeof value.id === "number" &&
    typeof value.name === "string"
  );
}

function parseUsers(value) {
  if (!Array.isArray(value) || !value.every(isUser)) {
    throw new TypeError("Expected an array of users");
  }

  return value;
}
```

Remember that `typeof null` is `"object"`, so always check for `null` explicitly.

## Reference equality

Objects are compared by identity:

```javascript
const a = { id: 1 };
const b = { id: 1 };
const c = a;

console.log(a === b); // false
console.log(a === c); // true
```

## Common mistakes

- Treating a shallow spread as a deep copy.
- Using `||` when `0`, `false`, or `""` is valid.
- Assuming nested API properties always exist.
- Comparing objects with `===` when content equality is intended.
- Using `for...in` without considering inherited properties.
- Trusting parsed JSON without validating its shape.
