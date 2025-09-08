# Python File I/O

File I/O (Input/Output) operations allow you to read from and write to files on your system. Python provides built-in functions and methods for working with files in a simple and efficient way.

## Opening Files

The `open()` function is used to open files:

```python
# Basic syntax
file = open(filename, mode)

# Common modes:
# 'r' - Read (default)
# 'w' - Write (creates new file or truncates existing)
# 'a' - Append (adds to end of file)
# 'x' - Exclusive creation (fails if file exists)
# 'b' - Binary mode
# 't' - Text mode (default)
# '+' - Read and write
```

## Reading Files

### Reading Entire File

```python
# Method 1: read() - reads entire file as string
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)

# Method 2: readlines() - reads all lines into a list
with open('example.txt', 'r') as file:
    lines = file.readlines()
    print(lines)

# Method 3: readline() - reads one line at a time
with open('example.txt', 'r') as file:
    line = file.readline()
    while line:
        print(line.strip())  # strip() removes newline character
        line = file.readline()
```

### Iterating Over File Lines

```python
# Most efficient way to read large files
with open('large_file.txt', 'r') as file:
    for line in file:
        print(line.strip())
```

### Reading with Specific Encoding

```python
# Specify encoding for international characters
with open('utf8_file.txt', 'r', encoding='utf-8') as file:
    content = file.read()
```

## Writing Files

### Writing Text Files

```python
# Write mode ('w') - creates new file or overwrites existing
with open('output.txt', 'w') as file:
    file.write("Hello, World!\n")
    file.write("This is a new line.")

# Append mode ('a') - adds to end of existing file
with open('output.txt', 'a') as file:
    file.write("\nThis line will be appended.")

# Writing multiple lines
lines = ["Line 1", "Line 2", "Line 3"]
with open('multi_line.txt', 'w') as file:
    for line in lines:
        file.write(line + '\n')

# Using writelines() - note: doesn't add newlines automatically
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open('multi_line.txt', 'w') as file:
    file.writelines(lines)
```

## Context Managers (with statement)

The `with` statement automatically handles file closing:

```python
# Automatic file closing
with open('file.txt', 'r') as file:
    content = file.read()
# File is automatically closed here

# Manual file handling (not recommended)
file = open('file.txt', 'r')
content = file.read()
file.close()  # Must remember to close!
```

## Binary Files

### Reading Binary Files

```python
# Read binary file
with open('image.jpg', 'rb') as file:
    binary_data = file.read()
    print(type(binary_data))  # <class 'bytes'>
```

### Writing Binary Files

```python
# Write binary data
binary_data = b'\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR'
with open('output.bin', 'wb') as file:
    file.write(binary_data)
```

## File Position and Seeking

```python
with open('file.txt', 'r') as file:
    # Get current position
    print(file.tell())  # 0

    # Read first 10 characters
    content = file.read(10)
    print(content)
    print(file.tell())  # 10

    # Move to position 5
    file.seek(5)
    print(file.tell())  # 5

    # Read from new position
    content = file.read(5)
    print(content)
```

## Working with CSV Files

### Reading CSV Files

```python
import csv

# Reading CSV with csv module
with open('data.csv', 'r') as file:
    csv_reader = csv.reader(file)
    for row in csv_reader:
        print(row)

# Reading with headers
with open('data.csv', 'r') as file:
    csv_reader = csv.DictReader(file)
    for row in csv_reader:
        print(row['column_name'])
```

### Writing CSV Files

```python
import csv

# Writing CSV
data = [
    ['Name', 'Age', 'City'],
    ['Alice', 30, 'New York'],
    ['Bob', 25, 'San Francisco']
]

with open('output.csv', 'w', newline='') as file:
    csv_writer = csv.writer(file)
    for row in data:
        csv_writer.writerow(row)

# Writing with DictWriter
fieldnames = ['Name', 'Age', 'City']
data = [
    {'Name': 'Alice', 'Age': 30, 'City': 'New York'},
    {'Name': 'Bob', 'Age': 25, 'City': 'San Francisco'}
]

with open('output.csv', 'w', newline='') as file:
    csv_writer = csv.DictWriter(file, fieldnames=fieldnames)
    csv_writer.writeheader()
    for row in data:
        csv_writer.writerow(row)
```

## Working with JSON Files

```python
import json

# Writing JSON
data = {
    "name": "Alice",
    "age": 30,
    "city": "New York",
    "hobbies": ["reading", "swimming"]
}

with open('data.json', 'w') as file:
    json.dump(data, file, indent=4)

# Reading JSON
with open('data.json', 'r') as file:
    loaded_data = json.load(file)
    print(loaded_data['name'])  # Alice
```

