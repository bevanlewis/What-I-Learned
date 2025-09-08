# Python Tuples

Tuples are ordered, immutable collections of items. Unlike lists, tuples cannot be modified after creation. They are useful for storing related data that shouldn't be changed, such as coordinates, RGB colors, or database records.

## Creating Tuples

```python
# Empty tuple
empty_tuple = ()

# Tuple with single element (note the comma)
single_element = (42,)
not_a_tuple = (42)  # This is just an integer

# Tuple with multiple elements
coordinates = (10, 20)
person = ("Alice", 30, "Engineer")

# Without parentheses (tuple packing)
point = 3, 4, 5
print(type(point))  # <class 'tuple'>

# Using tuple() constructor
numbers = tuple([1, 2, 3, 4, 5])
letters = tuple("hello")

print(numbers)  # (1, 2, 3, 4, 5)
print(letters)  # ('h', 'e', 'l', 'l', 'o')
```

## Accessing Tuple Elements

```python
colors = ("red", "green", "blue", "yellow", "purple")

# Indexing
print(colors[0])   # "red"
print(colors[2])   # "blue"

# Negative indexing
print(colors[-1])  # "purple" (last element)
print(colors[-2])  # "yellow" (second to last)

# Slicing
print(colors[1:4])  # ("green", "blue", "yellow")
print(colors[:3])   # ("red", "green", "blue")
print(colors[::2])  # ("red", "blue", "purple") (every second element)
```

## Tuple Immutability

```python
colors = ("red", "green", "blue")

# This will raise an error
# colors[0] = "orange"  # TypeError: 'tuple' object does not support item assignment

# Tuples containing mutable objects can have those objects modified
nested = ([1, 2], [3, 4])
nested[0].append(3)  # This works!
print(nested)  # ([1, 2, 3], [3, 4])

# But you can't replace the list itself
# nested[0] = [5, 6]  # TypeError: 'tuple' object does not support item assignment
```

## Tuple Methods

Tuples have only two built-in methods due to their immutability:

```python
numbers = (1, 2, 3, 2, 4, 2, 5)

# count() - count occurrences of a value
print(numbers.count(2))  # 3

# index() - find index of first occurrence
print(numbers.index(4))  # 4
print(numbers.index(2))  # 1

# index with start parameter
print(numbers.index(2, 2))  # 3 (start searching from index 2)
```

## Tuple Operations

### Concatenation

```python
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)

# Using + operator
combined = tuple1 + tuple2
print(combined)  # (1, 2, 3, 4, 5, 6)

# Note: This creates a new tuple, doesn't modify existing ones
```

### Repetition

```python
numbers = (1, 2, 3)
repeated = numbers * 3
print(repeated)  # (1, 2, 3, 1, 2, 3, 1, 2, 3)
```

### Membership Testing

```python
colors = ("red", "green", "blue")

print("red" in colors)     # True
print("yellow" in colors)  # False
print("yellow" not in colors)  # True
```

### Length and Iteration

```python
colors = ("red", "green", "blue")

print(len(colors))  # 3

# Iteration
for color in colors:
    print(color)

# With index
for index, color in enumerate(colors):
    print(f"{index}: {color}")
```

## Tuple Unpacking

```python
# Basic unpacking
point = (3, 4)
x, y = point
print(x, y)  # 3 4

# Unpacking with extended iterable unpacking (Python 3)
numbers = (1, 2, 3, 4, 5)
first, *middle, last = numbers
print(first)   # 1
print(middle)  # [2, 3, 4]
print(last)    # 5

# Ignoring values
name, _, age, _ = ("Alice", "Smith", 30, "Engineer")
print(name, age)  # Alice 30

# Swapping values
a, b = 1, 2
a, b = b, a
print(a, b)  # 2 1
```

## Named Tuples

Named tuples provide a way to create tuple subclasses with named fields:

```python
from collections import namedtuple

# Define a named tuple
Person = namedtuple('Person', ['name', 'age', 'city'])

# Create instances
alice = Person('Alice', 30, 'New York')
bob = Person('Bob', 25, 'San Francisco')

print(alice.name)   # "Alice"
print(alice.age)    # 30
print(alice.city)   # "New York"

# Access by index
print(alice[0])     # "Alice"
print(alice[1])     # 30

# Convert to dictionary
print(alice._asdict())  # {'name': 'Alice', 'age': 30, 'city': 'New York'}

# Replace values
older_alice = alice._replace(age=31)
print(older_alice)  # Person(name='Alice', age=31, city='New York')
```

## Common Use Cases for Tuples

### Returning Multiple Values from Functions

```python
def get_user_info():
    name = "Alice"
    age = 30
    email = "alice@example.com"
    return name, age, email

user_info = get_user_info()
print(user_info)  # ("Alice", 30, "alice@example.com")

# Unpack the result
name, age, email = get_user_info()
print(f"Name: {name}, Age: {age}, Email: {email}")
```

### Using Tuples as Dictionary Keys

