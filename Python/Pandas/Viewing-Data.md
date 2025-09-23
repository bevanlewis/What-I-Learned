# Viewing Data in Pandas

Pandas provides numerous methods to inspect and understand your data. This section covers the essential tools for viewing DataFrame and Series contents.

## Basic DataFrame Inspection

```python
import pandas as pd
import numpy as np

# Sample DataFrame
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve'],
    'Age': [25, 30, 35, 28, 32],
    'City': ['New York', 'San Francisco', 'Chicago', 'Boston', 'Seattle'],
    'Salary': [50000, 60000, 70000, 55000, 65000]
})

# Display first 5 rows
print(df.head())
```

### Viewing First/Last Rows

```python
# First 5 rows (default)
print(df.head())

# First 3 rows
print(df.head(3))

# Last 5 rows
print(df.tail())

# Last 2 rows
print(df.tail(2))
```

### Random Sampling

```python
# Random sample of rows
print(df.sample())      # One random row
print(df.sample(3))     # Three random rows
print(df.sample(frac=0.5))  # 50% of rows randomly
```

## DataFrame Information

```python
# Basic information
print(df.info())
```

### Statistical Summary

```python
# Statistical summary for numeric columns
print(df.describe())
```

### DataFrame Shape and Size

```python
# Shape (rows, columns)
print(df.shape)        # (5, 4)
print(f"Rows: {df.shape[0]}, Columns: {df.shape[1]}")

# Total number of elements
print(df.size)         # 20

# Memory usage
print(df.memory_usage())
print(df.memory_usage(deep=True))  # Include object memory
```

## Column and Index Information

```python
# Column names
print(df.columns)
print(list(df.columns))

# Index information
print(df.index)
print(df.index.tolist())

# Check if index has a name
print(df.index.name)
```

## Data Types

```python
# Data types of each column
print(df.dtypes)

# Detailed dtypes info
print(df.info())
```

## Unique Values and Counts

```python
# Unique values in a column
print(df['City'].unique())

# Count of unique values
print(df['City'].nunique())

# Value counts (frequency)
print(df['City'].value_counts())

# Normalized value counts
print(df['City'].value_counts(normalize=True))
```

## Missing Data Inspection

```python
# Check for missing values
print(df.isnull())
print(df.isna())  # Same as isnull()

# Count missing values per column
print(df.isnull().sum())

# Total missing values
print(df.isnull().sum().sum())

# Check if any missing values exist
print(df.isnull().any().any())
```

## Data Distribution

```python
# Basic statistics
print(df['Age'].mean())
print(df['Age'].median())
print(df['Age'].std())
print(df['Age'].min())
print(df['Age'].max())

# Quantiles
print(df['Age'].quantile([0.25, 0.5, 0.75]))

# Mode
print(df['Age'].mode())
```

## Correlation Analysis

```python
# Correlation matrix (numeric columns only)
numeric_df = df.select_dtypes(include=[np.number])
print(numeric_df.corr())

# Specific column correlations
print(df[['Age', 'Salary']].corr())
```

## Data Quality Checks

```python
# Check for duplicates
print(df.duplicated())
print(f"Number of duplicate rows: {df.duplicated().sum()}")

# Check data types consistency
print(df.dtypes)

# Range checks
print(f"Age range: {df['Age'].min()} - {df['Age'].max()}")
print(f"Salary range: {df['Salary'].min()} - {df['Salary'].max()}")

# Outlier detection (simple)
q1 = df['Salary'].quantile(0.25)
q3 = df['Salary'].quantile(0.75)
iqr = q3 - q1
lower_bound = q1 - 1.5 * iqr
upper_bound = q3 + 1.5 * iqr

outliers = df[(df['Salary'] < lower_bound) | (df['Salary'] > upper_bound)]
print(f"Potential outliers: {len(outliers)}")
```

## Advanced Viewing Options

### Setting Display Options

```python
# Set pandas display options
pd.set_option('display.max_rows', 100)        # Max rows to display
pd.set_option('display.max_columns', 50)      # Max columns to display
pd.set_option('display.width', 1000)          # Display width
pd.set_option('display.float_format', '{:.2f}'.format)  # Float formatting

# Reset to defaults
pd.reset_option('all')
```

### Custom Display Functions