## File and Directory Operations

```python
import os
import shutil

# Check if file exists
if os.path.exists('file.txt'):
    print("File exists")

# Check if it's a file or directory
print(os.path.isfile('file.txt'))   # True for files
print(os.path.isdir('folder'))      # True for directories

# Get file size
print(os.path.getsize('file.txt'))  # Size in bytes

# Get file modification time
import time
mod_time = os.path.getmtime('file.txt')
print(time.ctime(mod_time))  # Human readable time

# List files in directory
files = os.listdir('.')
print(files)

# Create directory
os.mkdir('new_folder')

# Create nested directories
os.makedirs('parent/child/grandchild')

# Remove file
os.remove('file.txt')

# Remove directory (must be empty)
os.rmdir('empty_folder')

# Remove directory and all contents
shutil.rmtree('folder_with_contents')

# Copy file
shutil.copy('source.txt', 'destination.txt')

# Move/rename file
shutil.move('old_name.txt', 'new_name.txt')
```

## Path Operations

```python
import os
from pathlib import Path

# Using os.path
filepath = os.path.join('folder', 'subfolder', 'file.txt')
print(filepath)  # 'folder/subfolder/file.txt' (platform specific)

dirname = os.path.dirname(filepath)
basename = os.path.basename(filepath)
print(dirname)   # 'folder/subfolder'
print(basename)  # 'file.txt'

# Using pathlib (Python 3.4+)
path = Path('folder') / 'subfolder' / 'file.txt'
print(path)           # folder/subfolder/file.txt
print(path.parent)    # folder/subfolder
print(path.name)      # file.txt
print(path.suffix)    # .txt
print(path.stem)      # file

# Check if path exists
if path.exists():
    print("Path exists")

# Create file
path.touch()

# Read and write
path.write_text("Hello, World!")
content = path.read_text()
print(content)
```

## Temporary Files

```python
import tempfile

# Create temporary file
with tempfile.NamedTemporaryFile(mode='w', delete=False) as temp_file:
    temp_file.write("Temporary content")
    temp_filename = temp_file.name

print(f"Temporary file created: {temp_filename}")

# Clean up
os.unlink(temp_filename)

# Create temporary directory
with tempfile.TemporaryDirectory() as temp_dir:
    temp_file_path = os.path.join(temp_dir, 'temp.txt')
    with open(temp_file_path, 'w') as f:
        f.write("Content in temporary directory")

print("Temporary directory and contents cleaned up automatically")
```

## Error Handling

```python
try:
    with open('nonexistent_file.txt', 'r') as file:
        content = file.read()
except FileNotFoundError:
    print("File not found")
except PermissionError:
    print("Permission denied")
except IOError as e:
    print(f"IO Error: {e}")

# More comprehensive error handling
def read_file_safely(filename):
    try:
        with open(filename, 'r') as file:
            return file.read()
    except FileNotFoundError:
        print(f"Error: File '{filename}' not found")
        return None
    except PermissionError:
        print(f"Error: Permission denied for '{filename}'")
        return None
    except Exception as e:
        print(f"Unexpected error reading '{filename}': {e}")
        return None
```

## File Buffering

```python
# Default buffering (usually line-buffered for text, block-buffered for binary)
with open('large_file.txt', 'r') as file:
    content = file.read()  # Reads entire file into memory

# Read in chunks to handle large files
chunk_size = 1024  # 1 KB
with open('large_file.txt', 'r') as file:
    while True:
        chunk = file.read(chunk_size)
        if not chunk:
            break
        process_chunk(chunk)

# Line by line (efficient for text files)
with open('large_file.txt', 'r') as file:
    for line in file:
        process_line(line)
```

## Working with Different Encodings

```python
# Specify encoding explicitly
with open('utf8_file.txt', 'r', encoding='utf-8') as file:
    content = file.read()

# Handle encoding errors
with open('file_with_unknown_encoding.txt', 'r', encoding='utf-8', errors='replace') as file:
    content = file.read()  # Replaces invalid characters with �

# Write with specific encoding
with open('output.txt', 'w', encoding='utf-16') as file:
    file.write("Hello, 世界!")
```

## Advanced File Operations

### Memory-mapped Files

```python
import mmap

# Memory mapping for large files
with open('large_file.txt', 'r') as file:
    with mmap.mmap(file.fileno(), 0, access=mmap.ACCESS_READ) as mapped_file:
        # File content is mapped to memory
        print(mapped_file[:100])  # First 100 bytes
```

