# Common Pandas Pitfalls

This guide covers the most common mistakes and pitfalls that pandas users encounter. Understanding these issues will help you avoid bugs, performance problems, and frustration.

## Data Selection Issues

### 1. SettingWithCopyWarning

**Problem**: Modifying a DataFrame slice creates unexpected behavior.

```python
# PROBLEMATIC CODE
df = pd.DataFrame({'A': [1, 2, 3, 4], 'B': [10, 20, 30, 40]})
subset = df[df['A'] > 2]  # This creates a view
subset['B'] = 999         # SettingWithCopyWarning!

print(df)  # Original DataFrame is NOT modified as expected
```

**Solution**: Use `.copy()` or `.loc`

```python
# SOLUTION 1: Explicit copy
subset = df[df['A'] > 2].copy()
subset['B'] = 999

# SOLUTION 2: Use .loc for assignment
df.loc[df['A'] > 2, 'B'] = 999
```

### 2. Chained Indexing

**Problem**: Chained indexing can lead to unpredictable results.

```python
# PROBLEMATIC: Chained indexing
df[df['A'] > 2]['B'] = 999  # This doesn't work as expected!

# Why it fails: df[df['A'] > 2] returns a copy, then ['B'] is a view of that copy
```

**Solution**: Use `.loc` for reliable access

```python
# CORRECT: Use .loc
df.loc[df['A'] > 2, 'B'] = 999
```

## Data Type Issues

### 3. Automatic Type Inference Problems

**Problem**: pandas may infer wrong data types, especially with mixed data.

```python
# Mixed data types cause object dtype
df = pd.DataFrame({
    'numbers': [1, 2, '3', 4],  # Mixed int/string
    'booleans': [True, False, 'True', 1]  # Mixed bools
})

print(df.dtypes)
# numbers    object  # Should be int64
# booleans   object  # Should be bool
```

**Solution**: Explicitly specify data types

```python
df = pd.DataFrame({
    'numbers': pd.to_numeric([1, 2, '3', 4], errors='coerce'),
    'booleans': pd.to_numeric([True, False, 'True', 1], errors='coerce').astype(bool)
})
```

### 4. NaN Comparison Issues

**Problem**: NaN values don't compare equal to themselves.

```python
df = pd.DataFrame({'A': [1, 2, float('nan'), 4]})

# This doesn't work as expected
nan_rows = df[df['A'] == float('nan')]  # Returns empty DataFrame!
```

**Solution**: Use `isna()` or `isnull()`

```python
# CORRECT: Use isna()
nan_rows = df[df['A'].isna()]

# Alternative: Use pandas.isna()
import pandas as pd
nan_rows = df[pd.isna(df['A'])]
```

## Performance Issues

### 5. Iterating Over DataFrames

**Problem**: Using loops instead of vectorized operations is slow.

```python
# SLOW: Iterating with iterrows()
df = pd.DataFrame({'A': range(100000), 'B': range(100000, 200000)})

# This is very slow for large DataFrames
for index, row in df.iterrows():
    df.loc[index, 'C'] = row['A'] + row['B']
```

**Solution**: Use vectorized operations

```python
# FAST: Vectorized operation
df['C'] = df['A'] + df['B']
```

### 6. Memory Inefficiency

**Problem**: Large DataFrames can consume excessive memory.

```python
# Inefficient: Default dtypes
df = pd.DataFrame({
    'small_int': [1, 2, 3, 4],      # Uses int64 (8 bytes each)
    'small_float': [1.0, 2.0, 3.0, 4.0],  # Uses float64 (8 bytes each)
    'repeated_string': ['A', 'B', 'A', 'B'] * 1000  # Object dtype
})

print(df.memory_usage(deep=True))
```

**Solution**: Optimize data types

```python
df = pd.DataFrame({
    'small_int': pd.to_numeric([1, 2, 3, 4], downcast='integer'),  # int8
    'small_float': pd.to_numeric([1.0, 2.0, 3.0, 4.0], downcast='float'),  # float32
    'repeated_string': pd.Categorical(['A', 'B', 'A', 'B'] * 1000)  # Category
})
```

## Index Issues

### 7. Index Misalignment

**Problem**: Operations on DataFrames with different indexes can cause issues.

```python
df1 = pd.DataFrame({'A': [1, 2]}, index=[0, 1])
df2 = pd.DataFrame({'B': [3, 4]}, index=[1, 2])

# Addition aligns by index, not position
result = df1 + df2
print(result)
#      A    B
# 0  NaN  NaN
# 1  5.0  NaN  # 2 + 3 = 5, but index 1 from df1 + index 1 from df2
# 2  NaN  NaN
```