```python
# Tuples can be used as dictionary keys (lists cannot)
coordinates = {}
coordinates[(1, 2)] = "Point A"
coordinates[(3, 4)] = "Point B"
coordinates[(1, 2)] = "Updated Point A"

print(coordinates[(1, 2)])  # "Updated Point A"

# RGB color values
colors = {
    (255, 0, 0): "Red",
    (0, 255, 0): "Green",
    (0, 0, 255): "Blue"
}

print(colors[(255, 0, 0)])  # "Red"
```

### Grouping Related Data

```python
# Employee records
employees = [
    ("Alice", "Engineering", 75000),
    ("Bob", "Marketing", 65000),
    ("Charlie", "Engineering", 80000)
]

# Database-like records
users = [
    (1, "alice", "alice@example.com", True),
    (2, "bob", "bob@example.com", False),
    (3, "charlie", "charlie@example.com", True)
]
```

## Tuple vs List Comparison

| Aspect      | Tuple                    | List          |
| ----------- | ------------------------ | ------------- |
| Mutability  | Immutable                | Mutable       |
| Syntax      | `(1, 2, 3)` or `1, 2, 3` | `[1, 2, 3]`   |
| Performance | Faster                   | Slower        |
| Memory      | Less memory              | More memory   |
| Methods     | Few methods              | Many methods  |
| Use case    | Fixed data               | Changing data |

```python
import sys

# Memory comparison
tuple_data = (1, 2, 3, 4, 5)
list_data = [1, 2, 3, 4, 5]

print(f"Tuple size: {sys.getsizeof(tuple_data)} bytes")
print(f"List size: {sys.getsizeof(list_data)} bytes")
```

## Converting Between Tuples and Lists

```python
# List to tuple
my_list = [1, 2, 3, 4, 5]
my_tuple = tuple(my_list)
print(my_tuple)  # (1, 2, 3, 4, 5)

# Tuple to list
my_tuple = (1, 2, 3, 4, 5)
my_list = list(my_tuple)
print(my_list)  # [1, 2, 3, 4, 5]

# Modify and convert back
my_list.append(6)
my_tuple = tuple(my_list)
print(my_tuple)  # (1, 2, 3, 4, 5, 6)
```

## Tuple Comprehensions

Python doesn't have tuple comprehensions directly, but you can create them using generator expressions:

```python
# This creates a generator, not a tuple
gen = (x**2 for x in range(1, 6))
print(type(gen))  # <class 'generator'>

# Convert to tuple
squares_tuple = tuple(x**2 for x in range(1, 6))
print(squares_tuple)  # (1, 4, 9, 16, 25)

# Or use a list comprehension and convert
even_squares = tuple(x**2 for x in range(1, 11) if x % 2 == 0)
print(even_squares)  # (4, 16, 36, 64, 100)
```

## Advanced Tuple Techniques

### Sorting Tuples

```python
# Sort by first element
data = [(3, 'c'), (1, 'a'), (2, 'b')]
sorted_data = sorted(data)
print(sorted_data)  # [(1, 'a'), (2, 'b'), (3, 'c')]

# Sort by second element
sorted_by_second = sorted(data, key=lambda x: x[1])
print(sorted_by_second)  # [(1, 'a'), (2, 'b'), (3, 'c')]

# Sort by length (for tuples of different sizes)
mixed = [(1, 2), (3, 4, 5), (6,)]
sorted_by_length = sorted(mixed, key=len)
print(sorted_by_length)  # [(6,), (1, 2), (3, 4, 5)]
```

### Tuple as Return Values in Recursive Functions

```python
def fibonacci(n):
    if n <= 1:
        return (n, 0)  # (current, previous)
    else:
        current, prev = fibonacci(n - 1)
        return (current + prev, current)

# Get nth Fibonacci number
result, _ = fibonacci(10)
print(result)  # 55
```

### Using Tuples with zip()

```python
names = ("Alice", "Bob", "Charlie")
ages = (30, 25, 35)
cities = ("New York", "San Francisco", "Chicago")

# Combine corresponding elements
people = list(zip(names, ages, cities))
print(people)  # [('Alice', 30, 'New York'), ('Bob', 25, 'San Francisco'), ('Charlie', 35, 'Chicago')]

# Unzip
names2, ages2, cities2 = zip(*people)
print(names2)   # ('Alice', 'Bob', 'Charlie')
print(ages2)    # (30, 25, 35)
print(cities2)  # ('New York', 'San Francisco', 'Chicago')
```

## Best Practices

1. Use tuples for immutable data collections
2. Use tuples as dictionary keys when you need composite keys
3. Use tuples to return multiple values from functions
4. Use tuples for data that shouldn't be modified
5. Consider named tuples for better code readability when dealing with structured data
6. Remember that tuples are immutable, so they're safe to use as dictionary keys or set elements
7. Use tuple unpacking to make your code more readable

## Performance Considerations

- Tuples are generally faster than lists for iteration
- Tuples use less memory than lists
- Tuples are hashable and can be used as dictionary keys and set elements
- For large collections of data that need frequent modification, use lists instead
