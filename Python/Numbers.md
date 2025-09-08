# Python Numbers and Math

Python supports three types of numeric data: integers, floating-point numbers, and complex numbers.

## Numeric Types

### Integers (int)

Whole numbers, positive or negative, without decimals.

```python
# Integer examples
age = 25
temperature = -5
big_number = 1000000

print(type(age))  # <class 'int'>
```

### Floating-Point Numbers (float)

Numbers with decimal points or in exponential notation.

```python
# Float examples
pi = 3.14159
height = 5.9
scientific = 1.23e-4  # 0.000123

print(type(pi))  # <class 'float'>
```

### Complex Numbers (complex)

Numbers with real and imaginary parts.

```python
# Complex number examples
z1 = 2 + 3j
z2 = complex(4, 5)  # 4 + 5j

print(type(z1))  # <class 'complex'>
print(z1.real)   # 2.0
print(z1.imag)   # 3.0
```

## Basic Math Operations

```python
a = 10
b = 3

print(a + b)   # Addition: 13
print(a - b)   # Subtraction: 7
print(a * b)   # Multiplication: 30
print(a / b)   # Division: 3.3333333333333335
print(a // b)  # Floor division: 3
print(a % b)   # Modulus: 1
print(a ** b)  # Exponentiation: 1000
```

## Assignment Operators

```python
x = 10

x += 5   # x = x + 5 -> 15
x -= 3   # x = x - 3 -> 12
x *= 2   # x = x * 2 -> 24
x /= 4   # x = x / 4 -> 6.0 (becomes float)
x //= 2  # x = x // 2 -> 3.0
x %= 2   # x = x % 2 -> 1.0
x **= 3  # x = x ** 3 -> 1.0
```

## Built-in Math Functions

```python
import math

# Absolute value
print(abs(-5))     # 5
print(abs(3.14))   # 3.14

# Rounding
print(round(3.7))  # 4
print(round(3.14159, 2))  # 3.14

# Power and square root
print(pow(2, 3))   # 8.0
print(math.sqrt(16))  # 4.0

# Trigonometric functions
print(math.sin(math.pi/2))  # 1.0
print(math.cos(0))          # 1.0
print(math.tan(math.pi/4))  # 1.0

# Logarithmic functions
print(math.log(10))      # Natural log: 2.302585092994046
print(math.log10(100))   # Base 10 log: 2.0
print(math.log2(8))      # Base 2 log: 3.0

# Constants
print(math.pi)   # 3.141592653589793
print(math.e)    # 2.718281828459045
```

## Number Conversion

```python
# String to number
num_str = "123"
num_int = int(num_str)
num_float = float(num_str)

print(num_int)   # 123
print(num_float) # 123.0

# Number to string
num = 456
str_num = str(num)
print(str_num)  # "456"

# Float to int (truncates)
float_num = 3.9
int_num = int(float_num)
print(int_num)  # 3

# Int to float
int_num = 5
float_num = float(int_num)
print(float_num)  # 5.0
```

## Working with Large Numbers

Python automatically handles large integers:

```python
# Very large numbers
big_num = 123456789012345678901234567890
print(big_num)  # 123456789012345678901234567890

# Big number operations
result = big_num * 2
print(result)  # 246913578024691357802469135780
```

## Precision and Floating-Point Issues

```python
# Floating-point precision issues
print(0.1 + 0.2)  # 0.30000000000000004 (not exactly 0.3)

# Using decimal module for precision
from decimal import Decimal

a = Decimal('0.1')
b = Decimal('0.2')
print(a + b)  # Decimal('0.3')

# Using round() to handle precision
result = 0.1 + 0.2
print(round(result, 1))  # 0.3
```

## Random Numbers

```python
import random

# Random float between 0 and 1
print(random.random())  # e.g., 0.37444887175646646

# Random integer in range
print(random.randint(1, 10))  # Random int between 1 and 10

# Random choice from list
colors = ['red', 'blue', 'green', 'yellow']
print(random.choice(colors))  # Random color

# Random float in range
print(random.uniform(1.0, 10.0))  # Random float between 1.0 and 10.0

# Shuffle a list
numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print(numbers)  # [3, 1, 5, 2, 4] (random order)
```

## Complex Number Operations

```python
z1 = 2 + 3j
z2 = 1 - 4j

print(z1 + z2)  # (3-1j)
print(z1 - z2)  # (1+7j)
print(z1 * z2)  # (14-5j)
print(z1 / z2)  # (-0.5882352941176471+0.6470588235294118j)

# Conjugate
print(z1.conjugate())  # (2-3j)

# Magnitude (absolute value)
print(abs(z1))  # 3.605551275463989
```

## Number Formatting

```python
# Format as currency
price = 1234.56789
print(f"${price:,.2f}")  # $1,234.57

# Format as percentage
percentage = 0.856
print(f"{percentage:.1%}")  # 85.6%

# Scientific notation
big_num = 123456789
print(f"{big_num:e}")  # 1.234568e+08

# Binary, octal, hexadecimal
num = 42
print(f"{num:b}")  # 101010 (binary)
print(f"{num:o}")  # 52 (octal)
print(f"{num:x}")  # 2a (hexadecimal)
```
