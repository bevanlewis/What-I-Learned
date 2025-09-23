# Pandas Best Practices

This guide covers essential best practices for working with Pandas efficiently and effectively. Following these guidelines will help you write cleaner, faster, and more maintainable code.

## Data Import and Setup

### 1. Import Conventions

```python
# Standard imports
import pandas as pd
import numpy as np

# Avoid wildcard imports
# from pandas import *  # DON'T DO THIS

# Import specific functions when needed
from pandas import read_csv, to_datetime
```

### 2. Set Display Options Early

```python
# Set pandas display options at the beginning of your script
pd.set_option('display.max_rows', 100)
pd.set_option('display.max_columns', 50)
pd.set_option('display.width', 1000)
pd.set_option('display.float_format', '{:.2f}'.format)

# Reset to defaults if needed
pd.reset_option('all')
```

### 3. Use Appropriate Data Types

```python
# Convert to efficient data types
df = pd.read_csv('data.csv')

# Convert object columns to category when appropriate
df['category_col'] = df['category_col'].astype('category')

# Use smaller numeric types when possible
df['small_int'] = df['small_int'].astype('int32')  # Instead of int64
df['small_float'] = df['small_float'].astype('float32')  # Instead of float64

# Convert to datetime early
df['date_col'] = pd.to_datetime(df['date_col'])
```

## Data Selection and Filtering

### 4. Use .loc and .iloc for Reliable Access

```python
# Good: Explicit indexing
df.loc[df['Age'] > 30, 'Salary'] = 80000
selected_rows = df.loc[0:5, ['Name', 'Age']]

# Avoid: Chained indexing (can cause issues)
# df[df['Age'] > 30]['Salary'] = 80000  # SettingWithCopyWarning
```

### 5. Prefer Vectorized Operations

```python
# Good: Vectorized operations
df['total'] = df['price'] * df['quantity']
df['is_adult'] = df['age'] >= 18

# Avoid: Iterating with loops (slow and verbose)
# totals = []
# for index, row in df.iterrows():
#     totals.append(row['price'] * row['quantity'])
# df['total'] = totals
```

### 6. Use query() for Complex Conditions

```python
# Good: Readable query syntax
adults = df.query('age >= 18 and salary > 50000')

# Variables in queries
min_salary = 50000
result = df.query('salary > @min_salary and department == "Engineering"')

# Avoid: Complex boolean indexing (hard to read)
# adults = df[(df['age'] >= 18) & (df['salary'] > 50000) & (df['department'] == 'Engineering')]
```

## Data Modification

### 7. Handle Missing Data Appropriately

```python
# Check for missing data first
print(df.isnull().sum())

# Fill missing values thoughtfully
df['age'] = df['age'].fillna(df['age'].median())  # Numeric: use median/mean
df['category'] = df['category'].fillna('Unknown')  # Categorical: use placeholder

# Consider dropping if appropriate
df = df.dropna(subset=['critical_column'])  # Drop rows where critical data is missing
```

### 8. Use copy() When Creating Subsets for Modification

```python
# Good: Explicit copy
subset = df[df['age'] > 25].copy()
subset['status'] = 'Adult'

# Avoid: Potential SettingWithCopyWarning
# subset = df[df['age'] > 25]  # This might be a view
# subset['status'] = 'Adult'   # Could cause warnings
```

### 9. Chain Operations Efficiently

```python
# Good: Method chaining for readability
result = (df
          .query('age >= 18')
          .groupby('department')['salary']
          .mean()
          .reset_index()
          .sort_values('salary', ascending=False))

# Avoid: Creating intermediate variables unnecessarily
# filtered = df[df['age'] >= 18]
# grouped = filtered.groupby('department')
# salaries = grouped['salary'].mean()
# result = salaries.reset_index()
# result = result.sort_values('salary', ascending=False)
```

## Performance Optimization

### 10. Read Large Files Efficiently

```python
# Use chunks for large files
chunk_size = 10000
chunks = []

for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    # Process each chunk
    chunk['new_column'] = chunk['existing_column'] * 2
    chunks.append(chunk)

df = pd.concat(chunks, ignore_index=True)

# Or process chunks individually to save memory
for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    process_chunk(chunk)  # Process and save/write results
```

### 11. Use Efficient File Formats

```python
# For long-term storage and fast I/O
df.to_parquet('data.parquet')  # Best for analytical workloads
df.to_feather('data.feather')  # Fastest for pandas-pandas transfer

# For interchange
df.to_csv('data.csv', index=False)  # Human-readable
df.to_excel('data.xlsx')            # Excel compatibility
```

### 12. Optimize Memory Usage

```python
# Check memory usage
print(df.memory_usage(deep=True))

# Downcast numeric types
df['int_col'] = pd.to_numeric(df['int_col'], downcast='integer')
df['float_col'] = pd.to_numeric(df['float_col'], downcast='float')

# Use category dtype for repeated strings
df['category_col'] = df['category_col'].astype('category')

# Delete unused DataFrames
del large_df
import gc
gc.collect()  # Force garbage collection
```

## Code Organization

### 13. Write Reusable Functions

