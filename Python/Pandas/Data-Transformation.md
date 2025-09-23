# Data Transformation

Pandas provides powerful tools for transforming and manipulating data using functions like apply(), map(), and transform().

## Using apply()

### Apply to Series

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'salary': [50000, 60000, 70000]
})

# Apply function to each element
df['name_upper'] = df['name'].apply(str.upper)
df['salary_log'] = df['salary'].apply(np.log)
print(df)
```

### Apply to DataFrame Rows/Columns

```python
# Apply function to each row
def calculate_bonus(row):
    base = row['salary'] * 0.1
    if row['name'].startswith('A'):
        return base * 1.2  # 20% bonus for names starting with A
    return base

df['bonus'] = df.apply(calculate_bonus, axis=1)
print(df)
```

## Using map()

```python
# Map values using dictionary
grade_map = {'A': 4.0, 'B': 3.0, 'C': 2.0, 'D': 1.0, 'F': 0.0}
grades_df = pd.DataFrame({'grade': ['A', 'B', 'C', 'A', 'B']})
grades_df['points'] = grades_df['grade'].map(grade_map)
print(grades_df)
```

## Using transform()

```python
# Transform maintains same shape as original
df['salary_zscore'] = df.groupby('department')['salary'].transform(
    lambda x: (x - x.mean()) / x.std()
)
print(df)
```

## String Transformations

```python
text_df = pd.DataFrame({
    'text': ['Hello World', 'PYTHON programming', 'Data Science']
})

# String operations
text_df['upper'] = text_df['text'].str.upper()
text_df['length'] = text_df['text'].str.len()
text_df['contains_python'] = text_df['text'].str.contains('PYTHON', case=False)
print(text_df)
```

## Numeric Transformations

```python
numeric_df = pd.DataFrame({
    'values': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
})

# Mathematical transformations
numeric_df['squared'] = numeric_df['values'] ** 2
numeric_df['sqrt'] = np.sqrt(numeric_df['values'])
numeric_df['log'] = np.log(numeric_df['values'])

# Statistical transformations
numeric_df['zscore'] = (numeric_df['values'] - numeric_df['values'].mean()) / numeric_df['values'].std()
print(numeric_df)
```

## Categorical Transformations

```python
# Convert to categorical and add codes
df['category'] = pd.Categorical(df['department'])
df['category_codes'] = df['category'].cat.codes
print(df)
```

## Best Practices

1. **Use vectorized operations** when possible (faster than apply/map)
2. **Choose appropriate transformation method** based on your needs
3. **Handle edge cases** in custom functions
4. **Validate transformations** to ensure correctness
5. **Consider performance** for large datasets

```python
# Performance comparison
import time

large_df = pd.DataFrame({'x': range(100000)})

# Vectorized (fast)
start = time.time()
large_df['x_squared'] = large_df['x'] ** 2
vectorized_time = time.time() - start

# Apply (slower)
start = time.time()
large_df['x_squared_apply'] = large_df['x'].apply(lambda x: x ** 2)
apply_time = time.time() - start

print(f"Vectorized: {vectorized_time:.4f}s")
print(f"Apply: {apply_time:.4f}s")
print(f"Apply is {apply_time/vectorized_time:.1f}x slower")
```
