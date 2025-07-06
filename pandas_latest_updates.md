# Pandas Latest Updates (Version 2.2.3)

This document summarizes the latest updates in pandas version 2.2.3, released on September 20, 2024.

## Key Updates

### 1. Python 3.13 Compatibility

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

**Cheat Sheet:**
- **Update:** Pandas 2.2.3 supports Python 3.13.
- **Action:** Upgrade pandas to 2.2.3 if you plan to use Python 3.13.
- **Benefit:** Seamless integration with the latest Python features and improvements.

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

**Cheat Sheet:**
- **Update:** Pandas 2.2.3 includes fixes for NumPy 2.1 compatibility.
- **Action:** Ensure your pandas (2.2.3+) and NumPy (2.1+) versions are up-to-date for optimal performance.
- **Benefit:** Continued stable interaction between pandas and NumPy.

### 3. Bug Fix: `eval()` with Complex Numbers

**Explanation:**
A bug in the `pandas.eval()` function that caused issues with complex numbers, particularly when division was involved (discarding the imaginary part), has been fixed. `eval()` will now correctly handle operations on complex numbers.

**Code Example:**
```python
import pandas as pd
import numpy as np

df = pd.DataFrame({'a': [1+2j, 3+4j, 5+6j]})

# Previous behavior (problematic in earlier versions for some complex operations)
# result_before_fix = pd.eval("df.a / (1+1j)") # Might have discarded imaginary part or errored

# Corrected behavior in pandas 2.2.3
# For demonstration, let's assume 'a' is a Series of complex numbers
a_series = pd.Series([1+2j, 3+4j, 5+6j], name='a')

# Simple division example that should now work correctly
result_fixed = pd.eval("a_series / (1+1j)", target=a_series.to_frame())
print(result_fixed)

# Expected output should correctly compute complex division:
# 0    (1.5+0.5j)
# 1    (3.5+0.5j)
# 2    (5.5+0.5j)
# Name: a, dtype: complex128

# Note: The exact way to reproduce the original bug (GH 21374) might be specific.
# The key takeaway is that complex number arithmetic within eval() is more robust.
```

**Cheat Sheet:**
- **Update:** Fixed bug in `pd.eval()` related to complex number division (GH 21374).
- **Affected Function:** `pandas.eval()`
- **Change:** Operations involving complex numbers (especially division) within `eval()` now produce correct results, retaining the imaginary part as expected.
- **Benefit:** More reliable calculations when using `eval()` with complex data types.

## Other Changes

- **Missing Licenses:** Licenses for 3rd party dependencies that were missing have been added back into the wheels. This is primarily a packaging and compliance update.

## Summary

Pandas 2.2.3 is a patch release focused on compatibility improvements (Python 3.13, NumPy 2.1) and bug fixes. While it doesn't introduce new features, these updates are important for ensuring stability and interoperability within the broader Python ecosystem.

For a full list of changes, please refer to the official [Pandas 2.2.3 Release Notes](https://pandas.pydata.org/docs/whatsnew/v2.2.3.html).