```python
def clean_dataframe(df):
    """Clean and prepare DataFrame for analysis."""
    # Remove duplicates
    df = df.drop_duplicates()

    # Handle missing values
    df = df.dropna(subset=['required_column'])
    df['optional_column'] = df['optional_column'].fillna('default')

    # Convert data types
    df['date_col'] = pd.to_datetime(df['date_col'])
    df['numeric_col'] = pd.to_numeric(df['numeric_col'])

    return df

# Usage
df_clean = clean_dataframe(df)
```

### 14. Use Meaningful Variable Names

```python
# Good
customer_orders = df[df['customer_type'] == 'premium']
monthly_sales = sales_df.groupby('month')['revenue'].sum()

# Avoid
df1 = df[df['col1'] == 'val']
s = df.groupby('col2')['col3'].sum()
```

### 15. Document Your Data Operations

```python
def calculate_customer_metrics(df):
    """
    Calculate key customer metrics from transaction data.

    Args:
        df: DataFrame with columns ['customer_id', 'transaction_date', 'amount']

    Returns:
        DataFrame with customer metrics: total_spend, avg_order_value, order_count
    """
    metrics = (df
               .groupby('customer_id')
               .agg({
                   'amount': ['sum', 'mean', 'count'],
                   'transaction_date': 'max'
               })
               .reset_index())

    # Flatten column names
    metrics.columns = ['customer_id', 'total_spend', 'avg_order_value', 'order_count', 'last_order_date']

    return metrics
```

## Error Handling and Validation

### 16. Validate Data Assumptions

```python
def validate_dataframe(df, required_columns=None):
    """Validate DataFrame structure and data quality."""
    errors = []

    # Check required columns
    if required_columns:
        missing_cols = set(required_columns) - set(df.columns)
        if missing_cols:
            errors.append(f"Missing required columns: {missing_cols}")

    # Check for empty DataFrame
    if df.empty:
        errors.append("DataFrame is empty")

    # Check data types
    if 'date_col' in df.columns:
        try:
            pd.to_datetime(df['date_col'])
        except ValueError:
            errors.append("Invalid date format in date_col")

    if errors:
        raise ValueError("Data validation failed: " + "; ".join(errors))

    return True

# Usage
validate_dataframe(df, required_columns=['name', 'age', 'salary'])
```

### 17. Handle Exceptions Gracefully

```python
def safe_read_csv(filename, **kwargs):
    """Safely read CSV with comprehensive error handling."""
    try:
        df = pd.read_csv(filename, **kwargs)
        print(f"Successfully read {len(df)} rows from {filename}")
        return df
    except FileNotFoundError:
        print(f"Error: File {filename} not found")
        return None
    except pd.errors.EmptyDataError:
        print(f"Error: {filename} is empty")
        return None
    except pd.errors.ParserError:
        print(f"Error: Could not parse {filename}")
        return None
    except Exception as e:
        print(f"Unexpected error reading {filename}: {e}")
        return None
```

## Testing and Debugging

### 18. Test Operations on Sample Data

```python
# Test operations on small sample first
sample_df = df.head(100).copy()

# Test your operations
result = (sample_df
          .query('age > 18')
          .groupby('category')['value']
          .transform(lambda x: x - x.mean()))

# Only apply to full dataset once tested
if result is not None:  # Check if operation succeeded
    full_result = (df
                   .query('age > 18')
                   .groupby('category')['value']
                   .transform(lambda x: x - x.mean()))
```

### 19. Use Assertions for Data Quality

```python
# Add data quality checks
assert df['age'].min() >= 0, "Age cannot be negative"
assert df['salary'].notnull().all(), "Salary cannot be null"
assert df['customer_id'].is_unique, "Customer IDs must be unique"

# Custom validation functions
def validate_age(age):
    return 0 <= age <= 120

age_valid = df['age'].apply(validate_age)
invalid_ages = df[~age_valid]
if not invalid_ages.empty:
    print(f"Found {len(invalid_ages)} invalid ages")
    print(invalid_ages)
```

## Collaboration and Maintenance

### 20. Version Control Friendly Practices

```python
# Avoid storing large DataFrames in version control
# df.to_pickle('processed_data.pkl')  # Don't commit large binary files

# Store raw data and processing scripts instead
df.to_csv('processed_data.csv', index=False)  # CSV is text-based and diff-able

# Use relative paths
data_path = 'data/raw_data.csv'
df = pd.read_csv(data_path)
```

### 21. Document Data Sources and Transformations

```python
# Document data lineage
data_sources = {
    'customers': 'customer_database_export_2023.csv',
    'orders': 'order_system_export_2023.csv',
    'products': 'product_catalog.json'
}

transformations_applied = [
    "Removed duplicate customer records",
    "Filled missing ages with median value",
    "Converted currency to USD",
    "Filtered out cancelled orders"
]

# Save metadata
metadata = {
    'sources': data_sources,
    'transformations': transformations_applied,
    'processing_date': pd.Timestamp.now(),
    'record_count': len(df)
}

import json
with open('data_metadata.json', 'w') as f:
    json.dump(metadata, f, indent=2, default=str)
```

## Summary

Following these best practices will help you:

- **Write more efficient code** that runs faster and uses less memory
- **Create more maintainable code** that's easier to understand and modify
- **Produce more reliable results** with better error handling and validation
- **Collaborate more effectively** with better documentation and organization
- **Avoid common pitfalls** and debugging headaches

Remember: pandas is designed for vectorized operations. When you find yourself writing loops, there's usually a better pandas way to accomplish the same task!
