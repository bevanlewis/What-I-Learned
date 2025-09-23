# Modifying Data in Pandas

Pandas provides comprehensive tools for modifying DataFrames and Series. This section covers adding, updating, and transforming data.

## Adding Columns

```python
import pandas as pd
import numpy as np

# Sample DataFrame
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'Salary': [50000, 60000, 70000]
})

# Add a single column
df['Department'] = ['Engineering', 'Marketing', 'Sales']
print(df)

# Add column with calculated values
df['Bonus'] = df['Salary'] * 0.1
print(df)

# Add column based on conditions
df['Senior'] = df['Age'] >= 30
print(df)

# Add column using assign() method
df = df.assign(Total_Compensation=df['Salary'] + df['Bonus'])
print(df)
```

## Modifying Existing Columns

```python
# Modify values in place
df['Age'] = df['Age'] + 1
print(df)

# Modify using conditions
df.loc[df['Salary'] > 60000, 'Department'] = 'Executive'
print(df)

# Modify multiple columns at once
df[['Salary', 'Bonus']] = df[['Salary', 'Bonus']] * 1.05  # 5% raise
print(df)

# Use apply() to modify columns
df['Name_Upper'] = df['Name'].apply(str.upper)
print(df)
```

## Renaming Columns

```python
# Rename single column
df = df.rename(columns={'Name': 'Employee_Name'})
print(df)

# Rename multiple columns
df = df.rename(columns={
    'Age': 'Employee_Age',
    'Salary': 'Annual_Salary'
})
print(df)

# Rename using a function
df = df.rename(columns=str.lower)  # Convert all to lowercase
print(df)

# Rename columns in place
df.rename(columns={'name': 'Name'}, inplace=True)
```

## Adding Rows

```python
# Add single row using loc
df.loc[len(df)] = ['Diana', 28, 55000, 'HR', 5500, False, 60500, 'DIANA']
print(df)

# Add multiple rows using concat
new_rows = pd.DataFrame({
    'Name': ['Eve', 'Frank'],
    'Age': [32, 29],
    'Salary': [65000, 58000],
    'Department': ['Finance', 'IT'],
    'Bonus': [6500, 5800],
    'Senior': [True, False],
    'Total_Compensation': [71500, 63800],
    'Name_Upper': ['EVE', 'FRANK']
})

df = pd.concat([df, new_rows], ignore_index=True)
print(df)
```

## Removing Columns and Rows

```python
# Remove single column
df = df.drop('Name_Upper', axis=1)
print(df)

# Remove multiple columns
df = df.drop(['Bonus', 'Total_Compensation'], axis=1)
print(df)

# Remove rows by index
df = df.drop([0, 2])  # Remove rows at index 0 and 2
print(df)

# Remove rows based on condition
df = df[df['Age'] >= 30]  # Keep only employees 30+
print(df)

# Remove duplicates
df = df.drop_duplicates(subset=['Name'])  # Remove duplicates based on Name
print(df)
```

## Data Type Conversion

```python
# Convert column data types
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': ['25', '30', '35'],  # String ages
    'Salary': ['50000.0', '60000.0', '70000.0']  # String salaries
})

# Convert to numeric types
df['Age'] = pd.to_numeric(df['Age'])
df['Salary'] = pd.to_numeric(df['Salary'])
print(df.dtypes)

# Convert to datetime
df_dates = pd.DataFrame({
    'Date_Str': ['2023-01-01', '2023-02-01', '2023-03-01']
})
df_dates['Date'] = pd.to_datetime(df_dates['Date_Str'])
print(df_dates.dtypes)

# Convert to category (memory efficient for repeated values)
df['Department'] = df['Department'].astype('category')
```

## Handling Missing Values

```python
# Create DataFrame with missing values
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, 7, 8],
    'C': [9, 10, 11, np.nan]
})

# Fill missing values with a specific value
df_filled = df.fillna(0)
print(df_filled)

# Fill with mean of column
df['A'] = df['A'].fillna(df['A'].mean())
print(df)

# Forward fill (use previous value)
df_ffill = df.fillna(method='ffill')
print(df_ffill)

# Backward fill (use next value)
df_bfill = df.fillna(method='bfill')
print(df_bfill)

# Interpolate missing values
df_interp = df.interpolate()
print(df_interp)

# Drop rows with missing values
df_dropped = df.dropna()
print(df_dropped)

# Drop columns with missing values
df_drop_cols = df.dropna(axis=1)
print(df_drop_cols)
```

## String Operations

```python
df = pd.DataFrame({
    'Name': ['alice smith', 'BOB JOHNSON', 'Charlie Brown'],
    'Email': ['alice@example.com', 'bob@example.com', 'charlie@example.com']
})

# Convert to uppercase
df['Name_Upper'] = df['Name'].str.upper()
print(df)

# Convert to title case
df['Name_Title'] = df['Name'].str.title()
print(df)

# Extract domain from email
df['Domain'] = df['Email'].str.split('@').str[1]
print(df)

# Replace text
df['Name'] = df['Name'].str.replace(' ', '_')
print(df)

# Split names into first and last
df[['First_Name', 'Last_Name']] = df['Name'].str.split(' ', expand=True)
print(df)
```

## Mathematical Operations

```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4],
    'B': [10, 20, 30, 40],
    'C': [100, 200, 300, 400]
})

# Add scalar to all values in column
df['A_Plus_10'] = df['A'] + 10
print(df)

# Element-wise operations between columns
df['A_Times_B'] = df['A'] * df['B']
print(df)

# Apply mathematical functions
df['A_Squared'] = df['A'] ** 2
df['B_Sqrt'] = np.sqrt(df['B'])
print(df)

# Cumulative operations
df['A_Cumsum'] = df['A'].cumsum()
df['B_Cumprod'] = df['B'].cumprod()
print(df)
```

