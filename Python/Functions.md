# Python Functions

Functions are reusable blocks of code that perform specific tasks. They help organize code, make it more readable, and avoid repetition. Functions can take inputs (parameters) and return outputs.

## Defining Functions

```python
# Basic function definition
def greet():
    print("Hello, World!")

# Function with parameters
def greet_person(name):
    print(f"Hello, {name}!")

# Function with return value
def add_numbers(a, b):
    return a + b

# Calling functions
greet()                    # "Hello, World!"
greet_person("Alice")      # "Hello, Alice!"
result = add_numbers(5, 3) # result = 8
```

## Function Parameters

### Positional Parameters

```python
def describe_person(name, age, city):
    print(f"{name} is {age} years old and lives in {city}.")

describe_person("Alice", 30, "New York")
# Output: Alice is 30 years old and lives in New York.
```

### Default Parameters

```python
def greet_person(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet_person("Alice")              # "Hello, Alice!"
greet_person("Bob", "Hi")          # "Hi, Bob!"
greet_person("Charlie", greeting="Hey")  # "Hey, Charlie!"
```

### Keyword Arguments

```python
def describe_person(name, age, city):
    print(f"{name} is {age} years old and lives in {city}.")

# Using keyword arguments
describe_person(name="Alice", age=30, city="New York")
describe_person(age=25, name="Bob", city="San Francisco")  # Order doesn't matter

# Mixing positional and keyword arguments
describe_person("Charlie", age=35, city="Chicago")
```

### Variable-Length Arguments

#### \*args (positional arguments)

```python
def sum_all(*args):
    total = 0
    for num in args:
        total += num
    return total

print(sum_all(1, 2, 3))        # 6
print(sum_all(1, 2, 3, 4, 5))  # 15
print(sum_all())               # 0

# Unpacking arguments
numbers = [1, 2, 3, 4, 5]
print(sum_all(*numbers))       # 15
```

#### \*\*kwargs (keyword arguments)

```python
def print_person_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_person_info(name="Alice", age=30, city="New York")
# Output:
# name: Alice
# age: 30
# city: New York

# Unpacking keyword arguments
person = {"name": "Bob", "age": 25, "city": "San Francisco"}
print_person_info(**person)
```

### Combining Different Parameter Types

```python
def complex_function(a, b=10, *args, **kwargs):
    print(f"a: {a}")
    print(f"b: {b}")
    print(f"args: {args}")
    print(f"kwargs: {kwargs}")

complex_function(1, 2, 3, 4, 5, x=6, y=7, z=8)
# Output:
# a: 1
# b: 2
# args: (3, 4, 5)
# kwargs: {'x': 6, 'y': 7, 'z': 8}
```

## Return Values

### Single Return Value

```python
def square(x):
    return x ** 2

result = square(5)
print(result)  # 25
```

### Multiple Return Values

```python
def get_user_info():
    name = "Alice"
    age = 30
    email = "alice@example.com"
    return name, age, email

# Unpacking return values
user_name, user_age, user_email = get_user_info()
print(user_name, user_age, user_email)  # Alice 30 alice@example.com

# Using as tuple
user_info = get_user_info()
print(user_info)  # ('Alice', 30, 'alice@example.com')
```

### Early Returns

```python
def divide(a, b):
    if b == 0:
        return "Error: Division by zero"
    return a / b

print(divide(10, 2))  # 5.0
print(divide(10, 0))  # "Error: Division by zero"
```

### Returning None

```python
def print_message(message):
    print(message)
    # Functions without explicit return return None

result = print_message("Hello")
print(result)  # None
```

## Function Scope and Variables

### Local vs Global Scope

```python
global_var = "I'm global"

def my_function():
    local_var = "I'm local"
    print(global_var)  # Can access global variables
    print(local_var)   # Can access local variables

my_function()
# print(local_var)  # NameError: local_var is not defined outside the function
print(global_var)     # "I'm global"
```

### Modifying Global Variables

```python
counter = 0

def increment_counter():
    global counter
    counter += 1

increment_counter()
print(counter)  # 1

# Better approach: Use return values
def increment_counter_better(current_count):
    return current_count + 1

counter = increment_counter_better(counter)
print(counter)  # 2
```

### nonlocal Keyword

```python
def outer_function():
    outer_var = "outer"

    def inner_function():
        nonlocal outer_var
        outer_var = "modified by inner"

    inner_function()
    print(outer_var)  # "modified by inner"

outer_function()
```

## Lambda Functions

Lambda functions are anonymous, single-expression functions:

```python
# Regular function
def add(x, y):
    return x + y

# Lambda function
add_lambda = lambda x, y: x + y

print(add(5, 3))        # 8
print(add_lambda(5, 3)) # 8

# Lambda in sorting
students = [("Alice", 85), ("Bob", 92), ("Charlie", 78)]
sorted_students = sorted(students, key=lambda x: x[1])
print(sorted_students)  # [('Charlie', 78), ('Alice', 85), ('Bob', 92)]

# Lambda with map and filter
numbers = [1, 2, 3, 4, 5]
squares = list(map(lambda x: x**2, numbers))
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))

print(squares)      # [1, 4, 9, 16, 25]
print(even_numbers) # [2, 4]
```

## Function Annotations

