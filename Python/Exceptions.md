# Python Exceptions and Error Handling

Exceptions are events that occur during program execution that disrupt the normal flow of instructions. Python provides robust exception handling mechanisms to gracefully handle errors and maintain program stability.

## Basic Exception Handling

### try...except Block

```python
try:
    # Code that might raise an exception
    result = 10 / 0
    print("This won't be printed")
except ZeroDivisionError:
    # Handle the specific exception
    print("Cannot divide by zero!")

print("Program continues...")
```

### Catching Multiple Exceptions

```python
try:
    # Code that might raise different exceptions
    num = int(input("Enter a number: "))
    result = 100 / num
    print(f"Result: {result}")
except ValueError:
    print("Please enter a valid number")
except ZeroDivisionError:
    print("Cannot divide by zero")
except Exception as e:
    print(f"An unexpected error occurred: {e}")

# Alternative: Catch multiple exceptions in one except block
try:
    num = int(input("Enter a number: "))
    result = 100 / num
except (ValueError, ZeroDivisionError) as e:
    print(f"Error: {e}")
```

## Exception Hierarchy

Python's built-in exceptions form a hierarchy:

```
BaseException
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception
    ├── ArithmeticError
    │   ├── FloatingPointError
    │   ├── OverflowError
    │   └── ZeroDivisionError
    ├── ImportError
    │   └── ModuleNotFoundError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── NameError
    ├── OSError
    │   ├── FileExistsError
    │   ├── FileNotFoundError
    │   ├── PermissionError
    │   └── TimeoutError
    ├── RuntimeError
    ├── TypeError
    ├── ValueError
    └── ...
```

### Catching Parent Exceptions

```python
# Catch ArithmeticError (parent of ZeroDivisionError)
try:
    result = 10 / 0
except ArithmeticError as e:
    print(f"Arithmetic error: {e}")

# Catch Exception (catches most common exceptions)
try:
    # Some risky operation
    pass
except Exception as e:
    print(f"An error occurred: {e}")
```

## The else and finally Clauses

### try...except...else

```python
try:
    num = int(input("Enter a number: "))
    result = 100 / num
except (ValueError, ZeroDivisionError) as e:
    print(f"Error: {e}")
else:
    # Executed only if no exception occurred
    print(f"Success! Result: {result}")
    print("No errors occurred in the try block")
```

### try...finally

```python
file = None
try:
    file = open('example.txt', 'r')
    content = file.read()
    print(content)
except FileNotFoundError:
    print("File not found")
finally:
    # Always executed, regardless of whether an exception occurred
    if file:
        file.close()
        print("File closed")
```

### Complete try...except...else...finally

```python
def divide_numbers(a, b):
    try:
        result = a / b
    except ZeroDivisionError:
        print("Cannot divide by zero")
        return None
    except TypeError:
        print("Both arguments must be numbers")
        return None
    else:
        print("Division successful")
        return result
    finally:
        print("Division operation completed")

print(divide_numbers(10, 2))   # Success case
print(divide_numbers(10, 0))   # Error case
```

## Raising Exceptions

### raise Statement

```python
def validate_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 150:
        raise ValueError("Age cannot be greater than 150")
    return age

try:
    validate_age(-5)
except ValueError as e:
    print(f"Validation error: {e}")

# Re-raising exceptions
try:
    validate_age(200)
except ValueError:
    print("Age validation failed")
    raise  # Re-raises the original exception
```

### Creating Custom Exceptions

```python
class InsufficientFundsError(Exception):
    """Raised when account balance is insufficient"""

    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        super().__init__(f"Insufficient funds: balance={balance}, required={amount}")

class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError(self.balance, amount)
        self.balance -= amount
        return self.balance

account = BankAccount(100)
try:
    account.withdraw(150)
except InsufficientFundsError as e:
    print(f"Transaction failed: {e}")
```

## Exception Chaining

```python
def process_data(data):
    try:
        # Some processing that might fail
        result = int(data)
        return result
    except ValueError as e:
        # Chain the exception with additional context
        raise RuntimeError("Failed to process data") from e

try:
    process_data("not_a_number")
except RuntimeError as e:
    print(f"Processing failed: {e}")
    print(f"Original cause: {e.__cause__}")
```