**Solution**: Reset indexes or use explicit alignment

```python
# Reset indexes for position-based operations
df1_reset = df1.reset_index(drop=True)
df2_reset = df2.reset_index(drop=True)

# Or align explicitly
result = df1.add(df2, fill_value=0)  # Fill missing values with 0
```

### 8. Modifying Index in Place

**Problem**: Modifying index can cause unexpected behavior.

```python
df = pd.DataFrame({'A': [1, 2, 3]}, index=[0, 1, 2])

# PROBLEMATIC: Modifying index in place
df.index = df.index + 10  # This can cause issues with views/copies
```

**Solution**: Create new DataFrame or use set_index()

```python
# BETTER: Create new DataFrame
df_new = df.set_index(df.index + 10)

# Or assign new index properly
df.index = pd.Index(df.index + 10)
```

## Merge and Join Issues

### 9. Unintended Cartesian Products

**Problem**: Merging without specifying keys creates cartesian product.

```python
df1 = pd.DataFrame({'A': [1, 2], 'key': [1, 1]})
df2 = pd.DataFrame({'B': [3, 4], 'key': [1, 1]})

# Without specifying how='inner', this creates 4 rows (2x2)
result = pd.merge(df1, df2, on='key')  # 4 rows!
print(f"Result has {len(result)} rows")  # 4 rows instead of expected 2
```

**Solution**: Specify merge type explicitly

```python
# Specify merge type
result = pd.merge(df1, df2, on='key', how='left')  # Keeps all rows from df1
result = pd.merge(df1, df2, on='key', how='inner')  # Only matching rows
```

### 10. Column Name Conflicts in Merge

**Problem**: Merging DataFrames with same column names creates ambiguous columns.

```python
df1 = pd.DataFrame({'key': [1, 2], 'value': [10, 20]})
df2 = pd.DataFrame({'key': [1, 2], 'value': [30, 40]})

result = pd.merge(df1, df2, on='key')
print(result.columns)  # ['key', 'value_x', 'value_y'] - confusing!
```

**Solution**: Rename columns before merging or use suffixes

```python
# Use suffixes to clarify column names
result = pd.merge(df1, df2, on='key', suffixes=('_df1', '_df2'))
print(result.columns)  # ['key', 'value_df1', 'value_df2']
```

## GroupBy Issues

### 11. GroupBy Key Errors

**Problem**: Trying to access grouped columns incorrectly.

```python
df = pd.DataFrame({
    'group': ['A', 'A', 'B', 'B'],
    'value': [1, 2, 3, 4]
})

grouped = df.groupby('group')

# PROBLEMATIC: This doesn't work
# print(grouped['value'].mean()['A'])  # KeyError!
```

**Solution**: Access grouped results correctly

```python
# CORRECT: Access as Series
means = grouped['value'].mean()
print(means['A'])  # Works!

# Or convert to DataFrame
means_df = grouped['value'].mean().reset_index()
print(means_df[means_df['group'] == 'A'])
```

### 12. Unexpected GroupBy Behavior

**Problem**: GroupBy includes all columns by default.

```python
df = pd.DataFrame({
    'group': ['A', 'A', 'B', 'B'],
    'value1': [1, 2, 3, 4],
    'value2': [10, 20, 30, 40]
})

# This includes value2 in the result, which might not be desired
grouped = df.groupby('group').sum()
print(grouped)
```

**Solution**: Specify columns explicitly

```python
# Only aggregate specific columns
grouped = df.groupby('group')[['value1', 'value2']].sum()
# Or
grouped = df.groupby('group').agg({'value1': 'sum', 'value2': 'mean'})
```

## DateTime Issues

### 13. Timezone Confusion

**Problem**: Mixing timezone-aware and timezone-naive datetime objects.

```python
import pytz

# PROBLEMATIC: Mixing timezones
dt_naive = pd.Timestamp('2023-01-01')
dt_aware = pd.Timestamp('2023-01-01', tz='UTC')

# This can cause issues
series = pd.Series([dt_naive, dt_aware])
```

**Solution**: Be consistent with timezones

```python
# Make all datetime objects timezone-aware or timezone-naive
dt_utc = pd.Timestamp('2023-01-01', tz='UTC')
dt_est = pd.Timestamp('2023-01-01', tz='US/Eastern')

# Convert to same timezone for comparison
dt_est_utc = dt_est.tz_convert('UTC')
```

