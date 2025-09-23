# Pandas DataFrame

A DataFrame is a two-dimensional labeled data structure with columns of potentially different types. It's similar to a spreadsheet or SQL table. DataFrames are the most commonly used pandas object.

## Creating DataFrames

### From a Dictionary

```python
import pandas as pd

# Creating a DataFrame from dictionary
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'Age': [25, 30, 35, 28],
    'City': ['New York', 'San Francisco', 'Chicago', 'Boston']
}

df = pd.DataFrame(data)
print(df)
```

### From a List of Lists

```python
# From list of lists
data = [
    ['Alice', 25, 'New York'],
    ['Bob', 30, 'San Francisco'],
    ['Charlie', 35, 'Chicago'],
    ['Diana', 28, 'Boston']
]

df = pd.DataFrame(data, columns=['Name', 'Age', 'City'])
print(df)
```

### From a List of Dictionaries

```python
# From list of dictionaries
data = [
    {'Name': 'Alice', 'Age': 25, 'City': 'New York'},
    {'Name': 'Bob', 'Age': 30, 'City': 'San Francisco'},
    {'Name': 'Charlie', 'Age': 35, 'City': 'Chicago'},
    {'Name': 'Diana', 'Age': 28, 'City': 'Boston'}
]

df = pd.DataFrame(data)
print(df)
```

### With Custom Index

```python
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'Age': [25, 30, 35, 28],
    'City': ['New York', 'San Francisco', 'Chicago', 'Boston']
}

df = pd.DataFrame(data, index=['A', 'B', 'C', 'D'])
print(df)
```

### From NumPy Arrays

```python
import numpy as np

# From numpy array
arr = np.random.randn(4, 3)
df = pd.DataFrame(arr, columns=['A', 'B', 'C'])
print(df)
```

## DataFrame Attributes

```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'San Francisco', 'Chicago']
})

print(df.shape)      # (3, 3) - rows, columns
print(df.columns)    # Index(['Name', 'Age', 'City'])
print(df.index)      # RangeIndex(start=0, stop=3, step=1)
print(df.dtypes)     # Data types of each column
print(df.values)     # Underlying numpy array
```

## Basic DataFrame Operations

### Viewing Data

```python
# First 5 rows
print(df.head())

# Last 5 rows
print(df.tail())

# Random sample
print(df.sample(2))

# DataFrame info
print(df.info())

# Statistical summary
print(df.describe())
```

### Selecting Columns

```python
# Single column
names = df['Name']
ages = df.Age  # Alternative syntax

# Multiple columns
subset = df[['Name', 'Age']]
print(subset)
```

### Selecting Rows

```python
# By position
first_row = df.iloc[0]
first_three = df.iloc[0:3]

# By label
df.index = ['A', 'B', 'C']
row_a = df.loc['A']
rows_a_c = df.loc['A':'C']
```

### Boolean Indexing

```python
# Filter rows
adults = df[df['Age'] >= 30]
young_people = df[(df['Age'] >= 18) & (df['Age'] < 30)]
ny_people = df[df['City'] == 'New York']
```

## Modifying DataFrames

### Adding Columns

```python
df['Salary'] = [50000, 60000, 70000]
df['Age_Group'] = pd.cut(df['Age'],
                        bins=[0, 18, 30, 50, 100],
                        labels=['Child', 'Young', 'Adult', 'Senior'])
print(df)
```

### Modifying Columns

```python
df['Age'] = df['Age'] + 1
df['City'] = df['City'].str.upper()
```

### Renaming Columns

```python
df = df.rename(columns={'Name': 'Full_Name', 'Age': 'Years_Old'})
```

### Dropping Columns/Rows

```python
# Drop columns
df = df.drop(['Salary'], axis=1)

# Drop rows
df = df.drop([0, 2])  # Drop rows with index 0 and 2
```

## DataFrame Methods

### Sorting

```python
# Sort by single column
df_sorted = df.sort_values('Age')

# Sort by multiple columns
df_sorted = df.sort_values(['City', 'Age'], ascending=[True, False])

# Sort by index
df_sorted = df.sort_index()
```

### Grouping and Aggregation

```python
# Group by and aggregate
grouped = df.groupby('City')['Age'].mean()
print(grouped)

# Multiple aggregations
result = df.groupby('City')['Salary'].agg(['mean', 'sum', 'count'])
print(result)
```

### Transposing

```python
# Transpose DataFrame
df_transposed = df.T
print(df_transposed)
```

### Pivoting

```python
# Create pivot table
pivot = df.pivot_table(values='Salary',
                      index='City',
                      columns='Department',
                      aggfunc='mean')
print(pivot)
```

## Data Cleaning