## Context Managers and Exceptions

```python
class DatabaseConnection:
    def __init__(self, db_name):
        self.db_name = db_name
        self.connection = None

    def __enter__(self):
        self.connection = f"Connected to {self.db_name}"
        print(self.connection)
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.connection:
            print(f"Disconnected from {self.db_name}")
        if exc_type:
            print(f"An error occurred: {exc_val}")
        return False  # Don't suppress the exception

# Usage
with DatabaseConnection("mydb") as db:
    # Simulate an error
    raise ValueError("Something went wrong")
```

## Built-in Exception Types

### Common Built-in Exceptions

```python
# ValueError - inappropriate value
try:
    int("abc")
except ValueError as e:
    print(f"ValueError: {e}")

# TypeError - operation on inappropriate type
try:
    "hello" + 5
except TypeError as e:
    print(f"TypeError: {e}")

# KeyError - dictionary key not found
try:
    my_dict = {"a": 1}
    print(my_dict["b"])
except KeyError as e:
    print(f"KeyError: {e}")

# IndexError - sequence index out of range
try:
    my_list = [1, 2, 3]
    print(my_list[5])
except IndexError as e:
    print(f"IndexError: {e}")

# FileNotFoundError - file not found
try:
    open("nonexistent_file.txt")
except FileNotFoundError as e:
    print(f"FileNotFoundError: {e}")

# ZeroDivisionError - division by zero
try:
    10 / 0
except ZeroDivisionError as e:
    print(f"ZeroDivisionError: {e}")

# AttributeError - attribute not found
try:
    my_list.length  # lists don't have 'length' attribute
except AttributeError as e:
    print(f"AttributeError: {e}")

# ImportError - module not found
try:
    import nonexistent_module
except ImportError as e:
    print(f"ImportError: {e}")
```

## Best Practices for Exception Handling

### 1. Be Specific with Exception Types

```python
# Good: Catch specific exceptions
try:
    file = open('data.txt', 'r')
    data = file.read()
    file.close()
except FileNotFoundError:
    print("File not found")
except PermissionError:
    print("Permission denied")

# Avoid: Catch all exceptions
try:
    # Some code
    pass
except Exception:  # Too broad
    pass
```

### 2. Use finally for Cleanup

```python
# Good: Use finally for cleanup
file = None
try:
    file = open('data.txt', 'r')
    content = file.read()
    # Process content
except IOError:
    print("File operation failed")
finally:
    if file:
        file.close()

# Better: Use context manager
try:
    with open('data.txt', 'r') as file:
        content = file.read()
        # Process content
except IOError:
    print("File operation failed")
# File automatically closed
```

### 3. Don't Suppress Exceptions

```python
# Avoid: Empty except blocks
try:
    risky_operation()
except Exception:
    pass  # Silent failure - hard to debug

# Good: Handle exceptions appropriately
try:
    risky_operation()
except ValueError as e:
    print(f"Invalid value: {e}")
    # Handle the error
except Exception as e:
    print(f"Unexpected error: {e}")
    raise  # Re-raise if you can't handle it
```

### 4. Use Custom Exceptions for Clarity

```python
class AuthenticationError(Exception):
    pass

class InsufficientPermissionsError(Exception):
    pass

def login(username, password):
    if not validate_credentials(username, password):
        raise AuthenticationError("Invalid username or password")
    if not check_permissions(username):
        raise InsufficientPermissionsError("User lacks required permissions")
    return True

try:
    login("user", "pass")
except AuthenticationError as e:
    print("Login failed: authentication error")
except InsufficientPermissionsError as e:
    print("Login failed: insufficient permissions")
```

### 5. Avoid Bare except Clauses

```python
# Avoid
try:
    # Code
    pass
except:  # Bare except
    pass

# Good
try:
    # Code
    pass
except Exception as e:
    print(f"Error: {e}")
```

### 6. Log Exceptions

```python
import logging

logging.basicConfig(level=logging.ERROR)

try:
    risky_operation()
except Exception as e:
    logging.error(f"An error occurred: {e}", exc_info=True)
    # Handle the error
    raise
```

### 7. Use Exception Chaining for Context

```python
def process_user_data(user_id):
    try:
        user = get_user(user_id)
        return process_data(user)
    except KeyError as e:
        raise ValueError(f"User {user_id} not found") from e
    except ValueError as e:
        raise RuntimeError("Failed to process user data") from e
```

