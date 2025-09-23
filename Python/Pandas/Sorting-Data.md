# Sorting Data in Pandas

Pandas provides powerful and flexible sorting capabilities for DataFrames and Series. This section covers various sorting techniques and best practices.

## Basic Sorting

### Sorting by Single Column

```python
import pandas as pd

# Sample DataFrame
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'Age': [25, 30, 35, 28],
    'Salary': [50000, 60000, 70000, 55000],
    'Department': ['Engineering', 'Marketing', 'Sales', 'HR']
})

# Sort by Age (ascending by default)
df_sorted = df.sort_values('Age')
print(df_sorted)

# Sort by Salary (descending)
df_sorted_desc = df.sort_values('Salary', ascending=False)
print(df_sorted_desc)
```

### Sorting by Multiple Columns

```python
# Sort by Department, then by Age within each department
df_sorted_multi = df.sort_values(['Department', 'Age'])
print(df_sorted_multi)

# Sort with different orders for each column
df_sorted_mixed = df.sort_values(['Department', 'Salary'],
                                ascending=[True, False])
print(df_sorted_mixed)
```

## Sorting Series

```python
# Sort Series values
ages = df['Age'].sort_values()
print(ages)

# Sort Series values in descending order
salaries_desc = df['Salary'].sort_values(ascending=False)
print(salaries_desc)

# Sort by index
ages_by_index = df['Age'].sort_index()
print(ages_by_index)
```

## Sorting by Index

```python
# Sort DataFrame by index
df_index_sorted = df.sort_index()
print(df_index_sorted)

# Sort by index in descending order
df_index_desc = df.sort_index(ascending=False)
print(df_index_desc)

# Sort by specific level in MultiIndex
# (assuming we have a MultiIndex DataFrame)
# df_multi = df.set_index(['Department', 'Name'])
# df_sorted_level = df_multi.sort_index(level='Department')
```

## Advanced Sorting Techniques

### Sorting with Custom Key Functions

```python
# Sort by string length
df['Name_Length'] = df['Name'].str.len()
df_sorted_length = df.sort_values('Name_Length')
print(df_sorted_length)

# Sort by custom function
def custom_sort_key(name):
    return len(name)  # Sort by name length

df_custom_sort = df.sort_values('Name', key=lambda x: x.str.len())
print(df_custom_sort)
```

### Sorting with NaN Values

```python
# DataFrame with NaN values
df_nan = pd.DataFrame({
    'A': [1, 3, float('nan'), 2],
    'B': [4, float('nan'), 6, 5]
})

# NaN values are placed at the end by default
df_sorted_nan = df_nan.sort_values('A')
print(df_sorted_nan)

# Place NaN values at the beginning
df_nan_first = df_nan.sort_values('A', na_position='first')
print(df_nan_first)
```

### Stable Sort

```python
# Stable sort maintains relative order of equal elements
df_stable = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Alice', 'Charlie'],
    'Score': [85, 90, 85, 88]
})

# Stable sort by Score, then Name
df_stable_sorted = df_stable.sort_values(['Score', 'Name'], kind='stable')
print(df_stable_sorted)

# Mergesort (default for stable sorting)
df_mergesort = df_stable.sort_values('Score', kind='mergesort')
print(df_mergesort)
```

## Partial Sorting

### nlargest() and nsmallest()

```python
# Get top 3 salaries
top_3_salaries = df.nlargest(3, 'Salary')
print(top_3_salaries)

# Get bottom 2 ages
youngest_2 = df.nsmallest(2, 'Age')
print(youngest_2)

# Multiple columns for tie-breaking
top_by_salary_age = df.nlargest(3, ['Salary', 'Age'])
print(top_by_salary_age)
```

## Sorting Categorical Data

```python
# Convert to categorical for custom sort order
df['Department'] = pd.Categorical(df['Department'],
                                 categories=['Engineering', 'Marketing', 'Sales', 'HR'],
                                 ordered=True)

df_cat_sorted = df.sort_values('Department')
print(df_cat_sorted)
```

## Sorting Date/Time Data

```python
# DataFrame with dates
df_dates = pd.DataFrame({
    'Date': pd.to_datetime(['2023-03-01', '2023-01-15', '2023-02-10']),
    'Value': [100, 200, 150]
})

# Sort by date
df_date_sorted = df_dates.sort_values('Date')
print(df_date_sorted)

# Sort by month/day regardless of year
df_dates['Month_Day'] = df_dates['Date'].dt.strftime('%m-%d')
df_month_day_sorted = df_dates.sort_values('Month_Day')
print(df_month_day_sorted)
```

## Performance Considerations

### In-Place Sorting

```python
# In-place sorting (modifies original DataFrame)
df_copy = df.copy()
df_copy.sort_values('Age', inplace=True)
print(df_copy)

# Regular sorting (returns new DataFrame)
df_sorted = df.sort_values('Age')
```

### Sorting Large DataFrames

```python
# For large DataFrames, consider memory usage
large_df = pd.DataFrame({
    'A': range(100000),
    'B': range(100000, 200000)
})

# Sort with specific algorithm for memory efficiency
large_sorted = large_df.sort_values('B', kind='mergesort')
print(large_sorted.head())
```

## Sorting with GroupBy

