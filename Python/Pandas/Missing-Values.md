# Handling Missing Values

Missing values (NaN, None, etc.) are common in real-world data. Pandas provides comprehensive tools for detecting, removing, and filling missing values.

## Detecting Missing Values

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, 7, 8],
    'C': [9, 10, 11, np.nan]
})

# Check for missing values
print(df.isnull())
print(df.isna())  # Same as isnull()

# Count missing values per column
print(df.isnull().sum())

# Total missing values
print(df.isnull().sum().sum())
```

## Removing Missing Values

### Drop Rows with Missing Values

```python
# Drop any row with missing values
df_clean = df.dropna()
print(df_clean)

# Drop rows where specific columns have missing values
df_clean_specific = df.dropna(subset=['A', 'B'])
print(df_clean_specific)

# Drop rows with all missing values
df_clean_all = df.dropna(how='all')
print(df_clean_all)
```

### Drop Columns with Missing Values

```python
# Drop columns with any missing values
df_clean_cols = df.dropna(axis=1)
print(df_clean_cols)

# Drop columns with all missing values
df_clean_cols_all = df.dropna(axis=1, how='all')
print(df_clean_cols_all)
```

## Filling Missing Values

### Fill with Specific Value

```python
# Fill all missing values with 0
df_filled = df.fillna(0)
print(df_filled)

# Fill with different values for different columns
df_filled_dict = df.fillna({'A': 0, 'B': -1, 'C': 999})
print(df_filled_dict)
```

### Fill with Statistical Measures

```python
# Fill with mean
df_filled_mean = df.fillna(df.mean())
print(df_filled_mean)

# Fill with median
df_filled_median = df.fillna(df.median())
print(df_filled_median)

# Fill with mode (most frequent value)
df_filled_mode = df.apply(lambda x: x.fillna(x.mode()[0]) if not x.mode().empty else x)
print(df_filled_mode)
```

### Forward and Backward Fill

```python
# Forward fill (use previous value)
df_ffill = df.fillna(method='ffill')
print(df_ffill)

# Backward fill (use next value)
df_bfill = df.fillna(method='bfill')
print(df_bfill)

# Fill with limit
df_limited = df.fillna(method='ffill', limit=1)
print(df_limited)
```

## Advanced Missing Value Handling

### Interpolate Missing Values

```python
# Linear interpolation
df_interpolated = df.interpolate(method='linear')
print(df_interpolated)

# Polynomial interpolation
df_poly = df.interpolate(method='polynomial', order=2)
print(df_poly)
```

### Conditional Filling

```python
# Fill based on conditions
df_conditional = df.copy()
df_conditional.loc[df_conditional['A'].isnull(), 'A'] = df_conditional['B'] * 2
print(df_conditional)
```

## Working with Different Missing Value Representations

```python
# Handle various missing value representations
df_mixed = pd.DataFrame({
    'A': [1, 2, 'NA', 4, 'N/A', None, 'null'],
    'B': [5, 'missing', 7, 8, '', 'NULL', 11]
})

# Convert various representations to NaN
df_cleaned = df_mixed.replace(['NA', 'N/A', 'null', 'NULL', 'missing', ''], np.nan)
print(df_cleaned)
```

## Missing Value Analysis

### Missing Value Patterns

```python
# Percentage of missing values
missing_percent = (df.isnull().sum() / len(df)) * 100
print(missing_percent)

# Visualize missing patterns
import seaborn as sns
import matplotlib.pyplot as plt

# Create missing value heatmap (if seaborn available)
try:
    plt.figure(figsize=(10, 6))
    sns.heatmap(df.isnull(), cbar=False, cmap='viridis')
    plt.title('Missing Values Heatmap')
    plt.show()
except ImportError:
    print("Seaborn not available for visualization")
```

### Correlation with Missing Values

```python
# Check if missing values in one column correlate with another
missing_corr = df.isnull().corr()
print(missing_corr)
```

## Best Practices

### Data Quality Checks

```python
def assess_data_quality(df):
    """Assess data quality including missing values."""
    quality_report = {
        'total_rows': len(df),
        'total_columns': len(df.columns),
        'missing_cells': df.isnull().sum().sum(),
        'missing_percentage': (df.isnull().sum().sum() / (len(df) * len(df.columns))) * 100,
        'columns_with_missing': (df.isnull().sum() > 0).sum(),
        'rows_with_missing': (df.isnull().sum(axis=1) > 0).sum()
    }

    print("Data Quality Report:")
    for key, value in quality_report.items():
        print(f"{key}: {value}")

    return quality_report

report = assess_data_quality(df)
```

### Appropriate Filling Strategies

```python
def smart_fill_missing(df):
    """Apply appropriate filling strategies based on data type."""
    df_filled = df.copy()

    for col in df.columns:
        if df[col].isnull().any():
            if df[col].dtype in ['int64', 'float64']:
                # For numeric columns, use median to handle outliers
                df_filled[col] = df[col].fillna(df[col].median())
            elif df[col].dtype == 'object':
                # For categorical columns, use mode
                mode_val = df[col].mode()
                if not mode_val.empty:
                    df_filled[col] = df[col].fillna(mode_val[0])
            # For datetime, you might want forward/backward fill
            elif pd.api.types.is_datetime64_any_dtype(df[col]):
                df_filled[col] = df[col].fillna(method='ffill')

    return df_filled

df_smart_filled = smart_fill_missing(df)
print(df_smart_filled)
```

## Common Patterns

### Time Series Missing Values

```python
# For time series data
ts_df = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=10),
    'value': [1, 2, np.nan, 4, 5, np.nan, np.nan, 8, 9, 10]
})

# Time-aware filling
ts_df['value_filled'] = ts_df['value'].fillna(method='ffill')
print(ts_df)
```

### Group-wise Filling

```python
# Fill missing values within groups
grouped_df = pd.DataFrame({
    'group': ['A', 'A', 'B', 'B', 'A'],
    'value': [1, np.nan, 3, np.nan, 5]
})

grouped_df['filled'] = grouped_df.groupby('group')['value'].transform(lambda x: x.fillna(x.mean()))
print(grouped_df)
```

## Performance Considerations

### Efficient Missing Value Detection

```python
# For large DataFrames, use vectorized operations
large_df = pd.DataFrame(np.random.randn(100000, 5))
large_df.iloc[::10, :] = np.nan  # Add missing values

# Efficient missing value counts
missing_counts = large_df.isnull().sum()
print(f"Missing values per column:\n{missing_counts}")

# Check if any missing values exist (fast)
has_missing = large_df.isnull().any().any()
print(f"Has missing values: {has_missing}")
```

## Validation After Handling Missing Values

```python
def validate_missing_value_handling(original_df, cleaned_df):
    """Validate that missing value handling was successful."""
    original_missing = original_df.isnull().sum().sum()
    cleaned_missing = cleaned_df.isnull().sum().sum()

    print(f"Original missing values: {original_missing}")
    print(f"Remaining missing values: {cleaned_missing}")
    print(f"Missing values handled: {original_missing - cleaned_missing}")

    # Check for unintended changes
    if len(cleaned_df) != len(original_df):
        print(f"Warning: Row count changed from {len(original_df)} to {len(cleaned_df)}")

    return cleaned_missing == 0

# Usage
is_clean = validate_missing_value_handling(df, df_filled_mean)
print(f"Data is clean: {is_clean}")
```

This guide covers comprehensive techniques for handling missing values in pandas, from basic detection to advanced filling strategies and validation.
