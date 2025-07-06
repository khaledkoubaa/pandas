# Pandas Latest Updates 🐼 (Version 2.2.3)

This document summarizes the latest updates in pandas version 2.2.3, released on September 20, 2024. 📅

## 🔑 Key Updates

### 🐍 1. Python 3.13 Compatibility

**Explanation:**
Pandas 2.2.3 is now officially compatible with the upcoming Python 3.13 release. This ensures that users can smoothly transition to the latest Python version without encountering compatibility issues with pandas. Wheels for both free-threaded and normal Python 3.13 will be available.

**Code Example:**
Since this is a compatibility update, there isn't a specific code example to demonstrate it. However, you can ensure your environment is set up for Python 3.13 and install pandas 2.2.3 as usual:
```bash
# Example of creating a Python 3.13 environment (syntax may vary based on your environment manager)
# conda create -n py313 python=3.13
# source activate py313

pip install pandas==2.2.3
```
```python
import pandas as pd
print(pd.__version__)
# Expected output: 2.2.3 (if running in a Python 3.13 environment)
```

**📝 Cheat Sheet:**
- **Update:** Pandas 2.2.3 includes fixes for NumPy 2.1 compatibility. ✅
- **Action:** Ensure your pandas (2.2.3+) and NumPy (2.1+) versions are up-to-date for optimal performance. 🚀
- **Benefit:** Continued stable interaction between pandas and NumPy. 🤝

### 2. NumPy 2.1 Compatibility

**Explanation:**
Minor fixes have been implemented in pandas 2.2.3 to ensure compatibility with NumPy version 2.1. This maintains interoperability between these two critical libraries in the Python data science ecosystem.

**Code Example:**
Similar to Python 3.13 compatibility, this update ensures that pandas functions correctly when used with NumPy 2.1.
```python
import pandas as pd
import numpy as np

print(f"Pandas version: {pd.__version__}")
print(f"NumPy version: {np.__version__}")

# Example of a pandas operation that relies on NumPy
s = pd.Series([1, 2, 3, np.nan])
print(s.mean())
# This should work correctly with pandas 2.2.3 and NumPy 2.1
```

**📝 Cheat Sheet:**
- **Update:** Fixed bug in `pd.eval()` related to complex number division (GH 21374). 🛠️
- **Affected Function:** `pandas.eval()`
- **Change:** Operations involving complex numbers (especially division) within `eval()` now produce correct results, retaining the imaginary part as expected. 💯
- **Benefit:** More reliable calculations when using `eval()` with complex data types. 💡

## ⚙️ Other Changes

- **Missing Licenses:** Licenses for 3rd party dependencies that were missing have been added back into the wheels. This is primarily a packaging and compliance update. 📄

## 📜 Summary

Pandas 2.2.3 is a patch release focused on compatibility improvements (Python 3.13, NumPy 2.1) and bug fixes. While it doesn't introduce new features, these updates are important for ensuring stability and interoperability within the broader Python ecosystem. 🌍

For a full list of changes, please refer to the official [Pandas 2.2.3 Release Notes](https://pandas.pydata.org/docs/whatsnew/v2.2.3.html). 🔗
