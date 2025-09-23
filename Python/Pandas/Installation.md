# Pandas Installation

## Installing Pandas

Pandas can be installed using pip or conda package managers.

### Using pip

```bash
pip install pandas
```

### Using conda

```bash
conda install pandas
```

### Installing with dependencies

For full functionality including optional dependencies:

```bash
pip install pandas[all]
```

### Installing specific version

```bash
pip install pandas==1.5.3
```

## System Requirements

### Python Version

- Python 3.8 or higher is required for pandas 2.0+
- Python 3.6.1+ for pandas 1.0+

### Operating Systems

- Windows
- macOS
- Linux

## Optional Dependencies

Pandas can benefit from additional packages for enhanced functionality:

### For Excel I/O

```bash
pip install openpyxl xlrd
```

### For SQL I/O

```bash
pip install sqlalchemy
```

### For HTML I/O

```bash
pip install lxml beautifulsoup4 html5lib
```

### For Parquet I/O

```bash
pip install pyarrow fastparquet
```

### For HDF5 I/O

```bash
pip install tables
```

## Verifying Installation

```python
import pandas as pd

print(f"Pandas version: {pd.__version__}")
```

## Upgrading Pandas

```bash
pip install --upgrade pandas
```

## Uninstalling Pandas

```bash
pip uninstall pandas
```
