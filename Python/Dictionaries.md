# Python Dictionaries

Dictionaries are unordered, mutable collections of key-value pairs. They are also known as associative arrays or hash maps in other programming languages. Keys must be immutable (strings, numbers, tuples), while values can be any data type.

## Creating Dictionaries

```python
# Empty dictionary
empty_dict = {}

# Dictionary with key-value pairs
person = {
    "name": "Alice",
    "age": 30,
    "city": "New York"
}

# Using dict() constructor
person2 = dict(name="Bob", age=25, city="San Francisco")

# From list of tuples
pairs = [("name", "Charlie"), ("age", 35), ("city", "Chicago")]
person3 = dict(pairs)

# Dictionary comprehension
squares = {x: x**2 for x in range(1, 6)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

## Accessing Dictionary Values

```python
person = {
    "name": "Alice",
    "age": 30,
    "city": "New York",
    "hobbies": ["reading", "swimming"]
}

# Accessing values using keys
print(person["name"])    # "Alice"
print(person["age"])     # 30

# Using get() method (safer)
print(person.get("city"))      # "New York"
print(person.get("salary"))    # None (key doesn't exist)
print(person.get("salary", 0)) # 0 (default value)

# Accessing nested values
print(person["hobbies"][0])  # "reading"
```

## Modifying Dictionaries

```python
person = {"name": "Alice", "age": 30}

# Adding new key-value pairs
person["city"] = "New York"
person["email"] = "alice@example.com"

# Updating existing values
person["age"] = 31

# Using update() method
person.update({"age": 32, "phone": "123-456-7890"})

print(person)
# {"name": "Alice", "age": 32, "city": "New York", "email": "alice@example.com", "phone": "123-456-7890"}
```

## Removing Items from Dictionaries

```python
person = {
    "name": "Alice",
    "age": 30,
    "city": "New York",
    "email": "alice@example.com"
}

# Using del keyword
del person["email"]
print(person)  # {"name": "Alice", "age": 30, "city": "New York"}

# Using pop() method
age = person.pop("age")
print(age)     # 30
print(person)  # {"name": "Alice", "city": "New York"}

# Using popitem() - removes and returns last inserted item
last_item = person.popitem()
print(last_item)  # ("city", "New York")

# Clear all items
person.clear()
print(person)  # {}
```

## Dictionary Methods

### Keys, Values, and Items

```python
person = {"name": "Alice", "age": 30, "city": "New York"}

# Get all keys
print(person.keys())    # dict_keys(['name', 'age', 'city'])

# Get all values
print(person.values())  # dict_values(['Alice', 30, 'New York'])

# Get all key-value pairs
print(person.items())   # dict_items([('name', 'Alice'), ('age', 30), ('city', 'New York')])

# Convert to lists
keys_list = list(person.keys())
values_list = list(person.values())
items_list = list(person.items())
```

### Checking Membership

```python
person = {"name": "Alice", "age": 30, "city": "New York"}

# Check if key exists
print("name" in person)      # True
print("salary" in person)    # False
print("salary" not in person)  # True

# Check if value exists
print("Alice" in person.values())  # True
print("Bob" in person.values())    # False
```

### Dictionary Views

```python
person = {"name": "Alice", "age": 30}

keys_view = person.keys()
values_view = person.values()
items_view = person.items()

# Views are dynamic - they reflect changes to the dictionary
person["city"] = "New York"
print(keys_view)   # dict_keys(['name', 'age', 'city'])
print(values_view) # dict_values(['Alice', 30, 'New York'])
print(items_view)  # dict_items([('name', 'Alice'), ('age', 30), ('city', 'New York')])
```

## Iterating Over Dictionaries

```python
person = {"name": "Alice", "age": 30, "city": "New York"}

# Iterate over keys
for key in person:
    print(f"{key}: {person[key]}")

# Iterate over keys (explicit)
for key in person.keys():
    print(key)

# Iterate over values
for value in person.values():
    print(value)

# Iterate over key-value pairs
for key, value in person.items():
    print(f"{key}: {value}")

# Using enumerate with items
for index, (key, value) in enumerate(person.items()):
    print(f"{index}: {key} = {value}")
