# Grouping and Aggregation in Pandas

Grouping and aggregation are powerful operations that allow you to analyze data by categories and compute summary statistics.

## Basic GroupBy Operations

### GroupBy Single Column

```python
import pandas as pd
import numpy as np

# Sample DataFrame
df = pd.DataFrame({
    'department': ['Engineering', 'Sales', 'Engineering', 'Sales', 'HR', 'HR'],
    'employee': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank'],
    'salary': [75000, 65000, 80000, 70000, 60000, 55000],
    'experience': [5, 3, 7, 4, 2, 1]
})

# Group by department
grouped = df.groupby('department')
print(grouped)  # <pandas.core.groupby.generic.DataFrameGroupBy object>

# Get group sizes
print(grouped.size())
```

### Common Aggregation Functions

```python
# Mean salary by department
print(grouped['salary'].mean())

# Multiple statistics
print(grouped['salary'].agg(['mean', 'sum', 'count', 'max', 'min']))

# Multiple columns
print(grouped[['salary', 'experience']].mean())
```

## Advanced Aggregation

### Custom Aggregation Functions

```python
def salary_range(x):
    """Calculate salary range (max - min)"""
    return x.max() - x.min()

# Apply custom function
print(grouped['salary'].agg(salary_range))

# Multiple custom functions
print(grouped['salary'].agg(['mean', salary_range]))
```

### Named Aggregation (pandas 0.25+)

```python
# Named aggregation for multiple columns with different functions
result = df.groupby('department').agg(
    avg_salary=('salary', 'mean'),
    total_salary=('salary', 'sum'),
    employee_count=('employee', 'count'),
    avg_experience=('experience', 'mean')
)

print(result)
```

### Dictionary-based Aggregation

```python
# Different aggregations for different columns
aggregations = {
    'salary': ['mean', 'sum', 'std'],
    'experience': ['mean', 'max'],
    'employee': 'count'  # Single function
}

result = grouped.agg(aggregations)
print(result)
```

## Multiple Grouping Columns

```python
# Add more data for multi-level grouping
df_extended = pd.DataFrame({
    'department': ['Engineering', 'Sales', 'Engineering', 'Sales', 'HR', 'HR', 'Engineering'],
    'location': ['NYC', 'LA', 'NYC', 'LA', 'NYC', 'LA', 'LA'],
    'employee': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank', 'Grace'],
    'salary': [75000, 65000, 80000, 70000, 60000, 55000, 78000]
})

# Group by multiple columns
multi_grouped = df_extended.groupby(['department', 'location'])
print(multi_grouped['salary'].mean())
```

## GroupBy Object Methods

### Iterating Over Groups

```python
# Iterate over groups
for name, group in grouped:
    print(f"\nDepartment: {name}")
    print(group[['employee', 'salary']])
```

### Get Specific Groups

```python
# Get a specific group
engineering = grouped.get_group('Engineering')
print(engineering)

# Check group names
print(grouped.groups.keys())
```

### Group Properties

```python
# Number of groups
print(len(grouped))

# Group indices
print(grouped.indices)

# Groups as dictionary
groups_dict = dict(list(grouped))
print(groups_dict.keys())
```

## Transform Operations

### transform() Method

```python
# Add department average salary to each employee
df['dept_avg_salary'] = grouped['salary'].transform('mean')
print(df[['employee', 'department', 'salary', 'dept_avg_salary']])

# Standardize salaries within department
df['salary_std'] = grouped['salary'].transform(lambda x: (x - x.mean()) / x.std())
print(df[['employee', 'department', 'salary_std']])
```

### apply() Method

```python
# Apply custom function to each group
def top_earner(group):
    return group.nlargest(1, 'salary')[['employee', 'salary']]

top_earners = grouped.apply(top_earner)
print(top_earners)
```

## Filtering Groups

### filter() Method

```python
# Keep only departments with average salary > 65000
high_salary_depts = grouped.filter(lambda x: x['salary'].mean() > 65000)
print(high_salary_depts)

# Keep departments with at least 2 employees
large_depts = grouped.filter(lambda x: len(x) >= 2)
print(large_depts)
```

## Grouping with Time Series

```python
# Create time series data
dates = pd.date_range('2023-01-01', periods=100, freq='D')
ts_df = pd.DataFrame({
    'date': dates,
    'value': np.random.randn(100),
    'category': np.random.choice(['A', 'B', 'C'], 100)
})

# Group by month
ts_df['month'] = ts_df['date'].dt.to_period('M')
monthly_avg = ts_df.groupby('month')['value'].mean()
print(monthly_avg)

# Group by multiple time periods
ts_df['year'] = ts_df['date'].dt.year
ts_df['quarter'] = ts_df['date'].dt.quarter
year_quarter_avg = ts_df.groupby(['year', 'quarter'])['value'].mean()
print(year_quarter_avg)
```

