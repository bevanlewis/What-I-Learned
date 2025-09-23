# Selecting Data in Pandas

Pandas provides powerful and flexible ways to select data from DataFrames and Series. This section covers the various methods for data selection, from basic column/row access to complex boolean indexing.

## Basic Column Selection

```python
import pandas as pd

# Sample DataFrame
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'Age': [25, 30, 35, 28],
    'City': ['New York', 'San Francisco', 'Chicago', 'Boston'],
    'Salary': [50000, 60000, 70000, 55000]
})

# Select single column (returns Series)
names = df['Name']
print(names)
print(type(names))  # <class 'pandas.core.series.Series'>

# Alternative syntax (only works if column name is valid Python identifier)
ages = df.Age
print(ages)
```

### Multiple Column Selection

```python
# Select multiple columns (returns DataFrame)
subset = df[['Name', 'Age']]
print(subset)
print(type(subset))  # <class 'pandas.core.frame.DataFrame'>

# Select columns by data type
numeric_cols = df.select_dtypes(include=['number'])
print(numeric_cols)

string_cols = df.select_dtypes(include=['object'])
print(string_cols)
```

## Row Selection by Position

### Using iloc (integer location)

```python
# Select single row
first_row = df.iloc[0]
print(first_row)

# Select multiple rows
first_three = df.iloc[0:3]
print(first_three)

# Select specific rows
selected_rows = df.iloc[[0, 2]]
print(selected_rows)

# Select row and column
value = df.iloc[0, 1]  # Row 0, Column 1
print(value)

# Select multiple rows and columns
subset = df.iloc[0:2, 0:2]  # First 2 rows, first 2 columns
print(subset)
```

## Row Selection by Label

### Using loc (label-based location)

```python
# Set a meaningful index
df.index = ['A', 'B', 'C', 'D']

# Select by label
row_a = df.loc['A']
print(row_a)

# Select multiple rows by label
rows_a_c = df.loc['A':'C']
print(rows_a_c)

# Select specific rows
selected = df.loc[['A', 'C']]
print(selected)

# Select row and column by label
value = df.loc['A', 'Age']
print(value)

# Select with boolean condition
adults = df.loc[df['Age'] >= 30]
print(adults)
```

## Boolean Indexing

### Single Condition

```python
# Filter rows where Age >= 30
adults = df[df['Age'] >= 30]
print(adults)

# Filter rows where City is 'New York'
ny_people = df[df['City'] == 'New York']
print(ny_people)

# Filter rows where Name starts with 'A'
starts_with_a = df[df['Name'].str.startswith('A')]
print(starts_with_a)
```

### Multiple Conditions

```python
# AND condition (&)
young_adults = df[(df['Age'] >= 25) & (df['Age'] <= 35)]
print(young_adults)

# OR condition (|)
high_salary_or_ny = df[(df['Salary'] > 60000) | (df['City'] == 'New York')]
print(high_salary_or_ny)

# NOT condition (~)
not_chicago = df[~(df['City'] == 'Chicago')]
print(not_chicago)

# Complex conditions
complex_filter = df[
    ((df['Age'] >= 25) & (df['Salary'] > 55000)) |
    (df['City'].isin(['New York', 'Boston']))
]
print(complex_filter)
```

## Conditional Selection Methods

### isin() - Check membership

```python
# Select rows where City is in a list
cities_of_interest = ['New York', 'Boston']
selected = df[df['City'].isin(cities_of_interest)]
print(selected)

# Select rows where Age is in a list
ages_of_interest = [25, 30]
selected = df[df['Age'].isin(ages_of_interest)]
print(selected)
```

### between() - Check range

```python
# Select rows where Age is between 25 and 35
age_range = df[df['Age'].between(25, 35)]
print(age_range)

# Select rows where Salary is between 50000 and 65000
salary_range = df[df['Salary'].between(50000, 65000)]
print(salary_range)
```

### str accessor - String operations

```python
# String contains
contains_alice = df[df['Name'].str.contains('Alice')]
print(contains_alice)

# String starts with
starts_with_c = df[df['Name'].str.startswith('C')]
print(starts_with_c)

# String length
long_names = df[df['Name'].str.len() > 4]
print(long_names)

# Case-insensitive search
contains_case_insensitive = df[df['Name'].str.lower().str.contains('alice')]
print(contains_case_insensitive)
```

## Advanced Selection Techniques

### query() Method

```python
# Using query() for readable conditions
result = df.query('Age >= 30 and Salary > 55000')
print(result)

# Using variables in query
min_age = 28
result = df.query('Age >= @min_age')
print(result)

# Complex queries
result = df.query('Age >= 25 and (City == "New York" or City == "Boston")')
print(result)
```

### where() Method

```python
# Keep all rows but replace values that don't meet condition with NaN
result = df.where(df['Age'] >= 30)
print(result)

# Keep values only for certain condition, others become NaN
result = df.where(df['Salary'] > 55000, other='Low Salary')
print(result)
```

### mask() Method

```python
# Opposite of where() - replace values that meet condition
result = df.mask(df['Age'] < 30, other='Too Young')
print(result)
```

## Index-based Selection

### Setting and Using Index

```python
# Set index to Name column
df_indexed = df.set_index('Name')

# Select by name
alice_data = df_indexed.loc['Alice']
print(alice_data)

# Select multiple names
selected = df_indexed.loc[['Alice', 'Bob']]
print(selected)

# Reset index
df_reset = df_indexed.reset_index()
print(df_reset)
```

