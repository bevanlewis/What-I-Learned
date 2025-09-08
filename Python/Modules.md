# Python Modules and Packages

Modules are files containing Python code that can be imported and used in other Python programs. Packages are directories containing multiple modules. This allows for code organization, reusability, and separation of concerns.

## Creating and Using Modules

### Creating a Module

Create a file called `mymodule.py`:

```python
# mymodule.py
def greet(name):
    return f"Hello, {name}!"

def add_numbers(a, b):
    return a + b

PI = 3.14159

class Calculator:
    def multiply(self, a, b):
        return a * b
```

### Importing a Module

```python
# Import the entire module
import mymodule

print(mymodule.greet("Alice"))        # "Hello, Alice!"
print(mymodule.add_numbers(5, 3))     # 8
print(mymodule.PI)                    # 3.14159

calc = mymodule.Calculator()
print(calc.multiply(4, 5))            # 20

# Import specific items
from mymodule import greet, PI

print(greet("Bob"))  # "Hello, Bob!"
print(PI)            # 3.14159

# Import with alias
import mymodule as mm

print(mm.greet("Charlie"))  # "Hello, Charlie!"

# Import specific items with aliases
from mymodule import greet as hello, add_numbers as add

print(hello("David"))  # "Hello, David!"
print(add(10, 7))     # 17
```

## Standard Library Modules

Python comes with a comprehensive standard library:

```python
# Math module
import math

print(math.sqrt(16))    # 4.0
print(math.pi)          # 3.141592653589793
print(math.sin(math.pi/2))  # 1.0

# Random module
import random

print(random.randint(1, 10))     # Random integer between 1 and 10
print(random.choice(['a', 'b', 'c']))  # Random choice from list
print(random.random())            # Random float between 0 and 1

# DateTime module
import datetime

now = datetime.datetime.now()
print(now)                        # Current date and time
print(now.year)                   # Current year
print(now.strftime("%Y-%m-%d"))   # Formatted date

# OS module
import os

print(os.getcwd())        # Current working directory
print(os.listdir())       # List files in current directory
print(os.path.exists("file.txt"))  # Check if file exists

# JSON module
import json

data = {"name": "Alice", "age": 30}
json_string = json.dumps(data)    # Convert to JSON string
print(json_string)                # '{"name": "Alice", "age": 30}'

parsed_data = json.loads(json_string)  # Parse JSON string
print(parsed_data["name"])        # "Alice"
```

## The import Statement

### Different Import Methods

```python
# 1. Import entire module
import module_name

# 2. Import specific items
from module_name import item1, item2

# 3. Import with alias
import module_name as alias

# 4. Import all items (not recommended)
from module_name import *

# 5. Conditional imports
try:
    import optional_module
except ImportError:
    optional_module = None
```

### Import Search Path

Python searches for modules in this order:

1. Built-in modules
2. Current directory
3. PYTHONPATH environment variable
4. Installation-dependent default path

```python
import sys
print(sys.path)  # List of directories Python searches for modules
```

## Creating Packages

Packages are directories containing multiple modules and an `__init__.py` file.

### Package Structure

```
mypackage/
├── __init__.py
├── module1.py
├── module2.py
└── subpackage/
    ├── __init__.py
    └── module3.py
```

### **init**.py Files

```python
# mypackage/__init__.py
from .module1 import function1
from .module2 import Class2

__version__ = "1.0.0"
__author__ = "Your Name"
```

### Using Packages

```python
# Import from package
from mypackage import module1
from mypackage.module1 import function1
from mypackage.subpackage import module3

# Import the package itself
import mypackage
print(mypackage.__version__)  # "1.0.0"
```

## Relative Imports

Within packages, you can use relative imports:

```python
# In mypackage/module1.py
from .module2 import some_function    # Relative import from same package
from ..subpackage.module3 import another_function  # Relative import from subpackage
```

## The **name** Variable

Every Python module has a `__name__` attribute:

```python
# module.py
def main():
    print("This is the main function")

if __name__ == "__main__":
    main()

# When run directly: __name__ = "__main__"
# When imported: __name__ = "module"
```

## Common Standard Library Modules

### Collections Module

```python
from collections import Counter, defaultdict, namedtuple

# Counter
from collections import Counter
words = ["apple", "banana", "apple", "orange", "banana", "apple"]
word_count = Counter(words)
print(word_count)  # Counter({'apple': 3, 'banana': 2, 'orange': 1})

# defaultdict
from collections import defaultdict
fruit_colors = defaultdict(lambda: "unknown")
fruit_colors["apple"] = "red"
fruit_colors["banana"] = "yellow"
print(fruit_colors["grape"])  # "unknown"

# namedtuple
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print(p.x, p.y)  # 1 2
```

### Itertools Module

```python
import itertools

# Infinite iterators
counter = itertools.count(1)
print(next(counter), next(counter), next(counter))  # 1 2 3

# Cycling through values
colors = itertools.cycle(['red', 'green', 'blue'])
print(next(colors), next(colors), next(colors), next(colors))  # 'red' 'green' 'blue' 'red'

# Combinations and permutations
items = ['A', 'B', 'C']
print(list(itertools.combinations(items, 2)))   # [('A', 'B'), ('A', 'C'), ('B', 'C')]
print(list(itertools.permutations(items, 2)))   # [('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]

# Grouping
data = [('A', 1), ('A', 2), ('B', 3), ('B', 4)]
for key, group in itertools.groupby(data, lambda x: x[0]):
    print(key, list(group))
# A [('A', 1), ('A', 2)]
# B [('B', 3), ('B', 4)]
```

