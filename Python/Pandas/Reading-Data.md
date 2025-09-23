# Reading Data in Pandas

Pandas provides powerful functions to read data from various file formats and sources. This section covers the most common methods for reading data into pandas DataFrames and Series.

## Reading CSV Files

### Basic CSV Reading

```python
import pandas as pd

# Basic CSV reading
df = pd.read_csv('data.csv')
print(df.head())  # First 5 rows
```

### CSV with Specific Options

```python
# Reading with custom options
df = pd.read_csv('data.csv',
                 sep=',',                    # Delimiter
                 header=0,                   # Row number to use as header
                 index_col=0,                # Column to use as index
                 parse_dates=['date_column'], # Columns to parse as dates
                 dtype={'column_name': str}) # Specify data types

print(df.head())
```

### CSV with Advanced Options

```python
# Reading large files in chunks
chunk_size = 1000
for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    # Process each chunk
    print(f"Processing chunk with {len(chunk)} rows")
    # Perform operations on chunk

# Reading specific columns only
df = pd.read_csv('data.csv', usecols=['column1', 'column2', 'column3'])

# Skipping rows
df = pd.read_csv('data.csv', skiprows=[0, 2, 5])  # Skip specific rows
df = pd.read_csv('data.csv', skiprows=2)          # Skip first n rows
```

### Reading from URLs

```python
# Reading CSV from URL
url = 'https://example.com/data.csv'
df = pd.read_csv(url)

# Reading from GitHub
github_url = 'https://raw.githubusercontent.com/user/repo/main/data.csv'
df = pd.read_csv(github_url)
```

## Reading Excel Files

```python
# Reading Excel file
df = pd.read_excel('data.xlsx', sheet_name='Sheet1')

# Reading specific sheet by index
df = pd.read_excel('data.xlsx', sheet_name=0)  # First sheet

# Reading multiple sheets
sheets = pd.read_excel('data.xlsx', sheet_name=['Sheet1', 'Sheet2'])
df1 = sheets['Sheet1']
df2 = sheets['Sheet2']

# Reading all sheets
all_sheets = pd.read_excel('data.xlsx', sheet_name=None)
for sheet_name, df in all_sheets.items():
    print(f"Sheet: {sheet_name}")
    print(df.head())
```

## Reading JSON Files

```python
# Reading JSON file
df = pd.read_json('data.json')

# Reading JSON with different orientations
df = pd.read_json('data.json', orient='records')    # List of records
df = pd.read_json('data.json', orient='index')      # Dict with index
df = pd.read_json('data.json', orient='values')     # Nested array

# Reading JSON Lines (.jsonl files)
df = pd.read_json('data.jsonl', lines=True)
```

## Reading from Databases

### SQL Databases with SQLAlchemy

```python
from sqlalchemy import create_engine
import pandas as pd

# Create database connection
engine = create_engine('sqlite:///data.db')
# engine = create_engine('postgresql://user:pass@localhost/db')
# engine = create_engine('mysql://user:pass@localhost/db')

# Read entire table
df = pd.read_sql_table('table_name', engine)

# Read with SQL query
query = "SELECT * FROM users WHERE age > 18"
df = pd.read_sql_query(query, engine)

# Read with parameters
query = "SELECT * FROM users WHERE age > ? AND city = ?"
df = pd.read_sql_query(query, engine, params=(18, 'New York'))
```

### SQLite with sqlite3

```python
import sqlite3

# Connect to database
conn = sqlite3.connect('data.db')

# Read data
df = pd.read_sql_query("SELECT * FROM users", conn)

conn.close()
```

## Reading HTML Tables

```python
# Read tables from HTML
tables = pd.read_html('https://example.com/table.html')
df = tables[0]  # First table

# Read specific table by index
df = pd.read_html('https://example.com/table.html')[1]  # Second table

# Read from local HTML file
df = pd.read_html('table.html')[0]
```

## Reading Parquet Files

```python
# Reading Parquet files
df = pd.read_parquet('data.parquet')

# Reading with specific engine
df = pd.read_parquet('data.parquet', engine='pyarrow')
df = pd.read_parquet('data.parquet', engine='fastparquet')
```

## Reading Other Formats

### Reading Pickle Files

```python
# Reading pickled pandas objects
df = pd.read_pickle('data.pkl')
```

### Reading HDF5 Files

```python
# Reading HDF5 files
df = pd.read_hdf('data.h5', key='df')
```

### Reading Feather Files

```python
# Reading Feather files
df = pd.read_feather('data.feather')
```

## Advanced Reading Techniques

### Handling Large Files

```python
# Process large CSV files in chunks
chunk_size = 10000
chunks = []

for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    # Process each chunk
    chunk['new_column'] = chunk['existing_column'] * 2
    chunks.append(chunk)

# Combine all chunks
df = pd.concat(chunks, ignore_index=True)
```

### Reading Files with Different Encodings