### MultiIndex Selection

```python
# Create MultiIndex DataFrame
multi_df = df.set_index(['City', 'Name'])

# Select by first level
ny_data = multi_df.loc['New York']
print(ny_data)

# Select by both levels
alice_ny = multi_df.loc[('New York', 'Alice')]
print(alice_ny)

# Slice MultiIndex
ny_people = multi_df.loc['New York':'Chicago']
print(ny_people)
```

## Time-based Selection

```python
# Create DataFrame with datetime index
dates = pd.date_range('2023-01-01', periods=10, freq='D')
time_df = pd.DataFrame({'value': range(10)}, index=dates)

# Select date range
jan_2023 = time_df['2023-01-01':'2023-01-05']
print(jan_2023)

# Select by year
year_2023 = time_df['2023']
print(year_2023)

# Select by partial date
jan_data = time_df[time_df.index.month == 1]
print(jan_data)
```

## Random Sampling

```python
# Random sample of rows
sample = df.sample(n=2)  # Fixed number
print(sample)

sample_frac = df.sample(frac=0.5)  # Percentage
print(sample_frac)

# Weighted sampling
weights = [0.1, 0.2, 0.3, 0.4]  # Higher weight for last row
weighted_sample = df.sample(n=2, weights=weights)
print(weighted_sample)
```

## Selection Performance Tips

### Avoid Chained Indexing

```python
# Avoid (chained indexing - can cause issues)
# df[df['Age'] > 30]['Name']

# Preferred approach
adults = df[df['Age'] > 30]
adult_names = adults['Name']
print(adult_names)

# Or use loc
adult_names = df.loc[df['Age'] > 30, 'Name']
print(adult_names)
```

### Use Efficient Selection Methods

```python
# Efficient: Use vectorized operations
result = df[df['Age'] >= 30]

# Less efficient: Iterate through rows
# result = df[[i for i in df.index if df.loc[i, 'Age'] >= 30]]
```

### Copy vs View

```python
# Create a copy to avoid SettingWithCopyWarning
subset = df[df['Age'] > 25].copy()
subset['Status'] = 'Adult'
```

## Common Selection Patterns

### Top N Records

```python
# Top 3 highest salaries
top_salaries = df.nlargest(3, 'Salary')
print(top_salaries)

# Bottom 2 ages
youngest = df.nsmallest(2, 'Age')
print(youngest)
```

### Conditional Column Selection

```python
# Select columns based on condition
numeric_columns = df.select_dtypes(include=['number']).columns
print(df[numeric_columns])

# Select columns by name pattern
name_cols = [col for col in df.columns if 'name' in col.lower()]
print(df[name_cols])
```

### Row and Column Selection Together

```python
# Select specific rows and columns
result = df.loc[df['Age'] >= 30, ['Name', 'Salary']]
print(result)

# Using iloc with conditions
adult_rows = df['Age'] >= 30
result = df.iloc[adult_rows.values, [0, 3]]  # Columns 0 and 3
print(result)
```

## Selection with Functions

### Using apply() with selection

```python
# Select rows based on custom function
def is_high_earner(row):
    return row['Salary'] > 60000 and row['Age'] < 35

high_earners = df[df.apply(is_high_earner, axis=1)]
print(high_earners)
```

### Filter with lambda functions

```python
# Filter with lambda
long_names = df[df['Name'].apply(lambda x: len(x) > 4)]
print(long_names)

# Multiple conditions with lambda
complex_filter = df[df.apply(lambda row: row['Age'] * row['Salary'] > 2000000, axis=1)]
print(complex_filter)
```

## Best Practices

1. **Use .loc and .iloc** for reliable data access
2. **Avoid chained indexing** when possible
3. **Use boolean indexing** for complex conditions
4. **Prefer vectorized operations** over loops
5. **Use .copy()** when creating subsets for modification
6. **Consider performance** for large DataFrames
7. **Use meaningful variable names** for filtered data
8. **Document complex selection logic**

## Common Pitfalls

### Chained Indexing Issues

```python
# Problematic: Can cause SettingWithCopyWarning
df[df['Age'] > 30]['Salary'] = 80000

# Solution: Use loc
df.loc[df['Age'] > 30, 'Salary'] = 80000
```

### Boolean Indexing with Multiple Conditions

```python
# Wrong: Using 'and' instead of '&'
# df[(df['Age'] > 25) and (df['Salary'] > 50000)]  # Error

# Correct: Use '&' for element-wise AND
df[(df['Age'] > 25) & (df['Salary'] > 50000)]
```

### Index Alignment Issues

```python
# When working with multiple DataFrames, ensure index alignment
df1 = df[df['Age'] > 25]
df2 = df[df['Salary'] > 55000]

# May not align as expected
combined = df1 + df2  # Can cause NaN values

# Better: Reset index or use merge
df1_reset = df1.reset_index(drop=True)
df2_reset = df2.reset_index(drop=True)
```

### Memory Issues with Large Selections

```python
# For large DataFrames, be mindful of memory usage
# Instead of creating large intermediate DataFrames:

# Good: Chain operations
result = (df[df['Age'] >= 30]
          .groupby('City')['Salary']
          .mean()
          .reset_index())

# Avoid: Creating large intermediate DataFrames
# filtered = df[df['Age'] >= 30]  # Large DataFrame
# grouped = filtered.groupby('City')  # Another large object
# result = grouped['Salary'].mean()
```