### Compressed Files

```python
import gzip
import zipfile

# Reading gzip files
with gzip.open('file.txt.gz', 'rt') as file:
    content = file.read()

# Writing gzip files
with gzip.open('output.txt.gz', 'wt') as file:
    file.write("Compressed content")

# Working with zip files
with zipfile.ZipFile('archive.zip', 'w') as zip_file:
    zip_file.write('file1.txt')
    zip_file.write('file2.txt')

# Reading zip files
with zipfile.ZipFile('archive.zip', 'r') as zip_file:
    zip_file.extractall('extracted_folder')
    print(zip_file.namelist())  # List of files in archive
```

### File Locking

```python
import fcntl

# File locking (Unix systems)
with open('shared_file.txt', 'w') as file:
    # Acquire exclusive lock
    fcntl.flock(file.fileno(), fcntl.LOCK_EX)

    # Critical section
    file.write("Exclusive content")

    # Lock is automatically released when file is closed
```

## Best Practices

### 1. Always use `with` statement

```python
# Good
with open('file.txt', 'r') as file:
    content = file.read()

# Avoid
file = open('file.txt', 'r')
content = file.read()
file.close()  # Easy to forget!
```

### 2. Handle exceptions properly

```python
def read_config(filename):
    try:
        with open(filename, 'r') as file:
            return file.read()
    except FileNotFoundError:
        print(f"Configuration file '{filename}' not found")
        return ""
    except PermissionError:
        print(f"Permission denied reading '{filename}'")
        return ""
```

### 3. Use appropriate encodings

```python
# Specify UTF-8 encoding explicitly
with open('file.txt', 'r', encoding='utf-8') as file:
    content = file.read()
```

### 4. Be careful with file paths

```python
import os

# Use os.path.join for cross-platform compatibility
filepath = os.path.join('folder', 'subfolder', 'file.txt')

# Normalize paths
normalized_path = os.path.normpath('/folder/./subfolder/../file.txt')
```

### 5. Check file existence before operations

```python
import os

if os.path.exists('file.txt'):
    with open('file.txt', 'r') as file:
        content = file.read()
else:
    print("File does not exist")
```

### 6. Use pathlib for modern Python

```python
from pathlib import Path

# Modern path handling
config_file = Path('config') / 'settings.json'

if config_file.exists():
    data = config_file.read_text()
    config_file.write_text('new content')
```

### 7. Handle large files efficiently

```python
# Process large files line by line
def process_large_file(filename):
    with open(filename, 'r') as file:
        for line_number, line in enumerate(file, 1):
            if line_number % 100000 == 0:  # Progress indicator
                print(f"Processed {line_number} lines")
            process_line(line)
```

### 8. Use context managers for custom file-like objects

```python
from contextlib import contextmanager

@contextmanager
def managed_file(filename, mode):
    file = open(filename, mode)
    try:
        yield file
    finally:
        file.close()

# Usage
with managed_file('test.txt', 'w') as file:
    file.write("Content")
# File is automatically closed
```

## Common File I/O Patterns

### Configuration Files

```python
import configparser

# Writing config
config = configparser.ConfigParser()
config['DEFAULT'] = {'ServerAliveInterval': '45',
                     'Compression': 'yes'}
config['bitbucket.org'] = {}
config['bitbucket.org']['User'] = 'hg'

with open('config.ini', 'w') as configfile:
    config.write(configfile)

# Reading config
config = configparser.ConfigParser()
config.read('config.ini')
print(config['bitbucket.org']['User'])
```

### Logging to Files

```python
import logging

# Configure logging to file
logging.basicConfig(filename='app.log', level=logging.INFO,
                    format='%(asctime)s - %(levelname)s - %(message)s')

logging.info('Application started')
logging.warning('This is a warning')
logging.error('An error occurred')

# Also log to console
console = logging.StreamHandler()
console.setLevel(logging.INFO)
logging.getLogger('').addHandler(console)
```

### Pickle for Python Objects

```python
import pickle

# Serialize Python objects
data = {'name': 'Alice', 'age': 30, 'items': [1, 2, 3]}

with open('data.pkl', 'wb') as file:
    pickle.dump(data, file)

# Deserialize
with open('data.pkl', 'rb') as file:
    loaded_data = pickle.load(file)

print(loaded_data)  # {'name': 'Alice', 'age': 30, 'items': [1, 2, 3]}
```

This comprehensive guide covers the fundamentals and advanced techniques for working with files in Python, from basic read/write operations to complex file handling patterns.
