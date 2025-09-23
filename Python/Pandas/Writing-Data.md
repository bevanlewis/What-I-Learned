# Writing Data in Pandas

Pandas provides comprehensive functionality to write DataFrames and Series to various file formats. This section covers exporting data to different formats with various options.

## Writing to CSV Files

### Basic CSV Writing

```python
import pandas as pd

# Sample DataFrame
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'San Francisco', 'Chicago']
})

# Write to CSV
df.to_csv('output.csv', index=False)
```

### CSV with Custom Options

```python
# Write with custom options
df.to_csv('output.csv',
          index=False,           # Don't write row indices
          sep=',',              # Delimiter
          decimal='.',          # Decimal separator
          encoding='utf-8',     # File encoding
          date_format='%Y-%m-%d') # Date format
```

### Writing Series to CSV

```python
s = pd.Series([1, 2, 3, 4, 5], index=['a', 'b', 'c', 'd', 'e'])
s.to_csv('series_output.csv', header=True)
```

## Writing to Excel Files

```python
# Write to Excel
df.to_excel('output.xlsx', sheet_name='Sheet1', index=False)

# Write multiple sheets
with pd.ExcelWriter('multi_sheet.xlsx') as writer:
    df1.to_excel(writer, sheet_name='Sheet1', index=False)
    df2.to_excel(writer, sheet_name='Sheet2', index=False)
    df3.to_excel(writer, sheet_name='Sheet3', index=False)

# Append to existing Excel file
with pd.ExcelWriter('existing.xlsx', mode='a') as writer:
    df.to_excel(writer, sheet_name='NewSheet', index=False)
```

## Writing to JSON Files

```python
# Write to JSON with different orientations
df.to_json('output.json', orient='records')    # List of records
df.to_json('output.json', orient='index')      # Dict with index
df.to_json('output.json', orient='split')      # Separate index, columns, data
df.to_json('output.json', orient='values')     # Just values as nested array

# Pretty-printed JSON
df.to_json('pretty.json', indent=2, orient='records')
```

## Writing to SQL Databases

### Using SQLAlchemy

```python
from sqlalchemy import create_engine

# Create database connection
engine = create_engine('sqlite:///data.db')
# engine = create_engine('postgresql://user:pass@localhost/db')
# engine = create_engine('mysql://user:pass@localhost/db')

# Write DataFrame to SQL table
df.to_sql('users', engine, index=False, if_exists='replace')

# Options for if_exists:
# 'fail' - Raise error if table exists
# 'replace' - Drop table and recreate
# 'append' - Add rows to existing table
```

### Using sqlite3

```python
import sqlite3

# Connect to database
conn = sqlite3.connect('data.db')

# Write DataFrame to SQL table
df.to_sql('users', conn, index=False, if_exists='replace')

conn.close()
```

## Writing to Parquet Files

```python
# Write to Parquet
df.to_parquet('output.parquet', index=False)

# With compression
df.to_parquet('compressed.parquet',
              compression='snappy',  # or 'gzip', 'brotli'
              index=False)

# With partitioning
df.to_parquet('partitioned/',
              partition_cols=['year', 'month'],
              index=False)
```

## Writing to Other Formats

### Writing Pickle Files

```python
# Write pickled pandas object
df.to_pickle('data.pkl')

# Read back
df_loaded = pd.read_pickle('data.pkl')
```

### Writing HDF5 Files

```python
# Write to HDF5
df.to_hdf('data.h5', key='df', mode='w')

# Append to HDF5 file
df.to_hdf('data.h5', key='df2', mode='a')
```

### Writing Feather Files

```python
# Write to Feather format
df.to_feather('data.feather')
```

## Controlling Output Format

### Custom Date Formatting

```python
# DataFrame with dates
df_dates = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=3),
    'value': [1, 2, 3]
})

# Custom date format
df_dates.to_csv('dates.csv', date_format='%Y-%m-%d', index=False)
```

### Formatting Numeric Data

```python
# DataFrame with floats
df_floats = pd.DataFrame({
    'value': [1.23456, 7.89012, 3.45678]
})

# Control decimal places
df_floats.to_csv('floats.csv',
                 float_format='%.2f',  # 2 decimal places
                 index=False)
```

### Custom Column Headers

```python
# Rename columns for output
df_output = df.rename(columns={
    'Name': 'Full Name',
    'Age': 'Age (Years)',
    'City': 'Location'
})

df_output.to_csv('renamed_columns.csv', index=False)
```

## Chunked Writing for Large Datasets

```python
# Write large DataFrame in chunks
chunk_size = 10000

# For CSV
for i in range(0, len(df), chunk_size):
    chunk = df.iloc[i:i+chunk_size]
    chunk.to_csv(f'output_chunk_{i//chunk_size}.csv', index=False)

# For Excel (with multiple sheets)
with pd.ExcelWriter('large_data.xlsx') as writer:
    for i in range(0, len(df), chunk_size):
        chunk = df.iloc[i:i+chunk_size]
        chunk.to_excel(writer,
                      sheet_name=f'Chunk_{i//chunk_size}',
                      index=False)
```

## Appending Data

### Append to Existing CSV

```python
# Append to existing CSV (without header)
df_new = pd.DataFrame({
    'Name': ['Diana', 'Eve'],
    'Age': [28, 32],
    'City': ['Boston', 'Seattle']
})

df_new.to_csv('output.csv', mode='a', header=False, index=False)
```