### Handling Missing Values

```python
# Check for missing values
print(df.isnull().sum())

# Drop rows with missing values
df_clean = df.dropna()

# Fill missing values
df['Age'] = df['Age'].fillna(df['Age'].mean())
df['City'] = df['City'].fillna('Unknown')
```

### Data Type Conversion

```python
# Convert data types
df['Age'] = df['Age'].astype(int)
df['Salary'] = df['Salary'].astype(float)

# Convert to datetime
df['Date'] = pd.to_datetime(df['Date'])

# Convert to category
df['Department'] = df['Department'].astype('category')
```

## Merging and Joining

### Concatenation

```python
df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})

# Vertical concatenation
result = pd.concat([df1, df2])

# Horizontal concatenation
result = pd.concat([df1, df2], axis=1)
```

### Merging (SQL-style joins)

```python
left = pd.DataFrame({'key': ['A', 'B', 'C'], 'value': [1, 2, 3]})
right = pd.DataFrame({'key': ['A', 'B', 'D'], 'value2': [4, 5, 6]})

# Inner join
result = pd.merge(left, right, on='key', how='inner')

# Left join
result = pd.merge(left, right, on='key', how='left')

# Outer join
result = pd.merge(left, right, on='key', how='outer')
```

## Statistical Operations

```python
# Summary statistics
print(df.describe())

# Correlation matrix
correlation = df.corr()
print(correlation)

# Covariance matrix
covariance = df.cov()
print(covariance)

# Column-wise operations
print(df['Age'].mean())
print(df['Salary'].sum())
print(df['Age'].std())
```

## String Operations

```python
# String methods on columns
df['Name'] = df['Name'].str.upper()
df['Name_Length'] = df['Name'].str.len()
contains_a = df['Name'].str.contains('A')
print(contains_a)
```

## DateTime Operations

```python
# Convert to datetime
df['Date'] = pd.to_datetime(df['Date'])

# Extract date components
df['Year'] = df['Date'].dt.year
df['Month'] = df['Date'].dt.month
df['Day'] = df['Date'].dt.day

# Date filtering
recent_dates = df[df['Date'] > '2023-01-01']
```

## Exporting Data

```python
# To CSV
df.to_csv('output.csv', index=False)

# To Excel
df.to_excel('output.xlsx', sheet_name='Sheet1', index=False)

# To JSON
df.to_json('output.json', orient='records')

# To SQL
from sqlalchemy import create_engine
engine = create_engine('sqlite:///data.db')
df.to_sql('table_name', engine, index=False, if_exists='replace')
```

## DataFrame Styling

```python
# Basic styling
styled_df = df.style.highlight_max(axis=0)
styled_df = df.style.background_gradient(cmap='viridis')
```

## Performance Considerations

### Memory Usage

```python
# Check memory usage
print(df.memory_usage(deep=True))

# Optimize data types
df['category'] = df['category'].astype('category')
df['small_int'] = df['small_int'].astype('int8')
```

### Copy vs View

```python
# Create a copy to avoid SettingWithCopyWarning
df_subset = df[df['Age'] > 30].copy()
df_subset['Status'] = 'Senior'
```

## Best Practices

1. **Use descriptive column names** and avoid spaces/special characters
2. **Set appropriate data types** to optimize memory usage
3. **Handle missing values** explicitly
4. **Use vectorized operations** instead of loops
5. **Avoid chained indexing** when modifying data
6. **Document your DataFrames** with clear column descriptions
7. **Use meaningful index** when appropriate
8. **Backup important data** before major operations

## Common DataFrame Patterns

### Creating Empty DataFrame and Adding Data

```python
# Create empty DataFrame
df = pd.DataFrame(columns=['Name', 'Age', 'City'])

# Add rows
df.loc[0] = ['Alice', 25, 'New York']
df.loc[1] = ['Bob', 30, 'San Francisco']
```

### Conditional Column Creation

```python
df['Category'] = 'Adult'
df.loc[df['Age'] < 18, 'Category'] = 'Minor'
df.loc[df['Age'] > 65, 'Category'] = 'Senior'
```

### Applying Functions

```python
# Apply function to column
df['Name_Upper'] = df['Name'].apply(str.upper)

# Apply function to entire DataFrame
df_transformed = df.apply(lambda x: x.str.upper() if x.dtype == 'object' else x)
```

### Melting and Pivoting

```python
# Melt wide format to long format
melted = pd.melt(df, id_vars=['Name'],
                 value_vars=['Math', 'Science', 'English'],
                 var_name='Subject', value_name='Score')

# Pivot back to wide format
pivoted = melted.pivot(index='Name', columns='Subject', values='Score')
```