```

## Dictionary Comprehensions

```python
# Basic dictionary comprehension
squares = {x: x**2 for x in range(1, 6)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# With conditional
even_squares = {x: x**2 for x in range(1, 11) if x % 2 == 0}
print(even_squares)  # {2: 4, 4: 16, 6: 36, 8: 64, 10: 100}

# Transforming existing dictionary
person = {"name": "Alice", "age": 30, "city": "New York"}
upper_person = {key.upper(): str(value).upper() for key, value in person.items()}
print(upper_person)  # {'NAME': 'ALICE', 'AGE': '30', 'CITY': 'NEW YORK'}

# Inverting dictionary
original = {"a": 1, "b": 2, "c": 3}
inverted = {value: key for key, value in original.items()}
print(inverted)  # {1: 'a', 2: 'b', 3: 'c'}
```

## Nested Dictionaries

```python
# Dictionary containing other dictionaries
company = {
    "employees": {
        "alice": {"age": 30, "role": "developer"},
        "bob": {"age": 25, "role": "designer"},
        "charlie": {"age": 35, "role": "manager"}
    },
    "departments": {
        "engineering": ["alice"],
        "design": ["bob"],
        "management": ["charlie"]
    }
}

# Accessing nested values
print(company["employees"]["alice"]["role"])  # "developer"

# Modifying nested values
company["employees"]["alice"]["salary"] = 75000

# Adding nested structures
company["employees"]["david"] = {"age": 28, "role": "analyst"}
```

## Default Dictionaries

```python
from collections import defaultdict

# Regular dictionary - KeyError for missing keys
# person = {}
# print(person["age"])  # KeyError

# Defaultdict with default value
person = defaultdict(lambda: "Unknown")
print(person["name"])  # "Unknown"
print(person["age"])   # "Unknown"

# Defaultdict with int default (useful for counting)
word_count = defaultdict(int)
words = ["apple", "banana", "apple", "orange", "banana", "apple"]

for word in words:
    word_count[word] += 1

print(word_count)  # defaultdict(<class 'int'>, {'apple': 3, 'banana': 2, 'orange': 1})

# Defaultdict with list default
grouped_words = defaultdict(list)
for word in words:
    grouped_words[len(word)].append(word)

print(grouped_words)  # defaultdict(<class 'list'>, {5: ['apple'], 6: ['banana', 'orange']})
```

## Dictionary Operations

### Merging Dictionaries

```python
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
dict3 = {"a": 10, "e": 5}

# Using update() - modifies in place
dict1_copy = dict1.copy()
dict1_copy.update(dict2)
print(dict1_copy)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# Using ** unpacking (Python 3.5+)
merged = {**dict1, **dict2, **dict3}
print(merged)  # {'a': 10, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# Using | operator (Python 3.9+)
combined = dict1 | dict2 | dict3
print(combined)  # {'a': 10, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
```

### Dictionary Comparison

```python
dict1 = {"a": 1, "b": 2}
dict2 = {"a": 1, "b": 2}
dict3 = {"b": 2, "a": 1}

print(dict1 == dict2)  # True (same key-value pairs)
print(dict1 == dict3)  # True (order doesn't matter)
print(dict1 is dict2)  # False (different objects)
```

## Common Dictionary Patterns

### Counting with Dictionaries

```python
# Count word frequencies
text = "the quick brown fox jumps over the lazy dog"
words = text.split()

word_freq = {}
for word in words:
    word_freq[word] = word_freq.get(word, 0) + 1

print(word_freq)  # {'the': 2, 'quick': 1, 'brown': 1, 'fox': 1, 'jumps': 1, 'over': 1, 'lazy': 1, 'dog': 1}

# Using defaultdict
from collections import defaultdict
word_freq = defaultdict(int)
for word in words:
    word_freq[word] += 1
```

### Grouping Data

```python
students = [
    {"name": "Alice", "grade": "A", "subject": "Math"},
    {"name": "Bob", "grade": "B", "subject": "Math"},
    {"name": "Charlie", "grade": "A", "subject": "Science"},
    {"name": "David", "grade": "C", "subject": "Math"},
    {"name": "Eve", "grade": "B", "subject": "Science"}
]

# Group by subject
by_subject = {}
for student in students:
    subject = student["subject"]
    if subject not in by_subject:
        by_subject[subject] = []
    by_subject[subject].append(student)

print(by_subject)

# Group by grade
from collections import defaultdict
by_grade = defaultdict(list)
for student in students:
    by_grade[student["grade"]].append(student)

print(by_grade)
```

### Creating Lookup Tables

```python
# Month name to number mapping
month_names = {
    "January": 1, "February": 2, "March": 3, "April": 4,
    "May": 5, "June": 6, "July": 7, "August": 8,
    "September": 9, "October": 10, "November": 11, "December": 12
}

# Reverse mapping
month_numbers = {v: k for k, v in month_names.items()}

print(month_names["March"])     # 3
print(month_numbers[3])         # "March"
```

## Dictionary Performance

- Average O(1) time complexity for get, set, and delete operations
- Keys must be hashable (immutable)
- Dictionaries maintain insertion order (Python 3.7+)
- Use dictionaries for fast lookups and when you need key-value relationships

## Memory Considerations

```python
# Dictionaries have overhead for each key-value pair
# Use tuples as keys for composite keys
coordinates = {}
coordinates[(1, 2)] = "point A"
coordinates[(3, 4)] = "point B"

print(coordinates[(1, 2)])  # "point A"

# But not lists (they're not hashable)
# coordinates[[1, 2]] = "point A"  # TypeError
```

## Best Practices

1. Use descriptive key names
2. Prefer `get()` method over direct access when key might not exist
3. Use `defaultdict` for counting and grouping operations
4. Consider using tuples as keys when you need composite keys
5. Use dictionary comprehensions for creating dictionaries from iterables
6. Be aware of the overhead when dealing with large dictionaries

## OrderedDict (from collections)

```python
from collections import OrderedDict

# OrderedDict maintains insertion order and has additional methods
od = OrderedDict()
od["first"] = 1
od["second"] = 2
od["third"] = 3

print(od)  # OrderedDict([('first', 1), ('second', 2), ('third', 3)])

# Move item to end
od.move_to_end("first")
print(od)  # OrderedDict([('second', 2), ('third', 3), ('first', 1)])

# Get first/last items
print(od.popitem(last=False))  # ('second', 2) - removes first item
```