### 14. Date Parsing Issues

**Problem**: Automatic date parsing can fail or give wrong results.

```python
# Ambiguous date formats
dates = ['01/02/2023', '02/01/2023']  # Is this Jan 2 or Feb 1?

df = pd.DataFrame({'date': dates})
df['parsed'] = pd.to_datetime(df['date'])  # May parse incorrectly
```

**Solution**: Specify date format explicitly

```python
# Specify format to avoid ambiguity
df['parsed'] = pd.to_datetime(df['date'], format='%m/%d/%Y')  # MM/DD/YYYY
# or
df['parsed'] = pd.to_datetime(df['date'], format='%d/%m/%Y')  # DD/MM/YYYY
```

## File I/O Issues

### 15. Encoding Problems

**Problem**: File encoding issues when reading/writing.

```python
# PROBLEMATIC: Default encoding may not work for all files
df = pd.read_csv('file_with_utf8.csv')  # May fail with special characters
```

**Solution**: Specify encoding explicitly

```python
# Specify encoding
df = pd.read_csv('file_with_utf8.csv', encoding='utf-8')
df = pd.read_csv('file_with_latin.csv', encoding='latin1')
df.to_csv('output.csv', encoding='utf-8', index=False)
```

### 16. Memory Issues with Large Files

**Problem**: Loading entire large files into memory.

```python
# PROBLEMATIC: Loads entire file into memory
df = pd.read_csv('huge_file.csv')  # May cause memory error
```

**Solution**: Use chunked reading

```python
# Read in chunks
chunk_size = 10000
chunks = pd.read_csv('huge_file.csv', chunksize=chunk_size)

# Process each chunk
for chunk in chunks:
    process_chunk(chunk)
```

## String Operation Issues

### 17. Missing str Accessor

**Problem**: Forgetting to use .str accessor for string operations.

```python
df = pd.DataFrame({'text': ['hello', 'world', 'pandas']})

# PROBLEMATIC: This doesn't work
# df['upper'] = df['text'].upper()  # AttributeError!
```

**Solution**: Use .str accessor

```python
# CORRECT: Use .str
df['upper'] = df['text'].str.upper()
df['length'] = df['text'].str.len()
df['contains_a'] = df['text'].str.contains('a')
```

## Statistical Operation Issues

### 18. NaN Propagation in Aggregations

**Problem**: NaN values affect statistical operations.

```python
df = pd.DataFrame({'values': [1, 2, float('nan'), 4, 5]})

print(df['values'].mean())  # NaN - mean includes NaN
print(df['values'].sum())   # NaN - sum includes NaN
```

**Solution**: Handle NaN values appropriately

```python
# Skip NaN values
print(df['values'].mean(skipna=True))   # 3.0
print(df['values'].sum(skipna=True))    # 12.0

# Or remove NaN values first
clean_values = df['values'].dropna()
print(clean_values.mean())  # 3.0
```

## Best Practices to Avoid Pitfalls

1. **Always use .loc and .iloc** for DataFrame indexing
2. **Use .copy()** when creating subsets for modification
3. **Specify data types explicitly** when creating DataFrames
4. **Handle NaN values appropriately** in comparisons and aggregations
5. **Use vectorized operations** instead of loops
6. **Specify encodings** when reading/writing files
7. **Use chunked reading** for large files
8. **Be consistent with timezones** in datetime operations
9. **Specify merge types** explicitly to avoid cartesian products
10. **Test operations on small samples** before applying to full datasets

## Debugging Tips

### Enable Warnings

```python
import warnings
warnings.filterwarnings('default')  # Show all pandas warnings

# Or be specific
warnings.filterwarnings('error', category=pd.errors.PerformanceWarning)
```

### Validate Results

```python
# Add assertions to check results
assert len(df) == expected_length, f"Expected {expected_length} rows, got {len(df)}"
assert df['column'].notnull().all(), "Column should not contain null values"
assert df['numeric_col'].dtype == 'int64', "Column should be int64"
```

### Use Debug Mode

```python
# Enable pandas debugging
pd.set_option('mode.chained_assignment', 'warn')  # Default
pd.set_option('mode.chained_assignment', 'raise')  # Strict mode

# Check for common issues
print(df.info())
print(df.describe())
print(df.isnull().sum())
```

Remember: When you encounter unexpected behavior in pandas, it's often related to one of these common pitfalls. The pandas documentation and community forums are excellent resources for troubleshooting specific issues.