### Functools Module

```python
import functools

# Partial functions
from functools import partial

def multiply(x, y):
    return x * y

double = partial(multiply, 2)
triple = partial(multiply, 3)

print(double(5))  # 10
print(triple(5))  # 15

# Reduce function
from functools import reduce

numbers = [1, 2, 3, 4, 5]
product = reduce(lambda x, y: x * y, numbers)
print(product)  # 120

# Lru_cache decorator
from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(100))  # Fast due to caching
```

## Installing Third-Party Packages

### Using pip

```bash
# Install a package
pip install requests

# Install specific version
pip install requests==2.25.1

# Install from requirements file
pip install -r requirements.txt

# List installed packages
pip list

# Show package information
pip show requests
```

### Using Installed Packages

```python
import requests

response = requests.get('https://api.github.com/user', auth=('user', 'pass'))
print(response.status_code)
print(response.json())
```

## Virtual Environments

Virtual environments allow you to create isolated Python environments:

```bash
# Create virtual environment
python -m venv myenv

# Activate virtual environment (Linux/Mac)
source myenv/bin/activate

# Activate virtual environment (Windows)
myenv\Scripts\activate

# Install packages in virtual environment
pip install requests

# Deactivate
deactivate
```

## Best Practices for Modules and Packages

### 1. Use clear, descriptive names

```python
# Good
import data_processing
from utils import format_date

# Avoid
import dp
from u import fd
```

### 2. Organize imports properly

```python
# Standard library imports
import os
import sys
from collections import defaultdict

# Third-party imports
import requests
from flask import Flask

# Local imports
from .utils import helper_function
from mypackage.module import MyClass
```

### 3. Use **init**.py files

```python
# mypackage/__init__.py
from .database import DatabaseConnection
from .api import APIClient

__version__ = "1.0.0"
__all__ = ["DatabaseConnection", "APIClient"]
```

### 4. Avoid circular imports

```python
# module_a.py
# from module_b import function_b  # This can cause circular import

def function_a():
    pass

# module_b.py
from module_a import function_a

def function_b():
    pass
```

### 5. Use relative imports within packages

```python
# In mypackage/subpackage/module.py
from ..utils import helper_function  # Relative import
from mypackage.utils import helper_function  # Absolute import (also works)
```

### 6. Document your modules

```python
"""
mypackage.utils
~~~~~~~~~~~~~~~

Utility functions for mypackage.

:copyright: (c) 2023 by Your Name
:license: MIT, see LICENSE for more details.
"""

def format_name(first, last):
    """Format a full name from first and last names."""
    return f"{first} {last}"
```

### 7. Use **all** to control public API

```python
# utils.py
def public_function():
    pass

def _private_function():
    pass

__all__ = ["public_function"]  # Only public_function will be imported with "from utils import *"
```

### 8. Handle import errors gracefully

```python
try:
    import optional_dependency
except ImportError:
    optional_dependency = None

def use_optional_feature():
    if optional_dependency is None:
        raise ImportError("optional_dependency is required for this feature")
    # Use optional_dependency
```

## Module Execution Context

### Running Modules Directly

```python
# script.py
def main():
    print("Running as main script")

def helper():
    print("Helper function")

if __name__ == "__main__":
    main()
```

### Module Loading and Execution

```python
# Python executes modules when imported
print("This runs when module is imported")

def function():
    print("This runs when function is called")

# This pattern prevents code from running on import
if __name__ == "__main__":
    print("This only runs when script is executed directly")
```

## Common Module Patterns

### Configuration Module

```python
# config.py
DATABASE_URL = "sqlite:///app.db"
SECRET_KEY = "your-secret-key"
DEBUG = True
```

```python
# main.py
from config import DATABASE_URL, DEBUG

if DEBUG:
    print("Running in debug mode")
```

### Factory Pattern with Modules

```python
# database/__init__.py
from .sqlite import SQLiteDatabase
from .postgresql import PostgreSQLDatabase

def create_database(db_type):
    if db_type == "sqlite":
        return SQLiteDatabase()
    elif db_type == "postgresql":
        return PostgreSQLDatabase()
    else:
        raise ValueError(f"Unsupported database type: {db_type}")
```

### Plugin System

```python
# plugins/__init__.py
import importlib
import pkgutil

def load_plugins():
    plugins = []
    for _, name, _ in pkgutil.iter_modules(__path__):
        module = importlib.import_module(f"{__name__}.{name}")
        if hasattr(module, "register"):
            plugins.append(module.register())
    return plugins
```

## Package Distribution

### Setup.py

```python
# setup.py
from setuptools import setup, find_packages

setup(
    name="mypackage",
    version="1.0.0",
    packages=find_packages(),
    install_requires=[
        "requests>=2.25.0",
        "flask>=2.0.0",
    ],
    entry_points={
        "console_scripts": [
            "mycommand=mypackage.cli:main",
        ],
    },
)
```

### Modern Setup with pyproject.toml

```toml
# pyproject.toml
[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "mypackage"
version = "1.0.0"
dependencies = [
    "requests>=2.25.0",
    "flask>=2.0.0",
]
```

This comprehensive guide covers the fundamentals of Python modules and packages, from basic imports to advanced package creation and distribution.
