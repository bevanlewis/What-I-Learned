# Pivot Tables

Pivot tables allow you to reshape and aggregate data in a flexible way, similar to Excel pivot tables.

## Creating Pivot Tables

```python
import pandas as pd
import numpy as np

# Sample sales data
df = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=20, freq='D'),
    'product': np.random.choice(['A', 'B', 'C'], 20),
    'region': np.random.choice(['North', 'South', 'East', 'West'], 20),
    'sales': np.random.randint(100, 1000, 20)
})

# Basic pivot table
pivot = pd.pivot_table(df,
                      values='sales',
                      index='product',
                      columns='region',
                      aggfunc='sum')
print(pivot)
```

## Aggregation Functions

```python
# Multiple aggregation functions
pivot_multi = pd.pivot_table(df,
                           values='sales',
                           index='product',
                           columns='region',
                           aggfunc=['sum', 'mean', 'count'])
print(pivot_multi)
```

## Custom Aggregation

```python
# Custom aggregation function
def custom_agg(x):
    return x.max() - x.min()

pivot_custom = pd.pivot_table(df,
                             values='sales',
                             index='product',
                             columns='region',
                             aggfunc=custom_agg)
print(pivot_custom)
```

## Multiple Index/Columns

```python
# Add month to data
df['month'] = df['date'].dt.month

# Multiple index levels
pivot_multi_idx = pd.pivot_table(df,
                               values='sales',
                               index=['month', 'product'],
                               columns='region',
                               aggfunc='sum')
print(pivot_multi_idx)
```

## Handling Missing Values

```python
# Fill missing values
pivot_filled = pd.pivot_table(df,
                             values='sales',
                             index='product',
                             columns='region',
                             aggfunc='sum',
                             fill_value=0)
print(pivot_filled)
```

## Margins and Totals

```python
# Add row and column totals
pivot_margins = pd.pivot_table(df,
                              values='sales',
                              index='product',
                              columns='region',
                              aggfunc='sum',
                              margins=True,
                              margins_name='Total')
print(pivot_margins)
```

## Best Practices

1. **Choose appropriate aggregation functions** based on your data
2. **Handle missing values** explicitly
3. **Use meaningful index and column selections**
4. **Consider memory usage** with large pivot tables
5. **Validate results** after creating pivot tables