```python
# Handle different file encodings
df = pd.read_csv('data_utf8.csv', encoding='utf-8')
df = pd.read_csv('data_latin.csv', encoding='latin1')
df = pd.read_csv('data_cp1252.csv', encoding='cp1252')
```

### Reading Files with Bad Lines

```python
# Skip bad lines in CSV
df = pd.read_csv('data.csv', error_bad_lines=False, warn_bad_lines=True)

# Or use on_bad_lines parameter (pandas 1.3+)
df = pd.read_csv('data.csv', on_bad_lines='skip')
```

### Custom Date Parsing

```python
# Custom date parsing
date_parser = lambda x: pd.to_datetime(x, format='%Y-%m-%d %H:%M:%S')
df = pd.read_csv('data.csv', parse_dates=['timestamp'], date_parser=date_parser)
```

### Reading Fixed-Width Files

```python
# Reading fixed-width formatted files
df = pd.read_fwf('data.txt', widths=[10, 5, 10])
```

## Data Type Specification

### Automatic Type Inference

```python
# Let pandas infer types (default)
df = pd.read_csv('data.csv')

# Check inferred types
print(df.dtypes)
```

### Explicit Type Specification

```python
# Specify types explicitly
dtype_dict = {
    'id': 'int64',
    'name': 'string',
    'age': 'int32',
    'salary': 'float64',
    'is_active': 'boolean'
}

df = pd.read_csv('data.csv', dtype=dtype_dict)
```

### Category Types for Memory Efficiency

```python
# Convert columns to category type
df = pd.read_csv('data.csv', dtype={'category_column': 'category'})
```

## Handling Missing Data During Reading

```python
# Specify custom NA values
df = pd.read_csv('data.csv', na_values=['NA', 'N/A', 'null', ''])

# Keep default NA values
df = pd.read_csv('data.csv', keep_default_na=True)

# Specify which values to treat as NA
na_values = ['missing', 'unknown', 'N/A']
df = pd.read_csv('data.csv', na_values=na_values)
```

## Performance Optimization

### Reading Only Required Columns

```python
# Read only specific columns
df = pd.read_csv('data.csv', usecols=['col1', 'col2', 'col5'])

# Read columns by position
df = pd.read_csv('data.csv', usecols=[0, 2, 4])
```

### Using Appropriate Engines

```python
# For CSV files, try different engines for performance
df = pd.read_csv('data.csv', engine='c')     # C engine (default, faster)
df = pd.read_csv('data.csv', engine='python') # Python engine (more flexible)
```

### Memory Mapping for Large Files

```python
# Memory mapping for very large files
import mmap

def read_large_csv(filename):
    with open(filename, 'r') as f:
        # Memory map the file
        mm = mmap.mmap(f.fileno(), 0, access=mmap.ACCESS_READ)
        # Read from memory-mapped file
        content = mm.read().decode('utf-8')
        mm.close()

    # Process the content
    from io import StringIO
    df = pd.read_csv(StringIO(content))
    return df
```

## Error Handling

```python
def safe_read_csv(filename, **kwargs):
    """Safely read CSV with error handling"""
    try:
        df = pd.read_csv(filename, **kwargs)
        print(f"Successfully read {len(df)} rows from {filename}")
        return df
    except FileNotFoundError:
        print(f"Error: File {filename} not found")
        return None
    except pd.errors.EmptyDataError:
        print(f"Error: File {filename} is empty")
        return None
    except Exception as e:
        print(f"Error reading {filename}: {e}")
        return None

# Usage
df = safe_read_csv('data.csv')
```

## Best Practices

1. **Always check file existence** before reading
2. **Specify data types** when possible to optimize memory usage
3. **Use chunks** for large files to avoid memory issues
4. **Handle encoding issues** appropriately
5. **Validate data** after reading
6. **Use appropriate engines** for different file types
7. **Consider compression** for large datasets
8. **Document your data sources** and reading parameters

## Common Issues and Solutions

### Encoding Issues

```python
# Try different encodings
encodings = ['utf-8', 'latin1', 'cp1252', 'iso-8859-1']
for encoding in encodings:
    try:
        df = pd.read_csv('problematic_file.csv', encoding=encoding)
        print(f"Success with {encoding}")
        break
    except UnicodeDecodeError:
        continue
```

### Malformed CSV Files

```python
# Handle quoting issues
df = pd.read_csv('bad_quotes.csv', quoting=3)  # QUOTE_ALL

# Skip problematic lines
df = pd.read_csv('bad_lines.csv', on_bad_lines='skip')
```

### Memory Issues with Large Files

```python
# Use dtypes to reduce memory usage
dtypes = {
    'large_int': 'int32',      # Instead of int64
    'category': 'category',    # For repeated strings
    'boolean': 'bool'          # For boolean data
}
df = pd.read_csv('large_file.csv', dtype=dtypes)
```