## Advanced Grouping Techniques

### Grouping with Bins

```python
# Group salaries into bins
salary_bins = pd.cut(df['salary'], bins=[0, 60000, 70000, 100000], labels=['Low', 'Medium', 'High'])
binned_stats = df.groupby(salary_bins)['experience'].mean()
print(binned_stats)
```

### Grouping with Functions

```python
# Group by first letter of employee name
def first_letter(name):
    return name[0]

letter_groups = df.groupby(df['employee'].apply(first_letter))
print(letter_groups['salary'].mean())
```

### Grouping with pd.Grouper

```python
# Advanced time grouping
ts_grouped = ts_df.groupby(pd.Grouper(key='date', freq='W'))['value'].mean()
print(ts_grouped)
```

## Performance Considerations

### Efficient Grouping

```python
# For large DataFrames, categorical data can improve performance
df['department'] = df['department'].astype('category')

# Use observed=True for categorical with unobserved categories
df.groupby('department', observed=True)['salary'].mean()
```

### Memory Usage

```python
# GroupBy can create intermediate objects
# Be mindful of memory with large DataFrames

# Instead of storing intermediate results
result = df.groupby('department')['salary'].mean()

# Consider processing in chunks for very large data
def process_large_groupby(df, group_col, agg_col, chunk_size=10000):
    results = []
    for i in range(0, len(df), chunk_size):
        chunk = df.iloc[i:i+chunk_size]
        chunk_result = chunk.groupby(group_col)[agg_col].mean()
        results.append(chunk_result)

    return pd.concat(results).groupby(level=0).mean()
```

## Common Patterns

### Pivot Table Alternative

```python
# GroupBy as alternative to pivot tables
pivot_alternative = df.groupby(['department', 'location'])['salary'].mean().unstack()
print(pivot_alternative)
```

### Ranking Within Groups

```python
# Rank salaries within each department
df['dept_salary_rank'] = df.groupby('department')['salary'].rank(ascending=False)
print(df[['employee', 'department', 'salary', 'dept_salary_rank']])
```

### Cumulative Operations by Group

```python
# Cumulative sum by department
df = df.sort_values(['department', 'salary'])
df['cumulative_salary'] = df.groupby('department')['salary'].cumsum()
print(df[['employee', 'department', 'salary', 'cumulative_salary']])
```

### Shifting Within Groups

```python
# Compare with previous employee in department (sorted by salary)
df['prev_salary'] = df.groupby('department')['salary'].shift(1)
df['salary_diff'] = df['salary'] - df['prev_salary']
print(df[['employee', 'department', 'salary', 'prev_salary', 'salary_diff']])
```

## Best Practices

1. **Sort data before grouping** when order matters
2. **Use categorical data types** for grouping columns with repeated values
3. **Choose appropriate aggregation functions** based on your data
4. **Use named aggregation** for clarity with multiple operations
5. **Consider memory usage** with large DataFrames
6. **Validate group sizes** before complex operations
7. **Use transform()** when you want to return a Series with the same shape as the input
8. **Use apply()** when you need to perform complex operations on each group

## Common Issues and Solutions

### Empty Groups

```python
# Handle empty groups
result = grouped['salary'].mean()
print(result.dropna())  # Remove NaN for empty groups
```

### Memory Issues with Large Groups

```python
# For very large groups, consider using dask or other parallel processing
# or break down the operation

# Instead of:
# large_result = large_df.groupby('category').agg({'col1': 'sum', 'col2': 'mean'})

# Use chunked processing:
def chunked_groupby(df, group_col, operations, chunk_size=50000):
    results = []
    for i in range(0, len(df), chunk_size):
        chunk = df.iloc[i:i+chunk_size]
        chunk_result = chunk.groupby(group_col).agg(operations)
        results.append(chunk_result)

    # Combine results
    combined = pd.concat(results)
    final_result = combined.groupby(level=0).agg({col: 'sum' if op == 'sum' else 'mean'
                                                 for col, op in operations.items()})
    return final_result
```

### GroupBy Key Errors

```python
# Ensure grouping column exists
if 'department' in df.columns:
    grouped = df.groupby('department')
else:
    raise ValueError("Department column not found")
```

This comprehensive guide covers the essential techniques for grouping and aggregating data in pandas, from basic operations to advanced patterns and performance optimizations.