## Exception Handling Patterns

### EAFP (Easier to Ask for Forgiveness than Permission)

```python
# EAFP style - try operation first
def get_value(dictionary, key, default=None):
    try:
        return dictionary[key]
    except KeyError:
        return default

# LBYL (Look Before You Leap) style - check first
def get_value_l byl(dictionary, key, default=None):
    if key in dictionary:
        return dictionary[key]
    return default

# EAFP is generally preferred in Python
```

### Retry Pattern

```python
import time
import random

def unreliable_operation():
    if random.random() < 0.7:  # 70% chance of failure
        raise ConnectionError("Network error")
    return "Success!"

def perform_with_retry(operation, max_retries=3, delay=1):
    for attempt in range(max_retries):
        try:
            return operation()
        except ConnectionError as e:
            if attempt == max_retries - 1:
                raise e
            print(f"Attempt {attempt + 1} failed, retrying in {delay} seconds...")
            time.sleep(delay)
            delay *= 2  # Exponential backoff

try:
    result = perform_with_retry(unreliable_operation)
    print(f"Result: {result}")
except ConnectionError as e:
    print(f"Operation failed after all retries: {e}")
```

### Resource Management Pattern

```python
class ResourceManager:
    def __init__(self, resource_name):
        self.resource_name = resource_name
        self.resource = None

    def __enter__(self):
        self.resource = f"Acquired {self.resource_name}"
        print(self.resource)
        return self.resource

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.resource:
            print(f"Released {self.resource_name}")
        return False

# Usage
with ResourceManager("database connection") as conn:
    # Use the resource
    if random.random() < 0.5:
        raise ValueError("Simulated error")
    print("Resource used successfully")
```

## Assertions

```python
def divide(a, b):
    assert b != 0, "Division by zero is not allowed"
    return a / b

# Assertions can be disabled with -O flag
# python -O script.py (optimizations enabled, assertions disabled)

try:
    divide(10, 0)
except AssertionError as e:
    print(f"Assertion failed: {e}")
```

## Warnings

```python
import warnings

def deprecated_function():
    warnings.warn("This function is deprecated", DeprecationWarning, stacklevel=2)
    return "old result"

# Usage
result = deprecated_function()  # Issues a warning
```

## Exception Handling in Different Contexts

### In Functions

```python
def safe_divide(a, b):
    """Safely divide two numbers."""
    try:
        return a / b
    except ZeroDivisionError:
        return float('inf')  # Return infinity
    except TypeError:
        raise ValueError("Both arguments must be numbers")

# Usage
print(safe_divide(10, 2))    # 5.0
print(safe_divide(10, 0))    # inf
```

### In Classes

```python
class Calculator:
    def __init__(self):
        self.memory = 0

    def divide(self, a, b):
        try:
            result = a / b
            self.memory = result
            return result
        except ZeroDivisionError:
            raise ValueError("Cannot divide by zero") from None
        except (TypeError, ValueError) as e:
            raise ValueError(f"Invalid input: {e}") from e

calc = Calculator()
try:
    result = calc.divide(10, 0)
except ValueError as e:
    print(f"Calculator error: {e}")
```

### In List Comprehensions

```python
numbers = ["1", "2", "abc", "4"]

# Handle exceptions in list comprehension
def safe_int_conversion():
    results = []
    for num in numbers:
        try:
            results.append(int(num))
        except ValueError:
            results.append(None)
    return results

print(safe_int_conversion())  # [1, 2, None, 4]
```

## Testing Exception Handling

```python
import pytest

def test_divide_by_zero():
    with pytest.raises(ZeroDivisionError):
        1 / 0

def test_custom_exception():
    account = BankAccount(100)
    with pytest.raises(InsufficientFundsError):
        account.withdraw(150)

# Using unittest
import unittest

class TestCalculator(unittest.TestCase):
    def test_divide_by_zero(self):
        with self.assertRaises(ZeroDivisionError):
            Calculator().divide(10, 0)

if __name__ == '__main__':
    unittest.main()
```

This comprehensive guide covers exception handling in Python, from basic try-except blocks to advanced patterns and best practices for robust error handling.
