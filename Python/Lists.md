# Python Lists

Lists are ordered, mutable collections of items. They can contain items of different data types and are one of the most versatile data structures in Python.

## Creating Lists

```python
# Empty list
empty_list = []

# List with items
fruits = ["apple", "banana", "orange"]

# Mixed data types
mixed = [1, "hello", 3.14, True]

# Using list() constructor
numbers = list((1, 2, 3, 4, 5))

# List with repeated values
zeros = [0] * 5  # [0, 0, 0, 0, 0]

# Nested lists
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

print(matrix[0][1])  # 2
```

## Accessing List Elements

```python
fruits = ["apple", "banana", "orange", "grape"]

# Positive indexing
print(fruits[0])  # "apple"
print(fruits[2])  # "orange"

# Negative indexing
print(fruits[-1])  # "grape" (last element)
print(fruits[-2])  # "orange" (second to last)

# Slicing
print(fruits[1:3])  # ["banana", "orange"]
print(fruits[:2])   # ["apple", "banana"]
print(fruits[2:])   # ["orange", "grape"]
print(fruits[::2])  # ["apple", "orange"] (every second element)
```

## Modifying Lists

```python
fruits = ["apple", "banana", "orange"]

# Change element
fruits[1] = "blueberry"
print(fruits)  # ["apple", "blueberry", "orange"]

# Change multiple elements with slicing
fruits[1:3] = ["cherry", "date"]
print(fruits)  # ["apple", "cherry", "date"]
```

## List Methods

### Adding Elements

```python
fruits = ["apple", "banana"]

# append() - add single element at the end
fruits.append("orange")
print(fruits)  # ["apple", "banana", "orange"]

# extend() - add multiple elements
fruits.extend(["grape", "kiwi"])
print(fruits)  # ["apple", "banana", "orange", "grape", "kiwi"]

# insert() - add element at specific position
fruits.insert(1, "apricot")
print(fruits)  # ["apple", "apricot", "banana", "orange", "grape", "kiwi"]
```

### Removing Elements

```python
fruits = ["apple", "banana", "orange", "banana", "grape"]

# remove() - remove first occurrence of value
fruits.remove("banana")
print(fruits)  # ["apple", "orange", "banana", "grape"]

# pop() - remove and return element at index (default: last)
last_fruit = fruits.pop()
print(last_fruit)  # "grape"
print(fruits)      # ["apple", "orange", "banana"]

# pop with index
second_fruit = fruits.pop(1)
print(second_fruit)  # "orange"
print(fruits)        # ["apple", "banana"]

# del statement - remove by index or slice
del fruits[0]
print(fruits)  # ["banana"]

# clear() - remove all elements
fruits.clear()
print(fruits)  # []
```

### Other Useful Methods

```python
fruits = ["apple", "banana", "orange", "grape"]

# index() - find index of first occurrence
print(fruits.index("banana"))  # 1

# count() - count occurrences
fruits = ["apple", "banana", "orange", "banana"]
print(fruits.count("banana"))  # 2

# sort() - sort list in place
numbers = [3, 1, 4, 1, 5]
numbers.sort()
print(numbers)  # [1, 1, 3, 4, 5]

# reverse() - reverse list in place
fruits = ["apple", "banana", "orange"]
fruits.reverse()
print(fruits)  # ["orange", "banana", "apple"]

# copy() - create shallow copy
original = [1, 2, 3]
copy_list = original.copy()
copy_list.append(4)
print(original)  # [1, 2, 3]
print(copy_list)  # [1, 2, 3, 4]
```

## List Operations

### Concatenation

```python
list1 = [1, 2, 3]
list2 = [4, 5, 6]

# Using + operator
combined = list1 + list2
print(combined)  # [1, 2, 3, 4, 5, 6]

# Using extend()
list1.extend(list2)
print(list1)  # [1, 2, 3, 4, 5, 6]
```

### Repetition

```python
numbers = [1, 2, 3]
repeated = numbers * 3
print(repeated)  # [1, 2, 3, 1, 2, 3, 1, 2, 3]
```

### Membership Testing

```python
fruits = ["apple", "banana", "orange"]

print("apple" in fruits)     # True
print("grape" in fruits)     # False
print("grape" not in fruits) # True
```

## List Comprehensions

List comprehensions provide a concise way to create lists.

### Basic Syntax

```python
# Traditional approach
squares = []
for x in range(1, 6):
    squares.append(x ** 2)
print(squares)  # [1, 4, 9, 16, 25]

# List comprehension
squares = [x ** 2 for x in range(1, 6)]
print(squares)  # [1, 4, 9, 16, 25]
```