### Append to SQL Table

```python
# Append to existing SQL table
df_new.to_sql('users', engine, index=False, if_exists='append')
```

## Handling Special Characters and Encoding

```python
# Handle different encodings
df.to_csv('utf8_output.csv', encoding='utf-8')
df.to_csv('latin_output.csv', encoding='latin1')

# Handle special characters in column names
df_with_special = pd.DataFrame({
    'Name (Full)': ['Alice', 'Bob'],
    'Age [Years]': [25, 30]
})

# Quote column names with special characters
df_with_special.to_csv('special_chars.csv', quoting=1, index=False)
```

## Compression Options

```python
# CSV with gzip compression
df.to_csv('compressed.csv.gz', compression='gzip', index=False)

# Parquet with different compressions
df.to_parquet('data_snappy.parquet', compression='snappy')
df.to_parquet('data_gzip.parquet', compression='gzip')

# JSON with compression
df.to_json('data.json.gz', compression='gzip', orient='records')
```

## Custom Formatting Functions

```python
# Apply custom formatting
def format_currency(x):
    return f"${x:,.2f}"

def format_percentage(x):
    return f"{x:.1%}"

df_formatted = df.copy()
df_formatted['salary'] = df_formatted['salary'].apply(format_currency)
df_formatted['rate'] = df_formatted['rate'].apply(format_percentage)

df_formatted.to_csv('formatted_output.csv', index=False)
```

## Writing with Progress Indication

```python
import os

def write_with_progress(df, filename, chunk_size=10000):
    """Write DataFrame with progress indication"""
    total_rows = len(df)

    if total_rows <= chunk_size:
        df.to_csv(filename, index=False)
        print(f"Written {total_rows} rows to {filename}")
        return

    # Write header first
    df.head(0).to_csv(filename, index=False)

    # Write data in chunks
    with open(filename, 'a') as f:
        for i in range(0, total_rows, chunk_size):
            chunk = df.iloc[i:i+chunk_size]
            chunk.to_csv(f, header=False, index=False)

            progress = min(i + chunk_size, total_rows)
            print(f"Written {progress}/{total_rows} rows ({progress/total_rows*100:.1f}%)")

    print(f"Completed writing {total_rows} rows to {filename}")

# Usage
write_with_progress(large_df, 'large_output.csv')
```

## Error Handling

```python
def safe_write_csv(df, filename, **kwargs):
    """Safely write DataFrame to CSV with error handling"""
    try:
        df.to_csv(filename, **kwargs)
        print(f"Successfully wrote {len(df)} rows to {filename}")

        # Verify file was written
        if os.path.exists(filename):
            file_size = os.path.getsize(filename)
            print(f"File size: {file_size} bytes")
        else:
            print(f"Warning: File {filename} was not created")

    except PermissionError:
        print(f"Error: Permission denied writing to {filename}")
    except OSError as e:
        print(f"Error: OS error writing to {filename}: {e}")
    except Exception as e:
        print(f"Error: Unexpected error writing to {filename}: {e}")

# Usage
safe_write_csv(df, 'output.csv', index=False)
```

## Performance Considerations

### Choosing the Right Format

```python
# For analysis and interchange: CSV
df.to_csv('data.csv', index=False)

# For Python/pandas only: Pickle
df.to_pickle('data.pkl')

# For big data: Parquet
df.to_parquet('data.parquet')

# For compressed storage: HDF5
df.to_hdf('data.h5', key='df')
```

### Memory-Efficient Writing

```python
# Convert to efficient dtypes before writing
df_optimized = df.copy()
df_optimized['category_col'] = df_optimized['category_col'].astype('category')
df_optimized['int_col'] = df_optimized['int_col'].astype('int32')

df_optimized.to_csv('optimized.csv', index=False)
```

## Best Practices

1. **Always specify index=False** when writing DataFrames unless you need the index
2. **Choose appropriate file formats** based on use case and performance needs
3. **Handle encoding properly** for international characters
4. **Use compression** for large datasets to save disk space
5. **Validate output** after writing important data
6. **Consider chunked writing** for very large datasets
7. **Document your export parameters** for reproducibility
8. **Use meaningful filenames** that indicate content and date

## Common Issues and Solutions

### Permission Errors

```python
# Handle permission errors
try:
    df.to_csv('readonly_file.csv', index=False)
except PermissionError:
    print("Permission denied. Try saving to a different location.")
    df.to_csv('~/output.csv', index=False)
```

### Encoding Issues

```python
# Handle encoding issues
try:
    df.to_csv('output.csv', encoding='utf-8', index=False)
except UnicodeEncodeError:
    # Fall back to different encoding
    df.to_csv('output.csv', encoding='utf-8-sig', index=False)
```

### Memory Issues

```python
# For very large DataFrames, use chunks
def write_large_df(df, filename, chunk_size=50000):
    """Write large DataFrame in chunks to avoid memory issues"""
    # Write header
    df.head(0).to_csv(filename, index=False)

    # Write data in chunks
    for i in range(0, len(df), chunk_size):
        chunk = df.iloc[i:i+chunk_size]
        chunk.to_csv(filename, mode='a', header=False, index=False)

write_large_df(huge_df, 'huge_output.csv')
```

### Date Formatting Issues

```python
# Ensure dates are properly formatted
df['date_col'] = pd.to_datetime(df['date_col'])
df.to_csv('dates.csv', date_format='%Y-%m-%d %H:%M:%S', index=False)
```
