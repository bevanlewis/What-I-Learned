# Python Loops

Loops are used to execute a block of code repeatedly. Python provides two main types of loops: `for` loops and `while` loops. Loops can be controlled using `break`, `continue`, and `else` statements.

## For Loops

For loops iterate over a sequence (such as a list, tuple, string, or range) or other iterable objects.

### Basic For Loop

```python
# Iterating over a list
fruits = ["apple", "banana", "orange", "grape"]

for fruit in fruits:
    print(fruit)

# Iterating over a string
for char in "Hello":
    print(char)

# Iterating over a range
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4
```

### Using range()

The `range()` function generates a sequence of numbers:

```python
# range(stop)
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# range(start, stop)
for i in range(2, 8):
    print(i)  # 2, 3, 4, 5, 6, 7

# range(start, stop, step)
for i in range(1, 10, 2):
    print(i)  # 1, 3, 5, 7, 9

# Negative step
for i in range(10, 0, -1):
    print(i)  # 10, 9, 8, 7, 6, 5, 4, 3, 2, 1
```

### Iterating with Index

```python
fruits = ["apple", "banana", "orange"]

# Using enumerate()
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# Manual indexing
for i in range(len(fruits)):
    print(f"{i}: {fruits[i]}")
```

### Iterating Over Dictionaries

```python
person = {"name": "Alice", "age": 30, "city": "New York"}

# Iterate over keys
for key in person:
    print(f"{key}: {person[key]}")

# Iterate over keys explicitly
for key in person.keys():
    print(key)

# Iterate over values
for value in person.values():
    print(value)

# Iterate over key-value pairs
for key, value in person.items():
    print(f"{key}: {value}")
```

## While Loops

While loops execute as long as a condition is `True`.

### Basic While Loop

```python
count = 0

while count < 5:
    print(count)
    count += 1

print("Loop finished")
```

### Infinite Loops

```python
# Be careful with infinite loops!
# while True:
#     print("This will run forever!")

# Controlled infinite loop with break
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input == 'quit':
        break
    print(f"You entered: {user_input}")
```

### While with Else

```python
count = 0

while count < 3:
    print(count)
    count += 1
else:
    print("While loop completed normally")

# Else block won't execute if loop is terminated with break
count = 0
while count < 5:
    if count == 3:
        break
    print(count)
    count += 1
else:
    print("This won't be printed")
```

## Control Statements

### Break Statement

The `break` statement exits the loop prematurely:

```python
# Break in for loop
for i in range(10):
    if i == 5:
        break
    print(i)  # 0, 1, 2, 3, 4

# Break in while loop
count = 0
while True:
    print(count)
    count += 1
    if count >= 5:
        break
```

### Continue Statement

The `continue` statement skips the rest of the current iteration and moves to the next one:

```python
# Skip even numbers
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)  # 1, 3, 5, 7, 9

# Skip specific values
fruits = ["apple", "banana", "orange", "grape"]
for fruit in fruits:
    if fruit == "banana":
        continue
    print(fruit)  # "apple", "orange", "grape"
```

### Pass Statement

The `pass` statement is a placeholder that does nothing:

```python
# Used when you need a statement syntactically but don't want to do anything
for i in range(5):
    if i % 2 == 0:
        pass  # TODO: handle even numbers later
    else:
        print(i)
```

## Nested Loops

```python
# Multiplication table
for i in range(1, 4):
    for j in range(1, 4):
        print(f"{i} * {j} = {i * j}")
    print()  # Empty line between rows

# Breaking out of nested loops
for i in range(3):
    for j in range(3):
        if i == 1 and j == 1:
            break
        print(f"i={i}, j={j}")
```

## List Comprehensions as Loop Alternatives

```python
# Traditional loop
numbers = [1, 2, 3, 4, 5]
squares = []
for num in numbers:
    squares.append(num ** 2)
print(squares)  # [1, 4, 9, 16, 25]

# List comprehension
squares = [num ** 2 for num in numbers]
print(squares)  # [1, 4, 9, 16, 25]

# With condition
even_squares = [num ** 2 for num in numbers if num % 2 == 0]
print(even_squares)  # [4, 16]
```

## Iterating Over Multiple Sequences

### Using zip()

```python
names = ["Alice", "Bob", "Charlie"]
ages = [30, 25, 35]
cities = ["New York", "San Francisco", "Chicago"]

for name, age, city in zip(names, ages, cities):
    print(f"{name} is {age} years old and lives in {city}")

# Handling sequences of different lengths
names = ["Alice", "Bob"]
ages = [30, 25, 35]  # Longer sequence

for name, age in zip(names, ages):
    print(f"{name}: {age}")  # Only prints Alice and Bob
```

### Using enumerate() with Multiple Items

```python
fruits = ["apple", "banana", "orange"]
for index, fruit in enumerate(fruits, start=1):
    print(f"{index}. {fruit}")
```

## Loop Patterns and Best Practices

### Pattern: Looping with Index