```python
def quick_overview(df):
    """Provide a quick overview of the DataFrame"""
    print("=== DataFrame Overview ===")
    print(f"Shape: {df.shape}")
    print(f"Columns: {list(df.columns)}")
    print("\nData Types:")
    print(df.dtypes)
    print(f"\nMissing Values: {df.isnull().sum().sum()}")
    print(f"Duplicate Rows: {df.duplicated().sum()}")
    print("\nFirst 5 rows:")
    print(df.head())
    print("\nStatistical Summary:")
    print(df.describe())

quick_overview(df)
```

### Conditional Viewing

```python
# View rows meeting conditions
high_salary = df[df['Salary'] > 60000]
print("High salary employees:")
print(high_salary)

# View specific columns for filtered rows
young_employees = df[df['Age'] < 30][['Name', 'Age', 'City']]
print("Young employees:")
print(young_employees)
```

## Series-Specific Viewing

```python
# For Series objects
age_series = df['Age']

print(age_series.head())
print(f"Series dtype: {age_series.dtype}")
print(f"Series size: {age_series.size}")

# Value counts for Series
print(age_series.value_counts().sort_index())
```

## Exporting Summary Information

```python
# Create a summary DataFrame
summary = pd.DataFrame({
    'Column': df.columns,
    'Data Type': df.dtypes,
    'Non-Null Count': df.notnull().sum(),
    'Null Count': df.isnull().sum(),
    'Unique Values': [df[col].nunique() for col in df.columns]
})

print("Data Summary:")
print(summary)
```

## Visual Inspection with Pandas

```python
# Basic plotting (requires matplotlib)
try:
    df['Age'].hist()
    df.plot.scatter(x='Age', y='Salary')
except ImportError:
    print("Matplotlib not available for plotting")
```

## Best Practices for Data Viewing

1. **Start with df.head() and df.info()** for initial exploration
2. **Check for missing values** early in your analysis
3. **Use df.describe()** to understand numeric distributions
4. **Examine unique values** in categorical columns
5. **Set appropriate display options** for large DataFrames
6. **Create custom summary functions** for repeated analysis
7. **Document your findings** as you explore the data
8. **Validate data quality** before proceeding with analysis

## Common Patterns

### Data Profile Report

```python
def create_data_profile(df):
    """Create a comprehensive data profile report"""
    profile = {}

    # Basic info
    profile['shape'] = df.shape
    profile['columns'] = list(df.columns)
    profile['dtypes'] = df.dtypes.to_dict()

    # Missing data
    profile['missing_counts'] = df.isnull().sum().to_dict()
    profile['missing_percentages'] = (df.isnull().sum() / len(df) * 100).to_dict()

    # Numeric statistics
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    if len(numeric_cols) > 0:
        profile['numeric_stats'] = df[numeric_cols].describe().to_dict()

    # Categorical statistics
    categorical_cols = df.select_dtypes(include=['object', 'category']).columns
    if len(categorical_cols) > 0:
        cat_stats = {}
        for col in categorical_cols:
            cat_stats[col] = {
                'unique_values': df[col].nunique(),
                'top_values': df[col].value_counts().head(5).to_dict()
            }
        profile['categorical_stats'] = cat_stats

    return profile

# Usage
profile = create_data_profile(df)
import json
print(json.dumps(profile, indent=2, default=str))
```

### Quick Data Health Check

```python
def data_health_check(df):
    """Perform basic data health checks"""
    issues = []

    # Check for missing values
    missing = df.isnull().sum()
    if missing.any():
        issues.append(f"Missing values found: {missing[missing > 0].to_dict()}")

    # Check for duplicates
    duplicates = df.duplicated().sum()
    if duplicates > 0:
        issues.append(f"Duplicate rows found: {duplicates}")

    # Check data types
    if df.select_dtypes(include=['object']).shape[1] > 0:
        # Could check for mixed types in object columns
        pass

    # Check for outliers (simple check)
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    for col in numeric_cols:
        q1 = df[col].quantile(0.25)
        q3 = df[col].quantile(0.75)
        iqr = q3 - q1
        outliers = df[(df[col] < q1 - 1.5 * iqr) | (df[col] > q3 + 1.5 * iqr)]
        if len(outliers) > 0:
            issues.append(f"Potential outliers in {col}: {len(outliers)} rows")

    if not issues:
        print("✅ Data health check passed - no issues found")
    else:
        print("⚠️  Data health issues found:")
        for issue in issues:
            print(f"  - {issue}")

# Usage
data_health_check(df)
```