```python
# Sort within groups
grouped_sorted = df.groupby('Department').apply(lambda x: x.sort_values('Salary', ascending=False))
print(grouped_sorted)

# Sort groups themselves
group_sizes = df.groupby('Department').size().sort_values(ascending=False)
print(group_sizes)
```

## Custom Sorting Orders

### Using sort_values with key parameter

```python
# Sort strings by length
df_name_sorted = df.sort_values('Name', key=lambda x: x.str.len())
print(df_name_sorted)

# Sort by absolute value
df_numbers = pd.DataFrame({'values': [3, -1, 2, -4, 0]})
df_abs_sorted = df_numbers.sort_values('values', key=abs)
print(df_abs_sorted)
```

### Custom sort for complex data

```python
# Sort by priority (custom order)
priority_order = {'High': 0, 'Medium': 1, 'Low': 2}
df_priority = pd.DataFrame({
    'Task': ['Task A', 'Task B', 'Task C'],
    'Priority': ['High', 'Low', 'Medium']
})

df_priority_sorted = df_priority.sort_values('Priority',
                                           key=lambda x: x.map(priority_order))
print(df_priority_sorted)
```

## Sorting Tips and Best Practices

### Consistent Sorting

```python
# Always specify sort parameters explicitly for consistency
df_sorted = df.sort_values(['Department', 'Name'],
                          ascending=[True, True],
                          na_position='last',
                          kind='mergesort')
```

### Handling Ties

```python
# Use multiple columns to handle ties consistently
df_tie_sorted = df.sort_values(['Salary', 'Age', 'Name'],
                              ascending=[False, False, True])
print(df_tie_sorted)
```

### Memory-Efficient Sorting

```python
# For memory-constrained environments
def memory_efficient_sort(df, column, ascending=True, chunksize=10000):
    """Sort large DataFrame in chunks"""
    if len(df) <= chunksize:
        return df.sort_values(column, ascending=ascending)

    # Sort in chunks and combine
    chunks = []
    for i in range(0, len(df), chunksize):
        chunk = df.iloc[i:i+chunksize].sort_values(column, ascending=ascending)
        chunks.append(chunk)

    return pd.concat(chunks).sort_values(column, ascending=ascending)

# Usage
large_sorted = memory_efficient_sort(large_df, 'A')
```

## Common Sorting Patterns

### Ranking and Sorting

```python
# Add rank column
df['Salary_Rank'] = df['Salary'].rank(ascending=False, method='dense')

# Sort by rank
df_rank_sorted = df.sort_values('Salary_Rank')
print(df_rank_sorted)
```

### Sorting by Frequency

```python
# Sort DataFrame by frequency of values in a column
value_counts = df['Department'].value_counts()
df_freq_sorted = df.set_index('Department').loc[value_counts.index].reset_index()
print(df_freq_sorted)
```

### Conditional Sorting

```python
# Sort differently based on conditions
def conditional_sort(df):
    # Sort high salaries descending, others ascending
    high_salary = df[df['Salary'] > 60000].sort_values('Salary', ascending=False)
    low_salary = df[df['Salary'] <= 60000].sort_values('Salary', ascending=True)
    return pd.concat([high_salary, low_salary])

df_cond_sorted = conditional_sort(df)
print(df_cond_sorted)
```

### Sorting with Custom Comparison

```python
# Sort by complex business logic
def business_sort_score(row):
    score = 0
    score += row['Salary'] / 1000  # Salary contribution
    score += row['Age'] * 10       # Age contribution
    if row['Department'] == 'Engineering':
        score += 100  # Department bonus
    return score

df['Sort_Score'] = df.apply(business_sort_score, axis=1)
df_business_sorted = df.sort_values('Sort_Score', ascending=False)
print(df_business_sorted[['Name', 'Sort_Score']])
```

## Sorting Validation

```python
# Verify sorting worked as expected
def verify_sort(df, column, ascending=True):
    """Verify DataFrame is sorted by column"""
    if ascending:
        is_sorted = df[column].is_monotonic_increasing
    else:
        is_sorted = df[column].is_monotonic_decreasing

    return is_sorted

# Usage
df_sorted = df.sort_values('Age')
print(f"Is sorted by Age: {verify_sort(df_sorted, 'Age')}")
```

## Best Practices

1. **Specify sort parameters explicitly** for reproducible results
2. **Use appropriate sorting algorithms** for your data size
3. **Handle NaN values appropriately** with na_position
4. **Consider memory usage** for large DataFrames
5. **Use stable sort** when relative order of equal elements matters
6. **Combine sorting with other operations** efficiently
7. **Validate sorting results** when critical
8. **Document custom sorting logic** clearly

## Performance Comparison

```python
import time

# Compare sorting methods
df_large = pd.DataFrame({
    'A': range(100000),
    'B': range(100000, 200000)
})

# Time different sorting approaches
start = time.time()
df_quicksort = df_large.sort_values('A', kind='quicksort')
quicksort_time = time.time() - start

start = time.time()
df_mergesort = df_large.sort_values('A', kind='mergesort')
mergesort_time = time.time() - start

print(f"Quicksort time: {quicksort_time:.4f} seconds")
print(f"Mergesort time: {mergesort_time:.4f} seconds")
```
