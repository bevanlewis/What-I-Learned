# Python Strings

Strings are sequences of characters used to represent text data. In Python, strings are immutable, meaning they cannot be changed after creation.

## Creating Strings

```python
# Single quotes
name = 'Alice'

# Double quotes
greeting = "Hello, World!"

# Triple quotes for multi-line strings
multiline = """This is a
multi-line
string"""

# Empty string
empty = ""

# String with escape characters
escaped = "He said, \"Hello!\""
```

## String Concatenation

```python
first_name = "John"
last_name = "Doe"
full_name = first_name + " " + last_name
print(full_name)  # "John Doe"

# Using join() for multiple strings
words = ["Hello", "World", "Python"]
sentence = " ".join(words)
print(sentence)  # "Hello World Python"
```

## String Formatting

### f-strings (Python 3.6+)

```python
name = "Alice"
age = 30
message = f"My name is {name} and I am {age} years old."
print(message)  # "My name is Alice and I am {age} years old."
```

### format() method

```python
name = "Bob"
age = 25
message = "My name is {} and I am {} years old.".format(name, age)
print(message)  # "My name is Bob and I am 25 years old."

# With named placeholders
message = "My name is {name} and I am {age} years old.".format(name="Charlie", age=35)
print(message)  # "My name is Charlie and I am 35 years old."
```

### % formatting (older style)

```python
name = "David"
age = 40
message = "My name is %s and I am %d years old." % (name, age)
print(message)  # "My name is David and I am 40 years old."
```

## String Methods

### Case Conversion

```python
text = "Hello World"

print(text.upper())      # "HELLO WORLD"
print(text.lower())      # "hello world"
print(text.capitalize()) # "Hello world"
print(text.title())      # "Hello World"
```

### Searching and Finding

```python
text = "Hello, World!"

print(text.find("World"))    # 7 (index of first occurrence)
print(text.index("World"))   # 7 (same as find, but raises error if not found)
print(text.count("l"))       # 3 (count occurrences)
print(text.startswith("Hello"))  # True
print(text.endswith("!"))        # True
```

### Replacing and Splitting

```python
text = "Hello, World!"

print(text.replace("World", "Python"))  # "Hello, Python!"
print(text.split(","))      # ['Hello', ' World!']
print(text.split())         # ['Hello,', 'World!']
```

### Trimming Whitespace

```python
text = "  Hello, World!  "

print(text.strip())   # "Hello, World!" (removes both ends)
print(text.lstrip())  # "Hello, World!  " (removes left side)
print(text.rstrip())  # "  Hello, World!" (removes right side)
```

### Checking String Properties

```python
text = "Hello123"

print(text.isalpha())   # False (contains numbers)
print(text.isdigit())   # False (contains letters)
print(text.isalnum())   # True (contains only letters and numbers)
print(text.islower())   # False
print(text.isupper())   # False
```

## String Slicing

```python
text = "Hello, World!"

print(text[0])      # 'H' (first character)
print(text[-1])     # '!' (last character)
print(text[0:5])    # 'Hello' (characters 0 to 4)
print(text[7:])     # 'World!' (from index 7 to end)
print(text[:5])     # 'Hello' (from start to index 4)
print(text[::2])    # 'Hlo ol!' (every second character)
print(text[::-1])   # '!dlroW ,olleH' (reversed string)
```

## String Length

```python
text = "Hello, World!"
print(len(text))  # 13
```

## String Membership

```python
text = "Hello, World!"

print("Hello" in text)     # True
print("Python" in text)    # False
print("Python" not in text)  # True
```

## Raw Strings

Raw strings treat backslashes as literal characters:

```python
# Regular string
path = "C:\\Users\\Documents\\file.txt"
print(path)  # "C:\Users\Documents\file.txt"

# Raw string
raw_path = r"C:\Users\Documents\file.txt"
print(raw_path)  # "C:\Users\Documents\file.txt"
```

## String Encoding

```python
text = "Hello, 世界!"

# Encode to bytes
encoded = text.encode('utf-8')
print(encoded)  # b'Hello, \xe4\xb8\x96\xe7\x95\x8c!'

# Decode from bytes
decoded = encoded.decode('utf-8')
print(decoded)  # "Hello, 世界!"
```

## Multi-line String Operations

```python
poem = """Roses are red,
Violets are blue,
Python is awesome,
And so are you!"""

print(poem)
# Output:
# Roses are red,
# Violets are blue,
# Python is awesome,
# And so are you!

# Split into lines
lines = poem.split('\n')
print(lines)
# ['Roses are red,', 'Violets are blue,', 'Python is awesome,', 'And so are you!']
```