```python
# When you need both index and value
items = ["a", "b", "c", "d"]

# Good: Using enumerate
for index, item in enumerate(items):
    print(f"Index {index}: {item}")

# Avoid: Manual indexing
for i in range(len(items)):
    print(f"Index {i}: {items[i]}")
```

### Pattern: Finding Items

```python
numbers = [1, 3, 5, 7, 9, 2, 4, 6, 8, 10]

# Find first even number
for num in numbers:
    if num % 2 == 0:
        print(f"First even number: {num}")
        break

# Find all even numbers
even_numbers = []
for num in numbers:
    if num % 2 == 0:
        even_numbers.append(num)

print(even_numbers)
```

### Pattern: Accumulating Results

```python
numbers = [1, 2, 3, 4, 5]

# Sum using loop
total = 0
for num in numbers:
    total += num
print(total)  # 15

# Using built-in functions (preferred)
print(sum(numbers))  # 15
```

### Pattern: Transforming Data

```python
words = ["hello", "world", "python"]

# Convert to uppercase
upper_words = []
for word in words:
    upper_words.append(word.upper())

print(upper_words)  # ['HELLO', 'WORLD', 'PYTHON']

# Using list comprehension (preferred)
upper_words = [word.upper() for word in words]
print(upper_words)
```

## Advanced Loop Techniques

### Iterating Over Files

```python
# Reading lines from a file
with open('example.txt', 'r') as file:
    for line in file:
        print(line.strip())
```

### Using itertools for Advanced Iteration

```python
import itertools

# Infinite counter
for i in itertools.count(1):
    if i > 5:
        break
    print(i)

# Cycling through values
for item in itertools.cycle(['A', 'B', 'C']):
    print(item)
    if some_condition:
        break

# Combinations
items = ['A', 'B', 'C']
for combo in itertools.combinations(items, 2):
    print(combo)  # ('A', 'B'), ('A', 'C'), ('B', 'C')
```

### Generator Expressions

```python
# Memory-efficient for large datasets
numbers = range(1000000)

# List comprehension (creates entire list in memory)
squares_list = [x**2 for x in numbers]  # Uses lots of memory

# Generator expression (creates values on-demand)
squares_gen = (x**2 for x in numbers)   # Memory efficient

# Use generator in loop
for square in squares_gen:
    if square > 100:
        break
    print(square)
```

## Loop Performance Considerations

### Avoid Unnecessary Operations in Loops

```python
# Inefficient: Calling len() in each iteration
items = [1, 2, 3, 4, 5]
for i in range(len(items)):  # len() called every iteration
    print(items[i])

# Better: Calculate length once
length = len(items)
for i in range(length):
    print(items[i])

# Best: Iterate directly
for item in items:
    print(item)
```

### Use Appropriate Loop Type

```python
# For known sequences, use for loops
names = ["Alice", "Bob", "Charlie"]
for name in names:
    print(f"Hello, {name}")

# For conditional iteration, use while loops
count = 0
while count < 10 and some_condition():
    count += 1
```

## Common Loop Mistakes

### Modifying List While Iterating

```python
# Dangerous: Modifying list while iterating over it
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # This can cause items to be skipped!

print(numbers)  # [1, 3, 5] - 2 and 4 are missing!

# Safe: Iterate over a copy
numbers = [1, 2, 3, 4, 5]
for num in numbers[:]:  # Iterate over a copy
    if num % 2 == 0:
        numbers.remove(num)

print(numbers)  # [1, 3, 5]
```

### Off-by-One Errors

```python
# Common mistake: Off-by-one error
items = [1, 2, 3, 4, 5]

# Wrong: Will cause IndexError
# for i in range(len(items) + 1):
#     print(items[i])

# Correct
for i in range(len(items)):
    print(items[i])
```

### Infinite Loops

```python
# Accidental infinite loop
count = 0
while count < 10:
    print(count)
    # Forgot to increment!
    # count += 1

# Fix
while count < 10:
    print(count)
    count += 1
```

## Loop Else Clause

The `else` clause in loops executes when the loop completes normally (without `break`):

```python
# Search for an item
numbers = [1, 3, 5, 7, 9]
target = 4

for num in numbers:
    if num == target:
        print(f"Found {target}")
        break
else:
    print(f"{target} not found")

# Else executes only if break didn't occur
target = 5
for num in numbers:
    if num == target:
        print(f"Found {target}")
        break
else:
    print(f"{target} not found")  # This won't execute
```

## Best Practices

1. Use `for` loops when you know the number of iterations
2. Use `while` loops when the number of iterations is unknown
3. Prefer `enumerate()` when you need both index and value
4. Use list comprehensions for simple transformations
5. Avoid modifying collections while iterating over them
6. Use meaningful variable names
7. Keep loop bodies short and focused
8. Use `break` and `continue` judiciously
9. Consider using built-in functions like `sum()`, `max()`, `min()` when appropriate
10. Test your loops with edge cases (empty collections, single items, etc.)
