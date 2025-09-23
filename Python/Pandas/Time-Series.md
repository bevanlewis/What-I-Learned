# Time Series Data

Pandas excels at handling time series data with powerful date/time functionality.

## Creating Time Series

```python
import pandas as pd
import numpy as np

# Create date range
dates = pd.date_range('2023-01-01', periods=10, freq='D')
print(dates)

# Create time series DataFrame
ts_df = pd.DataFrame({
    'date': dates,
    'value': np.random.randn(10)
})
print(ts_df)
```

## DateTime Operations

### Converting to DateTime

```python
# Convert string to datetime
df = pd.DataFrame({'date_str': ['2023-01-01', '2023-02-01', '2023-03-01']})
df['date'] = pd.to_datetime(df['date_str'])
print(df.dtypes)
```

### Extracting Date Components

```python
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day'] = df['date'].dt.day
df['weekday'] = df['date'].dt.day_name()
print(df)
```

## Resampling

### Downsampling

```python
# Create daily data
daily_data = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=90, freq='D'),
    'value': np.random.randn(90)
})

# Resample to monthly
monthly = daily_data.set_index('date').resample('M').mean()
print(monthly)
```

### Upsampling

```python
# Upsample monthly to daily
monthly_data = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=3, freq='M'),
    'value': [100, 110, 120]
})

daily_upsampled = monthly_data.set_index('date').resample('D').ffill()
print(daily_upsampled.head())
```

## Rolling Windows

```python
# Rolling mean
ts_data = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=30, freq='D'),
    'value': np.random.randn(30)
})

ts_data['rolling_mean_7'] = ts_data['value'].rolling(window=7).mean()
ts_data['rolling_std_7'] = ts_data['value'].rolling(window=7).std()
print(ts_data.head(10))
```

## Time-based Selection

```python
# Select date range
subset = ts_data[(ts_data['date'] >= '2023-01-10') & (ts_data['date'] <= '2023-01-20')]
print(subset)

# Select by year/month
january_data = ts_data[ts_data['date'].dt.month == 1]
print(january_data)
```

## Time Zones

```python
# Work with time zones
naive_time = pd.Timestamp('2023-01-01 12:00:00')
aware_time = naive_time.tz_localize('UTC')
print(aware_time)

# Convert time zones
est_time = aware_time.tz_convert('US/Eastern')
print(est_time)
```

## Best Practices

1. **Set datetime index** for time series operations
2. **Use appropriate frequency** for date ranges
3. **Handle time zones** consistently
4. **Validate date conversions** to ensure correctness
