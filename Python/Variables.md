# Python Variables and Data Types

Variables are containers for storing data values. In Python, you don't need to declare variables with a specific type - Python automatically determines the type based on the value assigned.

## Variable Declaration

```python
# Variable assignment
name = "Alice"      # String
age = 30           # Integer
height = 5.7       # Float
is_student = True  # Boolean

# Multiple assignment
x, y, z = 1, 2, 3

# Same value to multiple variables
a = b = c = 10
```

## Data Types

Python has several built-in data types:

### Numeric Types

- **int**: Integer numbers (e.g., 5, -3, 1000)
- **float**: Floating-point numbers (e.g., 3.14, -2.5, 1.0)
- **complex**: Complex numbers (e.g., 2+3j)

### Text Type

- **str**: String values (e.g., "Hello", 'World')

### Boolean Type

- **bool**: Boolean values (True or False)

### Sequence Types

- **list**: Ordered, mutable collection
- **tuple**: Ordered, immutable collection
- **range**: Sequence of numbers

### Mapping Type

- **dict**: Key-value pairs

### Set Types

- **set**: Unordered, unique elements
- **frozenset**: Immutable set

### None Type

- **NoneType**: Represents the absence of a value

## Type Checking

You can check the type of a variable using the `type()` function:

```python
name = "Alice"
age = 30
height = 5.7

print(type(name))    # <class 'str'>
print(type(age))     # <class 'int'>
print(type(height))  # <class 'float'>
```

## Type Conversion

You can convert between different data types:

```python
# String to integer
num_str = "123"
num_int = int(num_str)
print(num_int)  # 123

# Integer to string
num = 456
num_str = str(num)
print(num_str)  # "456"

# Float to integer (truncates decimal)
float_num = 3.9
int_num = int(float_num)
print(int_num)  # 3

# Integer to float
int_num = 5
float_num = float(int_num)
print(float_num)  # 5.0
```

## Variable Naming Rules

- Must start with a letter or underscore (\_)
- Can contain letters, numbers, and underscores
- Case-sensitive (age, Age, AGE are different variables)
- Cannot be Python keywords

```python
# Valid variable names
name = "John"
_age = 25
user_name = "alice"
count1 = 10

# Invalid variable names (would cause syntax errors)
# 1name = "invalid"
# for = "invalid"
# class = "invalid"
```

## Constants

Python doesn't have built-in constant types, but you can indicate constants by using all uppercase names:

```python
PI = 3.14159
MAX_CONNECTIONS = 100
DATABASE_URL = "localhost:5432"
```

## Dynamic Typing

Python is dynamically typed, meaning you can reassign variables to different types:

```python
# This is perfectly valid in Python
x = 5         # x is an integer
x = "hello"   # now x is a string
x = [1, 2, 3] # now x is a list
```

## Checking Variable Existence

You can check if a variable exists using the `globals()` or `locals()` functions:

```python
if 'my_variable' in globals():
    print("my_variable exists")
else:
    print("my_variable does not exist")
```
