# Performance Tips

Optimizing pandas performance is crucial when working with large datasets. Here are key techniques to improve speed and memory usage.

## Vectorized Operations

```python
import pandas as pd
import numpy as np
import time

# Create large dataset
df = pd.DataFrame({
    'A': np.random.randn(100000),
    'B': np.random.randn(100000),
    'C': np.random.randn(100000)
})

# GOOD: Vectorized operations
start = time.time()
df['D'] = df['A'] + df['B'] * df['C']
vectorized_time = time.time() - start

# BAD: Iterating with loops
start = time.time()
d_values = []
for index, row in df.iterrows():
    d_values.append(row['A'] + row['B'] * row['C'])
df['D_loop'] = d_values
loop_time = time.time() - start

print(f"Vectorized: {vectorized_time:.4f}s")
print(f"Loop: {loop_time:.4f}s")
print(f"Vectorized is {loop_time/vectorized_time:.0f}x faster")
```

## Efficient Data Types

### Memory Usage Analysis

```python
# Check memory usage
print(df.memory_usage(deep=True))
print(f"Total memory: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")

# Memory usage by data type
print(df.memory_usage(deep=True) / 1024**2)  # In MB
```

### Optimizing Data Types

```python
# Convert to efficient types
df_optimized = df.copy()

# Downcast numeric types
df_optimized['A'] = pd.to_numeric(df_optimized['A'], downcast='float')
df_optimized['B'] = pd.to_numeric(df_optimized['B'], downcast='float')
df_optimized['C'] = pd.to_numeric(df_optimized['C'], downcast='float')

# Convert repeated strings to categorical
df_cat = pd.DataFrame({
    'category': ['A', 'B', 'C'] * 10000,
    'value': np.random.randn(30000)
})

print("Before categorical:")
print(df_cat.memory_usage(deep=True))

df_cat['category'] = df_cat['category'].astype('category')

print("After categorical:")
print(df_cat.memory_usage(deep=True))
```

## Chunked Processing

```python
# Process large files in chunks
chunk_size = 10000

def process_chunk(chunk):
    # Perform operations on chunk
    chunk['new_col'] = chunk['A'] * 2
    return chunk

# Reading in chunks
chunks = []
for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    processed_chunk = process_chunk(chunk)
    chunks.append(processed_chunk)

# Combine results
result = pd.concat(chunks, ignore_index=True)
print(f"Processed {len(result)} rows")
```

## Efficient Selection

```python
# GOOD: Use .loc for label-based selection
subset = df.loc[df['A'] > 0, ['A', 'B']]

# BAD: Chained indexing (creates copies)
# subset = df[df['A'] > 0]['B']

# GOOD: Use query for complex conditions
filtered = df.query('A > 0 and B < 1')
```

## Caching and Reuse

```python
# Cache expensive operations
@pd.util.cache_readonly
def expensive_computation(self):
    # Expensive calculation
    return self['A'].rolling(30).mean()

# Reuse computed results
df['rolling_mean'] = expensive_computation(df)
```

## Parallel Processing

```python
from multiprocessing import Pool
import pandas as pd

def process_group(group):
    # Process each group
    return group['value'].mean()

# Split DataFrame and process in parallel
groups = [group for name, group in df.groupby('category')]

with Pool(processes=4) as pool:
    results = pool.map(process_group, groups)

print(results)
```

## Profiling Performance

```python
import cProfile
import pstats

def profile_function(func, *args, **kwargs):
    profiler = cProfile.Profile()
    profiler.enable()
    result = func(*args, **kwargs)
    profiler.disable()

    stats = pstats.Stats(profiler)
    stats.sort_stats('cumulative').print_stats(10)  # Top 10 functions
    return result

# Usage
result = profile_function(my_slow_function, df)
```

## Best Practices

### 1. Avoid Loops When Possible

```python
# Instead of loops, use:
# - Vectorized operations
# - apply() only when necessary
# - pandas built-in methods
```

### 2. Use Appropriate Data Structures

```python
# For time series, set datetime index
df.index = pd.to_datetime(df['date'])
df = df.drop('date', axis=1)

# Use categorical for repeated strings
df['category'] = df['category'].astype('category')
```

### 3. Optimize I/O Operations

```python
# Use efficient file formats
df.to_parquet('data.parquet')  # Faster than CSV
df.to_feather('data.feather')  # Fastest for pandas-pandas

# Use compression
df.to_csv('data.csv.gz', compression='gzip')
```

### 4. Memory-Efficient Operations

```python
# Delete unused DataFrames
del large_df
import gc
gc.collect()

# Use copy=False when possible
df_view = df[['A', 'B']].copy(deep=False)
```

### 5. Monitor Performance

```python
import time
import psutil

def monitor_performance(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        start_memory = psutil.Process().memory_info().rss / 1024**2  # MB

        result = func(*args, **kwargs)

        end_time = time.time()
        end_memory = psutil.Process().memory_info().rss / 1024**2

        print(f"Execution time: {end_time - start_time:.2f}s")
        print(f"Memory usage: {end_memory - start_memory:.2f}MB")

        return result
    return wrapper

@monitor_performance
def my_data_operation(df):
    return df.groupby('category')['value'].mean()

result = my_data_operation(df)
```

## Common Performance Issues

### 1. Copying DataFrames Unnecessarily

```python
# BAD: Creates unnecessary copy
df_modified = df.copy()
df_modified['new_col'] = df_modified['A'] * 2

# GOOD: Modify in place when possible
df['new_col'] = df['A'] * 2
```

### 2. Using object dtype for numeric data

```python
# BAD: Object dtype for numbers
df_bad = pd.DataFrame({'numbers': ['1', '2', '3']})
print(df_bad['numbers'].dtype)  # object

# GOOD: Proper numeric dtype
df_good = pd.DataFrame({'numbers': [1, 2, 3]})
print(df_good['numbers'].dtype)  # int64
```

### 3. Not using indexes effectively

```python
# BAD: Searching without index
result = df[df['id'] == target_id]

# GOOD: Set index for fast lookups
df_indexed = df.set_index('id')
result = df_indexed.loc[target_id]
```

## Scaling to Large Datasets

### Dask for Very Large Data

```python
# For datasets that don't fit in memory
import dask.dataframe as dd

# Read large CSV with Dask
ddf = dd.read_csv('very_large_file.csv')

# Perform operations (lazy evaluation)
result = ddf.groupby('category')['value'].mean()

# Compute when needed
final_result = result.compute()
```

### Database Integration

```python
# For very large datasets, consider database operations
import sqlalchemy as sa

engine = sa.create_engine('postgresql://user:pass@localhost/db')

# Perform aggregation in database
query = """
SELECT category, AVG(value) as avg_value
FROM large_table
GROUP BY category
"""

result = pd.read_sql(query, engine)
```

This comprehensive guide covers essential performance optimization techniques for pandas, from basic vectorized operations to advanced parallel processing and memory management strategies.
