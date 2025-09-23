# Merging and Joining DataFrames

Pandas provides powerful tools for combining DataFrames through merging and joining operations, similar to SQL joins.

## Basic Merging

### Inner Join (Default)

```python
import pandas as pd

# Sample DataFrames
employees = pd.DataFrame({
    'emp_id': [1, 2, 3, 4],
    'name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'dept_id': [10, 20, 10, 30]
})

departments = pd.DataFrame({
    'dept_id': [10, 20, 30, 40],
    'dept_name': ['Engineering', 'Sales', 'HR', 'Finance']
})

# Inner join on dept_id
result = pd.merge(employees, departments, on='dept_id')
print(result)
```

### Left Join

```python
# Left join - keep all employees, even if no department match
result = pd.merge(employees, departments, on='dept_id', how='left')
print(result)
```

### Right Join

```python
# Right join - keep all departments, even if no employee match
result = pd.merge(employees, departments, on='dept_id', how='right')
print(result)
```

### Outer Join

```python
# Outer join - keep all records from both DataFrames
result = pd.merge(employees, departments, on='dept_id', how='outer')
print(result)
```

## Merging on Different Column Names

```python
# When column names differ
employees = pd.DataFrame({
    'employee_id': [1, 2, 3],
    'name': ['Alice', 'Bob', 'Charlie'],
    'department_code': [10, 20, 10]
})

depts = pd.DataFrame({
    'code': [10, 20, 30],
    'department': ['Engineering', 'Sales', 'HR']
})

# Specify left_on and right_on
result = pd.merge(employees, depts,
                 left_on='department_code',
                 right_on='code')
print(result)
```

## Merging on Index

```python
# Merge on index
employees_idx = employees.set_index('employee_id')
salaries = pd.DataFrame({
    'salary': [75000, 65000, 80000]
}, index=[1, 2, 3])

result = pd.merge(employees_idx, salaries, left_index=True, right_index=True)
print(result)
```

## Concatenation

### Vertical Concatenation

```python
# Stack DataFrames vertically
df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})

result = pd.concat([df1, df2])
print(result)
```

### Horizontal Concatenation

```python
# Stack DataFrames horizontally
result = pd.concat([df1, df2], axis=1)
print(result)
```

### Concatenation with Keys

```python
# Add hierarchical index
result = pd.concat([df1, df2], keys=['first', 'second'])
print(result)
```

## Join Method

```python
# Using .join() method
left = pd.DataFrame({'A': [1, 2], 'B': [3, 4]}, index=[0, 1])
right = pd.DataFrame({'C': [5, 6], 'D': [7, 8]}, index=[0, 1])

result = left.join(right)
print(result)
```

## Handling Column Name Conflicts

```python
# When both DataFrames have same column names
df1 = pd.DataFrame({'key': [1, 2], 'value': [10, 20]})
df2 = pd.DataFrame({'key': [1, 2], 'value': [30, 40]})

# Use suffixes to distinguish columns
result = pd.merge(df1, df2, on='key', suffixes=('_left', '_right'))
print(result)
```

## Advanced Merging Techniques

### Merging Multiple DataFrames

```python
# Merge multiple DataFrames
result = pd.merge(pd.merge(employees, departments, on='dept_id'),
                 salaries, left_on='emp_id', right_index=True)
print(result)
```

### Conditional Merging

```python
# Merge based on conditions
result = pd.merge(employees, departments,
                 left_on='dept_id',
                 right_on='dept_id',
                 how='inner')
print(result)
```

## Performance Considerations

### Choosing the Right Join Type

- **Inner Join**: Use when you only want matching records
- **Left Join**: Use when you want all records from left DataFrame
- **Right Join**: Use when you want all records from right DataFrame
- **Outer Join**: Use when you want all records from both DataFrames

### Index for Performance

```python
# Set merge keys as index for better performance
employees = employees.set_index('dept_id')
departments = departments.set_index('dept_id')

result = employees.join(departments, how='inner')
print(result)
```

## Common Patterns

### Lookup Tables

```python
# Department lookup
dept_lookup = pd.DataFrame({
    'dept_id': [10, 20, 30],
    'dept_name': ['Engineering', 'Sales', 'HR'],
    'location': ['NYC', 'LA', 'Chicago']
})

# Merge with lookup
result = pd.merge(employees, dept_lookup, on='dept_id')
print(result)
```

### Time Series Merging

```python
# Merge time series data
sales = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=5),
    'sales': [100, 150, 200, 175, 225]
})

weather = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=5),
    'temperature': [20, 22, 25, 23, 21]
})

# Merge on date
sales_weather = pd.merge(sales, weather, on='date')
print(sales_weather)
```

## Best Practices

1. **Choose appropriate join types** based on your data requirements
2. **Handle column name conflicts** with suffixes
3. **Set appropriate indexes** for better performance
4. **Validate merge results** by checking row counts
5. **Use merge indicators** to understand join behavior

```python
# Use indicator to see join behavior
result = pd.merge(employees, departments, on='dept_id', how='outer', indicator=True)
print(result['_merge'].value_counts())
```

6. **Consider data types** when merging
7. **Handle missing values** appropriately after merging
