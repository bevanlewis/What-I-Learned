# Pandas Series

A Series is a one-dimensional labeled array capable of holding any data type (integers, strings, floating point numbers, Python objects, etc.). It's similar to a column in a spreadsheet or a single column of data in a database table.

## Creating Series

### From a List

```python
import pandas as pd

# Basic series
s = pd.Series([1, 3, 5, 6, 8])
print(s)
```

### With Custom Index

```python
# Series with custom index
s = pd.Series([1, 3, 5, 6, 8], index=['a', 'b', 'c', 'd', 'e'])
print(s)
```

### From a Dictionary

```python
# Series from dictionary
data = {'a': 1, 'b': 2, 'c': 3}
s = pd.Series(data)
print(s)
```

### From NumPy Array

```python
import numpy as np

# From numpy array
arr = np.array([1, 2, 3, 4, 5])
s = pd.Series(arr)
print(s)
```

### With Specific Data Type

```python
# Specify data type
s = pd.Series([1, 2, 3], dtype='float64')
print(s.dtype)
```

## Series Attributes

```python
s = pd.Series([1, 2, 3, 4, 5], index=['a', 'b', 'c', 'd', 'e'])

print(s.index)      # Index labels
print(s.values)     # Underlying data
print(s.dtype)      # Data type
print(s.shape)      # Shape (rows,)
print(s.size)       # Number of elements
print(s.ndim)       # Number of dimensions (1 for Series)
```

## Accessing Series Elements

### By Position (iloc)

```python
s = pd.Series([10, 20, 30, 40, 50])

print(s.iloc[0])    # First element: 10
print(s.iloc[-1])   # Last element: 50
print(s.iloc[1:4])  # Elements 1, 2, 3: [20, 30, 40]
```

### By Label (loc)

```python
s = pd.Series([10, 20, 30, 40, 50], index=['a', 'b', 'c', 'd', 'e'])

print(s.loc['a'])       # Element with label 'a': 10
print(s.loc['b':'d'])   # Elements from 'b' to 'd': [20, 30, 40]
```

### Boolean Indexing

```python
s = pd.Series([10, 20, 30, 40, 50])

print(s[s > 30])        # Elements greater than 30: [40, 50]
print(s[s % 20 == 0])   # Elements divisible by 20: [20, 40]
```

## Series Operations

### Arithmetic Operations

```python
s1 = pd.Series([1, 2, 3, 4, 5])
s2 = pd.Series([10, 20, 30, 40, 50])

print(s1 + s2)  # Addition: [11, 22, 33, 44, 55]
print(s1 * 2)   # Scalar multiplication: [2, 4, 6, 8, 10]
print(s1 ** 2)  # Power: [1, 4, 9, 16, 25]
```

### Statistical Operations

```python
s = pd.Series([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

print(s.sum())      # Sum: 55
print(s.mean())     # Mean: 5.5
print(s.median())   # Median: 5.5
print(s.std())      # Standard deviation
print(s.min())      # Minimum: 1
print(s.max())      # Maximum: 10
print(s.count())    # Count: 10
```

### String Operations

```python
s = pd.Series(['apple', 'banana', 'cherry'])

print(s.str.upper())      # ['APPLE', 'BANANA', 'CHERRY']
print(s.str.len())        # [5, 6, 6]
print(s.str.contains('a')) # [True, True, False]
```

## Modifying Series

### Changing Values

```python
s = pd.Series([1, 2, 3, 4, 5])

s.iloc[0] = 10        # Change by position
s.loc['a'] = 20       # Change by label (if index exists)
s[s > 3] = 0          # Change multiple values with condition

print(s)
```

### Adding Elements

```python
s = pd.Series([1, 2, 3])

s.loc['d'] = 4        # Add with label
s = s.append(pd.Series([5]))  # Append another series

print(s)
```

### Removing Elements

```python
s = pd.Series([1, 2, 3, 4, 5])

s = s.drop(0)         # Remove by index
s = s.drop('a')       # Remove by label (if index exists)

print(s)
```

## Series Methods

### head() and tail()

```python
s = pd.Series(range(1, 101))

print(s.head())   # First 5 elements
print(s.tail())   # Last 5 elements
print(s.head(3))  # First 3 elements
```

### unique() and value_counts()

```python
s = pd.Series(['a', 'b', 'a', 'c', 'b', 'a'])

print(s.unique())          # ['a', 'b', 'c']
print(s.value_counts())    # Count of each unique value
```

### isnull() and notnull()

```python
s = pd.Series([1, 2, None, 4, None])

print(s.isnull())   # [False, False, True, False, True]
print(s.notnull())  # [True, True, False, True, False]
```

### fillna()

```python
s = pd.Series([1, 2, None, 4, None])

print(s.fillna(0))      # Fill with 0
print(s.fillna(s.mean()))  # Fill with mean
```

## Series with Different Data Types

### Numeric Series

```python
# Integer series
int_series = pd.Series([1, 2, 3, 4, 5], dtype='int64')

# Float series
float_series = pd.Series([1.1, 2.2, 3.3, 4.4, 5.5])
```

### String Series

```python
# String series
str_series = pd.Series(['apple', 'banana', 'cherry'])
```

### Boolean Series

```python
# Boolean series
bool_series = pd.Series([True, False, True, False])
```

### DateTime Series

```python
# DateTime series
date_series = pd.Series(pd.date_range('2023-01-01', periods=5))
print(date_series)
```

## Series Comparison

```python
s1 = pd.Series([1, 2, 3])
s2 = pd.Series([1, 2, 4])

print(s1 == s2)  # Element-wise comparison: [True, True, False]
print(s1 > s2)   # [False, False, False]
```

## Converting Series

### To List

```python
s = pd.Series([1, 2, 3, 4, 5])
print(s.tolist())  # [1, 2, 3, 4, 5]
```

### To Dictionary

```python
s = pd.Series([1, 2, 3], index=['a', 'b', 'c'])
print(s.to_dict())  # {'a': 1, 'b': 2, 'c': 3}
```

### To NumPy Array

```python
s = pd.Series([1, 2, 3, 4, 5])
print(s.to_numpy())  # array([1, 2, 3, 4, 5])
```

## Series Iteration

```python
s = pd.Series([1, 2, 3], index=['a', 'b', 'c'])

# Iterate over values
for value in s:
    print(value)

# Iterate over index-value pairs
for index, value in s.items():
    print(f"{index}: {value}")
```

## Best Practices

1. **Use meaningful index labels** when appropriate
2. **Specify data types** explicitly when creating Series
3. **Use vectorized operations** instead of loops
4. **Handle missing values** appropriately
5. **Use loc/iloc** for reliable indexing
6. **Consider memory usage** for large Series

## Common Use Cases

- **Time series data**: Series with datetime index
- **Categorical data**: Series with category dtype
- **Numeric computations**: Mathematical operations on numeric series
- **Text processing**: String operations on string series
- **Boolean masking**: Using boolean series for filtering data