## Conditional Modifications

```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'Age': [25, 30, 35, 28],
    'Salary': [50000, 60000, 70000, 55000]
})

# Conditional assignment
df['Category'] = 'Standard'
df.loc[df['Salary'] > 60000, 'Category'] = 'Executive'
df.loc[df['Age'] < 30, 'Category'] = 'Junior'
print(df)

# Using numpy.where
df['Status'] = np.where(df['Age'] >= 30, 'Senior', 'Junior')
print(df)

# Multiple conditions
conditions = [
    (df['Age'] < 25),
    (df['Age'] >= 25) & (df['Age'] < 35),
    (df['Age'] >= 35)
]
choices = ['Entry Level', 'Mid Level', 'Senior Level']
df['Level'] = np.select(conditions, choices, default='Unknown')
print(df)
```

## Transforming Data

### Using apply()

```python
# Apply function to each element
df['Salary_Log'] = df['Salary'].apply(np.log)
print(df)

# Apply function to each row
def categorize_salary(salary):
    if salary < 55000:
        return 'Low'
    elif salary < 65000:
        return 'Medium'
    else:
        return 'High'

df['Salary_Category'] = df['Salary'].apply(categorize_salary)
print(df)

# Apply function with multiple columns
def calculate_bonus(row):
    base_bonus = row['Salary'] * 0.1
    if row['Age'] > 30:
        return base_bonus * 1.2  # 20% more for older employees
    return base_bonus

df['Custom_Bonus'] = df.apply(calculate_bonus, axis=1)
print(df)
```

### Using map()

```python
# Map values using a dictionary
grade_map = {'A': 4, 'B': 3, 'C': 2, 'D': 1, 'F': 0}
df_grades = pd.DataFrame({
    'Student': ['Alice', 'Bob', 'Charlie'],
    'Grade': ['A', 'B', 'A']
})
df_grades['Points'] = df_grades['Grade'].map(grade_map)
print(df_grades)

# Map using a function
df['Name_Length'] = df['Name'].map(len)
print(df)
```

## Pivot and Melt Operations

```python
# Create sample data for pivoting
sales_data = pd.DataFrame({
    'Date': ['2023-01', '2023-01', '2023-02', '2023-02'],
    'Product': ['A', 'B', 'A', 'B'],
    'Sales': [100, 150, 120, 180]
})

# Pivot: wide format
pivot_table = sales_data.pivot(index='Date', columns='Product', values='Sales')
print(pivot_table)

# Melt: long format to wide format (reverse pivot)
melted_data = pivot_table.reset_index().melt(id_vars='Date', var_name='Product', value_name='Sales')
print(melted_data)
```

## Performance Considerations

### Vectorized Operations

```python
# Good: Vectorized operations are fast
df['Total'] = df['Salary'] + df['Bonus']

# Avoid: Iterating with loops (slow)
# totals = []
# for index, row in df.iterrows():
#     totals.append(row['Salary'] + row['Bonus'])
# df['Total'] = totals
```

### Memory-Efficient Modifications

```python
# Use copy() when creating subsets for modification
subset = df[df['Age'] > 25].copy()
subset['Status'] = 'Adult'

# Avoid SettingWithCopyWarning
df.loc[df['Age'] > 25, 'Status'] = 'Adult'
```

### In-Place Operations

```python
# In-place operations save memory
df['Age'].fillna(0, inplace=True)

# Equivalent but creates copy
# df['Age'] = df['Age'].fillna(0)
```

## Best Practices

1. **Use vectorized operations** instead of loops when possible
2. **Be careful with chained indexing** - use .loc for safety
3. **Use copy()** when creating subsets for modification
4. **Handle missing values** appropriately before operations
5. **Validate data types** before performing operations
6. **Consider memory usage** for large DataFrames
7. **Use meaningful column names** and document changes
8. **Test modifications** on small samples first

## Common Modification Patterns

### Data Normalization

```python
# Min-Max normalization
df['Salary_Normalized'] = (df['Salary'] - df['Salary'].min()) / (df['Salary'].max() - df['Salary'].min())

# Z-score normalization
df['Age_Zscore'] = (df['Age'] - df['Age'].mean()) / df['Age'].std()
print(df)
```

### Binning/Numerical Categorization

```python
# Age groups
df['Age_Group'] = pd.cut(df['Age'], bins=[0, 25, 35, 50, 100], labels=['Young', 'Adult', 'Middle-aged', 'Senior'])

# Salary brackets
df['Salary_Bracket'] = pd.cut(df['Salary'], bins=3, labels=['Low', 'Medium', 'High'])
print(df)
```

### Date Feature Engineering

```python
df_dates = pd.DataFrame({
    'Date': pd.date_range('2023-01-01', periods=5, freq='D'),
    'Value': [10, 20, 15, 25, 30]
})

df_dates['Year'] = df_dates['Date'].dt.year
df_dates['Month'] = df_dates['Date'].dt.month
df_dates['Day'] = df_dates['Date'].dt.day
df_dates['Weekday'] = df_dates['Date'].dt.day_name()
df_dates['Is_Weekend'] = df_dates['Date'].dt.weekday >= 5
print(df_dates)
```

### Text Data Cleaning

```python
df_text = pd.DataFrame({
    'Text': ['  Hello World  ', 'PYTHON programming', 'Data Science 101']
})

# Clean text
df_text['Clean_Text'] = (df_text['Text']
                        .str.strip()           # Remove whitespace
                        .str.lower()           # Convert to lowercase
                        .str.replace(r'\d+', '')  # Remove numbers
                        .str.replace(r'\s+', ' '))  # Normalize spaces

print(df_text)
```
