# Python Booleans

Booleans represent truth values: `True` or `False`. They are fundamental to decision-making in programming and are used in conditional statements, loops, and logical operations.

## Boolean Values

```python
# Boolean literals
is_active = True
is_deleted = False

print(type(is_active))  # <class 'bool'>
print(is_active)        # True
print(is_deleted)       # False
```

## Comparison Operators

Comparison operators return boolean values by comparing two values:

| Operator | Description              | Example         |
| -------- | ------------------------ | --------------- |
| `==`     | Equal to                 | `5 == 5` → True |
| `!=`     | Not equal to             | `5 != 3` → True |
| `>`      | Greater than             | `7 > 3` → True  |
| `<`      | Less than                | `3 < 7` → True  |
| `>=`     | Greater than or equal to | `5 >= 5` → True |
| `<=`     | Less than or equal to    | `3 <= 5` → True |

```python
a = 10
b = 5

print(a == b)   # False
print(a != b)   # True
print(a > b)    # True
print(a < b)    # False
print(a >= b)   # True
print(a <= b)   # False
```

## Logical Operators

Logical operators combine boolean values:

| Operator | Description | Example                  |
| -------- | ----------- | ------------------------ |
| `and`    | Logical AND | `True and False` → False |
| `or`     | Logical OR  | `True or False` → True   |
| `not`    | Logical NOT | `not True` → False       |

### AND Operator (`and`)

Returns `True` only if both operands are `True`:

```python
print(True and True)    # True
print(True and False)   # False
print(False and True)   # False
print(False and False)  # False

# Practical example
age = 25
has_license = True
can_drive = age >= 18 and has_license
print(can_drive)  # True
```

### OR Operator (`or`)

Returns `True` if at least one operand is `True`:

```python
print(True or True)     # True
print(True or False)    # True
print(False or True)    # True
print(False or False)   # False

# Practical example
is_weekend = False
is_holiday = True
can_sleep_in = is_weekend or is_holiday
print(can_sleep_in)  # True
```

### NOT Operator (`not`)

Reverses the boolean value:

```python
print(not True)   # False
print(not False)  # True

# Practical example
is_raining = False
should_take_umbrella = not is_raining
print(should_take_umbrella)  # True
```

## Truth Tables

### AND Truth Table

| A     | B     | A and B |
| ----- | ----- | ------- |
| True  | True  | True    |
| True  | False | False   |
| False | True  | False   |
| False | False | False   |

### OR Truth Table

| A     | B     | A or B |
| ----- | ----- | ------ |
| True  | True  | True   |
| True  | False | True   |
| False | True  | True   |
| False | False | False  |

### NOT Truth Table

| A     | not A |
| ----- | ----- |
| True  | False |
| False | True  |

## Truthy and Falsy Values

In Python, all values have an inherent boolean value. Values that evaluate to `False` are called "falsy", and values that evaluate to `True` are called "truthy".

### Falsy Values

```python
# These all evaluate to False
falsy_values = [
    False,      # Boolean False
    None,       # None type
    0,          # Zero integer
    0.0,        # Zero float
    "",         # Empty string
    [],         # Empty list
    {},         # Empty dictionary
    (),         # Empty tuple
    set(),      # Empty set
]

for value in falsy_values:
    print(f"{value} is falsy: {not bool(value)}")
```

### Truthy Values

All other values are truthy:

```python
# These all evaluate to True
truthy_values = [
    True,       # Boolean True
    1,          # Non-zero integer
    -1,         # Negative non-zero integer
    3.14,       # Non-zero float
    "hello",    # Non-empty string
    [1, 2, 3],  # Non-empty list
    {"key": "value"},  # Non-empty dictionary
    (1, 2),     # Non-empty tuple
    {1, 2, 3},  # Non-empty set
]

for value in truthy_values:
    print(f"{value} is truthy: {bool(value)}")
```

## Short-Circuit Evaluation

Python uses short-circuit evaluation for logical operators:

```python
# AND short-circuit: stops at first False
def check_first():
    print("Checking first condition")
    return False

def check_second():
    print("Checking second condition")
    return True

result = check_first() and check_second()
print(f"Result: {result}")
# Output: Checking first condition, Result: False
# (check_second() is never called)

# OR short-circuit: stops at first True
def check_third():
    print("Checking third condition")
    return True

def check_fourth():
    print("Checking fourth condition")
    return False

result = check_third() or check_fourth()
print(f"Result: {result}")
# Output: Checking third condition, Result: True
# (check_fourth() is never called)
```

## Boolean Conversion

You can explicitly convert values to boolean using `bool()`:

```python
print(bool(0))        # False
print(bool(1))        # True
print(bool(""))       # False
print(bool("hello"))  # True
print(bool([]))       # False
print(bool([1, 2]))   # True
```

## Chaining Comparisons

Python allows chaining comparison operators:

```python
x = 5

# Instead of: x >= 0 and x <= 10
print(0 <= x <= 10)  # True

# Multiple comparisons
y = 7
print(1 < x < y < 10)  # True

# With different operators
z = 8
print(x < y and y < z)  # True
print(x < y < z)        # True (equivalent to above)
```

## Boolean in Conditional Statements

Booleans are commonly used in conditional statements:

```python
age = 20
has_ticket = True

if age >= 18 and has_ticket:
    print("You can enter the concert!")
else:
    print("Sorry, you cannot enter.")

# Using truthy/falsy values
name = ""
if not name:
    print("Name is required!")
else:
    print(f"Hello, {name}!")
```

## Identity vs Equality

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)  # True (values are equal)
print(a is b)  # False (different objects in memory)
print(a is c)  # True (same object in memory)

# For simple values
x = 5
y = 5
print(x == y)  # True
print(x is y)  # True (integers -5 to 256 are cached)
```

## Boolean Methods

Some useful boolean-related methods:

```python
# any() - returns True if any element is truthy
numbers = [0, 1, 2, 3]
print(any(numbers))  # True

empty_list = []
print(any(empty_list))  # False

# all() - returns True if all elements are truthy
numbers = [1, 2, 3, 4]
print(all(numbers))  # True

mixed = [0, 1, 2]
print(all(mixed))  # False
```
