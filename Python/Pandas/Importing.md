# Importing Pandas

## Standard Import

```python
import pandas as pd
```

This is the standard and recommended way to import pandas. The `pd` alias is universally accepted in the pandas community.

## Alternative Import Methods

### Import specific components

```python
from pandas import DataFrame, Series
from pandas import read_csv
```

### Import with different alias

```python
import pandas as pds
```

### Import submodules

```python
import pandas.io  # For I/O operations
import pandas.tools  # For utility functions
```

## Common Imports with Pandas

Pandas is often used with these libraries:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

## Checking Pandas Version

```python
import pandas as pd

print(f"Pandas version: {pd.__version__}")
print(f"NumPy version: {pd.np.__version__}")  # Pandas includes NumPy
```

## Setting Display Options

```python
import pandas as pd

# Set display options for better output formatting
pd.set_option('display.max_columns', None)  # Show all columns
pd.set_option('display.max_rows', 100)     # Show up to 100 rows
pd.set_option('display.width', 1000)       # Set display width
pd.set_option('display.float_format', '{:.2f}'.format)  # Format floats
```

## Importing for Development

```python
import pandas as pd

# Enable all warnings (useful during development)
import warnings
warnings.filterwarnings('default')

# Or suppress specific warnings
warnings.filterwarnings('ignore', category=FutureWarning)
```

## Memory Management

```python
import pandas as pd

# Check pandas memory usage
print(f"Pandas memory usage: {pd.memory_usage(deep=True)}")

# For large datasets, consider chunked reading
for chunk in pd.read_csv('large_file.csv', chunksize=10000):
    process_chunk(chunk)
```

## Best Practices

1. **Always use the standard import**: `import pandas as pd`
2. **Set display options early** in your scripts/notebooks
3. **Check pandas version** when debugging issues
4. **Import related libraries together** (numpy, matplotlib, etc.)
5. **Consider memory implications** for large datasets
