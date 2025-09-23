# Removing Duplicates

Duplicate data can skew analysis results. Pandas provides tools to identify and remove duplicate rows or values.

## Detecting Duplicates

```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Alice', 'Charlie', 'Bob'],
    'age': [25, 30, 25, 35, 30],
    'city': ['NYC', 'LA', 'NYC', 'Chicago', 'LA']
})

# Check for duplicate rows
print(df.duplicated())

# Count duplicates
print(f"Number of duplicate rows: {df.duplicated().sum()}")

# Check duplicates in specific columns
print(df.duplicated(subset=['name', 'age']))
```

## Removing Duplicates

### Remove All Duplicate Rows

```python
# Remove duplicate rows (keeps first occurrence)
df_unique = df.drop_duplicates()
print(df_unique)
```

### Remove Duplicates Based on Specific Columns

```python
# Remove duplicates based on name column
df_unique_name = df.drop_duplicates(subset=['name'])
print(df_unique_name)

# Remove duplicates based on multiple columns
df_unique_name_age = df.drop_duplicates(subset=['name', 'age'])
print(df_unique_name_age)
```

### Keep Last Occurrence

```python
# Keep last occurrence instead of first
df_keep_last = df.drop_duplicates(keep='last')
print(df_keep_last)
```

### Remove All Duplicates

```python
# Remove all rows that are duplicates (no rows kept)
df_remove_all = df.drop_duplicates(keep=False)
print(df_remove_all)
```

## Advanced Duplicate Handling

### Mark Duplicates for Inspection

```python
# Add column to mark duplicates
df['is_duplicate'] = df.duplicated()
print(df)

# Mark duplicates by specific columns
df['name_duplicate'] = df.duplicated(subset=['name'])
print(df)
```

### Get Duplicate Rows Only

```python
# Get only the duplicate rows
duplicate_rows = df[df.duplicated()]
print(duplicate_rows)

# Get all occurrences of duplicates
all_duplicates = df[df.duplicated(keep=False)]
print(all_duplicates)
```

## Best Practices

1. **Inspect duplicates first** before removing them
2. **Specify subset columns** when appropriate
3. **Choose keep strategy** based on your needs
4. **Validate results** after duplicate removal

```python
def safe_remove_duplicates(df, subset=None, keep='first'):
    """Safely remove duplicates with validation."""
    original_count = len(df)

    df_clean = df.drop_duplicates(subset=subset, keep=keep)

    removed_count = original_count - len(df_clean)
    print(f"Removed {removed_count} duplicate rows")

    return df_clean

# Usage
df_clean = safe_remove_duplicates(df, subset=['name', 'age'])
```