```python
def greet_person(name: str, age: int) -> str:
    return f"Hello, {name}! You are {age} years old."

# Annotations are stored in __annotations__
print(greet_person.__annotations__)
# {'name': <class 'str'>, 'age': <class 'int'>, 'return': <class 'str'>}

# They don't affect function behavior
result = greet_person("Alice", 30)
print(result)  # "Hello, Alice! You are 30 years old."
```

## Docstrings

```python
def calculate_area(length: float, width: float) -> float:
    """
    Calculate the area of a rectangle.

    Args:
        length (float): The length of the rectangle
        width (float): The width of the rectangle

    Returns:
        float: The area of the rectangle

    Examples:
        >>> calculate_area(5, 3)
        15
    """
    return length * width

print(calculate_area.__doc__)
help(calculate_area)
```

## Function Objects

Functions are first-class objects in Python:

```python
def greet(name):
    return f"Hello, {name}!"

def farewell(name):
    return f"Goodbye, {name}!"

# Assigning functions to variables
say_hello = greet
print(say_hello("Alice"))  # "Hello, Alice!"

# Functions as parameters
def execute_function(func, name):
    return func(name)

print(execute_function(greet, "Bob"))     # "Hello, Bob!"
print(execute_function(farewell, "Bob"))  # "Goodbye, Bob!"

# Functions in data structures
functions = [greet, farewell]
for func in functions:
    print(func("Charlie"))
```

## Decorators

Decorators modify the behavior of functions:

```python
def uppercase_decorator(func):
    def wrapper(name):
        result = func(name)
        return result.upper()
    return wrapper

@uppercase_decorator
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))  # "HELLO, ALICE!"

# Manual decoration
def farewell(name):
    return f"Goodbye, {name}!"

decorated_farewell = uppercase_decorator(farewell)
print(decorated_farewell("Bob"))  # "GOODBYE, BOB!"
```

## Recursion

Functions can call themselves:

```python
def factorial(n):
    if n == 0 or n == 1:
        return 1
    return n * factorial(n - 1)

print(factorial(5))  # 120

# Fibonacci sequence
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(10))  # 55
```

## Higher-Order Functions

Functions that take other functions as arguments or return functions:

```python
def apply_operation(func, x, y):
    return func(x, y)

def add(x, y):
    return x + y

def multiply(x, y):
    return x * y

print(apply_operation(add, 5, 3))      # 8
print(apply_operation(multiply, 5, 3)) # 15

# Function factory
def create_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

double = create_multiplier(2)
triple = create_multiplier(3)

print(double(5))  # 10
print(triple(5))  # 15
```

## Best Practices

### 1. Use descriptive names

```python
# Good
def calculate_total_price(items, tax_rate):
    # ...

# Avoid
def calc(it, tr):
    # ...
```

### 2. Keep functions focused

```python
# Good: Single responsibility
def validate_email(email):
    # Check email format
    pass

def send_email(email, message):
    # Send email
    pass

# Avoid: Multiple responsibilities
def process_email(email, message):
    # Validate and send email
    pass
```

### 3. Use default parameters wisely

```python
# Good
def create_user(name, email, active=True):
    # ...

# Avoid
def create_user(name=None, email=None):
    # ...
```

### 4. Handle edge cases

```python
def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

### 5. Use type hints (optional but recommended)

```python
from typing import List, Optional

def find_user(user_id: int) -> Optional[dict]:
    # ...
    pass

def process_items(items: List[str]) -> int:
    # ...
    pass
```

### 6. Document your functions

```python
def complex_calculation(data: List[float], threshold: float = 0.5) -> dict:
    """
    Perform complex calculations on data.

    Args:
        data: List of numerical values
        threshold: Cutoff value for filtering

    Returns:
        Dictionary with calculation results
    """
    # Implementation
    pass
```

### 7. Avoid mutable default arguments

```python
# Avoid
def append_item(item, my_list=[]):
    my_list.append(item)
    return my_list

# Good
def append_item(item, my_list=None):
    if my_list is None:
        my_list = []
    my_list.append(item)
    return my_list
```

### 8. Use \*args and \*\*kwargs for flexibility

```python
def flexible_function(*args, **kwargs):
    # Can handle any number of arguments
    pass
```

## Common Function Patterns

### Factory Pattern

```python
def create_greeter(greeting):
    def greeter(name):
        return f"{greeting}, {name}!"
    return greeter

say_hello = create_greeter("Hello")
say_hi = create_greeter("Hi")

print(say_hello("Alice"))  # "Hello, Alice!"
print(say_hi("Bob"))       # "Hi, Bob!"
```

### Callback Pattern

```python
def process_data(data, callback):
    result = []
    for item in data:
        processed = callback(item)
        result.append(processed)
    return result

def double(x):
    return x * 2

def square(x):
    return x ** 2

numbers = [1, 2, 3, 4, 5]
doubled = process_data(numbers, double)
squared = process_data(numbers, square)

print(doubled)  # [2, 4, 6, 8, 10]
print(squared)  # [1, 4, 9, 16, 25]
```

### Memoization

```python
def memoize(func):
    cache = {}

    def wrapper(*args):
        if args in cache:
            return cache[args]
        result = func(*args)
        cache[args] = result
        return result

    return wrapper

@memoize
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(100))  # Much faster due to caching
```