### With Conditional Filtering

```python
# Filter even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_numbers = [x for x in numbers if x % 2 == 0]
print(even_numbers)  # [2, 4, 6, 8, 10]

# Filter and transform
result = [x * 2 for x in numbers if x > 5]
print(result)  # [12, 14, 16, 18, 20]
```

### Nested List Comprehensions

```python
# Flatten a matrix
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [x for row in matrix for x in row]
print(flattened)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Create a multiplication table
table = [[i * j for j in range(1, 4)] for i in range(1, 4)]
print(table)  # [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

### Conditional Expressions in Comprehensions

```python
numbers = [1, 2, 3, 4, 5, 6]
result = [x if x % 2 == 0 else x * 2 for x in numbers]
print(result)  # [2, 2, 6, 4, 10, 6]
```

## Iterating Over Lists

### Using for loops

```python
fruits = ["apple", "banana", "orange"]

for fruit in fruits:
    print(fruit)

# With index
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
```

### Using while loops

```python
fruits = ["apple", "banana", "orange"]
i = 0
while i < len(fruits):
    print(fruits[i])
    i += 1
```

## List Functions

### Built-in Functions

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

print(len(numbers))   # 8 (length)
print(max(numbers))   # 9 (maximum)
print(min(numbers))   # 1 (minimum)
print(sum(numbers))   # 31 (sum)
print(sorted(numbers))  # [1, 1, 2, 3, 4, 5, 6, 9] (sorted copy)
```

### Using with other functions

```python
# Convert strings to uppercase
fruits = ["apple", "banana", "orange"]
upper_fruits = list(map(str.upper, fruits))
print(upper_fruits)  # ['APPLE', 'BANANA', 'ORANGE']

# Filter strings longer than 5 characters
long_fruits = list(filter(lambda x: len(x) > 5, fruits))
print(long_fruits)  # ['banana', 'orange']

# Reduce to concatenate strings
from functools import reduce
result = reduce(lambda x, y: x + ", " + y, fruits)
print(result)  # "apple, banana, orange"
```

## Common List Patterns

### Finding elements

```python
numbers = [1, 2, 3, 4, 5, 2, 3]

# Find first occurrence
print(numbers.index(2))  # 1

# Find all occurrences
indices = [i for i, x in enumerate(numbers) if x == 2]
print(indices)  # [1, 5]

# Check if element exists
print(2 in numbers)  # True
```

### Removing duplicates

```python
numbers = [1, 2, 2, 3, 3, 3, 4, 5]

# Using set
unique_numbers = list(set(numbers))
print(unique_numbers)  # [1, 2, 3, 4, 5] (order may change)

# Preserving order
unique_ordered = []
for num in numbers:
    if num not in unique_ordered:
        unique_ordered.append(num)
print(unique_ordered)  # [1, 2, 3, 4, 5]
```

### List of lists operations

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# Transpose matrix
transpose = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
print(transpose)  # [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

# Sum of each row
row_sums = [sum(row) for row in matrix]
print(row_sums)  # [6, 15, 24]

# Sum of each column
col_sums = [sum(row[i] for row in matrix) for i in range(len(matrix[0]))]
print(col_sums)  # [12, 15, 18]
```

### Sorting complex lists

```python
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 92},
    {"name": "Charlie", "grade": 78}
]

# Sort by grade
sorted_students = sorted(students, key=lambda x: x["grade"])
print(sorted_students)
# [{'name': 'Charlie', 'grade': 78}, {'name': 'Alice', 'grade': 85}, {'name': 'Bob', 'grade': 92}]

# Sort by name
sorted_by_name = sorted(students, key=lambda x: x["name"])
print(sorted_by_name)
# [{'name': 'Alice', 'grade': 85}, {'name': 'Bob', 'grade': 92}, {'name': 'Charlie', 'grade': 78}]
```

## Memory Considerations

```python
# Lists are mutable and can be modified in place
original = [1, 2, 3]
modified = original
modified.append(4)
print(original)  # [1, 2, 3, 4] (original is also modified!)

# To avoid this, create a copy
original = [1, 2, 3]
copy_list = original.copy()
copy_list.append(4)
print(original)  # [1, 2, 3]
print(copy_list)  # [1, 2, 3, 4]
```

## Performance Tips

- Use `append()` for adding single elements
- Use `extend()` for adding multiple elements
- Use list comprehensions instead of loops when possible
- Use `in` operator for membership testing
- Consider using `collections.deque` for frequent append/popleft operations
- For large lists, consider using `numpy` arrays for numerical operations
