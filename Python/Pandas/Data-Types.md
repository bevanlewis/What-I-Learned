# Data Type Conversion

Pandas provides extensive tools for converting and optimizing data types, which is crucial for memory efficiency and correct operations.

## Checking Data Types

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'integers': ['1', '2', '3'],
    'floats': ['1.1', '2.2', '3.3'],
    'booleans': ['True', 'False', 'True'],
    'dates': ['2023-01-01', '2023-02-01', '2023-03-01']
})

# Check current data types
print(df.dtypes)
```

## Converting Data Types

### Numeric Conversions

```python
# Convert to integers
df['integers'] = pd.to_numeric(df['integers'], errors='coerce').astype('int64')
print(df['integers'].dtype)

# Convert to floats
df['floats'] = pd.to_numeric(df['floats'], errors='coerce')
print(df['floats'].dtype)

# Handle errors during conversion
df['mixed'] = ['1', '2', 'not_a_number']
df['mixed_numeric'] = pd.to_numeric(df['mixed'], errors='coerce')  # NaN for invalid
print(df['mixed_numeric'])
```

### Boolean Conversions

```python
# Convert to boolean
df['booleans'] = df['booleans'].astype(bool)
print(df['booleans'].dtype)

# Convert various representations to boolean
bool_map = {'True': True, 'False': False, '1': True, '0': False}
df['bool_from_str'] = df['booleans'].map(bool_map)
print(df['bool_from_str'])
```

### DateTime Conversions

```python
# Convert to datetime
df['dates'] = pd.to_datetime(df['dates'])
print(df['dates'].dtype)

# Specify date format
df['custom_dates'] = pd.to_datetime(['01/02/2023', '02/03/2023'], format='%m/%d/%Y')
print(df['custom_dates'])
```

## Memory Optimization

### Downcasting Numeric Types

```python
# Create DataFrame with large numeric ranges
large_df = pd.DataFrame({
    'big_int': [1, 2, 3, 4, 5],
    'big_float': [1.0, 2.0, 3.0, 4.0, 5.0]
})

print("Original memory usage:")
print(large_df.memory_usage(deep=True))

# Downcast to smaller types
large_df['big_int'] = pd.to_numeric(large_df['big_int'], downcast='integer')
large_df['big_float'] = pd.to_numeric(large_df['big_float'], downcast='float')

print("\nOptimized memory usage:")
print(large_df.memory_usage(deep=True))
```

### Categorical Data Types

```python
# Convert repeated strings to categorical
df_cat = pd.DataFrame({
    'category': ['A', 'B', 'A', 'C', 'B', 'A'] * 1000,
    'values': range(6000)
})

print("Original memory:")
print(df_cat.memory_usage(deep=True))

# Convert to categorical
df_cat['category'] = df_cat['category'].astype('category')

print("\nCategorical memory:")
print(df_cat.memory_usage(deep=True))
```

## Advanced Type Conversions

### Custom Conversion Functions

```python
def safe_int_conversion(x):
    """Safely convert to int with error handling."""
    try:
        return int(float(x))
    except (ValueError, TypeError):
        return np.nan

df['safe_int'] = df['mixed'].apply(safe_int_conversion)
print(df['safe_int'])
```

### Converting Multiple Columns

```python
# Define conversion mapping
conversions = {
    'integers': 'int32',
    'floats': 'float32',
    'booleans': 'bool'
}

for col, dtype in conversions.items():
    if col in df.columns:
        df[col] = df[col].astype(dtype)

print(df.dtypes)
```

## Best Practices

1. **Check data types early** in your analysis
2. **Use appropriate numeric types** to save memory
3. **Convert strings to categorical** for repeated values
4. **Handle conversion errors** gracefully
5. **Validate conversions** after applying them
6. **Consider performance implications** of data types

```python
# Validation function
def validate_conversions(df, expected_dtypes):
    """Validate that DataFrame has expected data types."""
    for col, expected_dtype in expected_dtypes.items():
        actual_dtype = df[col].dtype
        if actual_dtype != expected_dtype:
            print(f"Warning: {col} has {actual_dtype}, expected {expected_dtype}")

# Usage
expected = {'integers': 'int32', 'floats': 'float32'}
validate_conversions(df, expected)
```
