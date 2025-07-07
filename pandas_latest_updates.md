# Pandas Latest Updates 🐼 (Version 2.3.0)

This document summarizes the key updates in pandas version 2.3.0, as documented with a release date of June 4, 2025. 📅

## 🔑 Key Updates & Enhancements

### `StringDtype` Enhancements

**説明 (Explanation):**
Pandas 2.3.0 brings several enhancements to `StringDtype` and its operations, improving consistency and functionality, especially when interacting with PyArrow.

1.  **`Series.str.decode()` の `dtype` 引数 (New `dtype` argument for `Series.str.decode()`):**
    *   `Series.str.decode()` now has a `dtype` argument to control the output data type.
    *   When `future.infer_string` is `True`, `Series.str.decode()` will now result in `StringDtype`.

2.  **`to_hdf()` と `StringDtype` のラウンドトリップ (Roundtrip of `StringDtype` with `to_hdf()`):**
    *   `to_hdf()` now correctly round-trips data with `StringDtype`.

3.  **`StringDtype` の累積演算 (Cumulative operations for `StringDtype`):**
    *   `cumsum()`, `cummin()`, and `cummax()` reductions are now implemented for `StringDtype` columns.

4.  **`StringDtype` の合計 (`sum()`) 演算 (Sum reduction for `StringDtype`):**
    *   The `sum()` reduction is now implemented for `StringDtype` columns.

**コード例 (Code Example):**
```python
import pandas as pd

# Example for Series.str.decode() with new dtype argument (conceptual)
# Assuming 's_bytes' is a Series of byte strings
# s_decoded_string = s_bytes.str.decode(encoding='utf-8', dtype=pd.StringDtype())

# Example for StringDtype sum
s_str = pd.Series(["a", "b", "c"], dtype=pd.StringDtype())
print(f"Sum of StringDtype Series: {s_str.sum()}")
# Expected: "abc"

# Example for StringDtype cumsum
print(f"Cumsum of StringDtype Series:\n{s_str.cumsum()}")
# Expected:
# 0      a
# 1     ab
# 2    abc
# dtype: string
```

**📝 チートシート (Cheat Sheet):**
-   **`Series.str.decode()`:** Gains `dtype` argument; returns `StringDtype` if `future.infer_string=True`. ⚙️
-   **`to_hdf()`:** Improved `StringDtype` round-tripping. 💾
-   **Cumulative Ops:** `cumsum()`, `cummin()`, `cummax()` now work with `StringDtype`. ➕➖
-   **`sum()` for `StringDtype`:** Concatenates strings. 🔗

### Improved NumPy >= 2 Compatibility

**説明 (Explanation):**
The semantics for the `copy` keyword in `__array__` methods (used in `np.array()` or `np.asarray()`) have been updated for correct behavior with NumPy version 2 and above. This enhances the interoperability between pandas and the newer versions of NumPy.

**コード例 (Code Example):**
```python
import pandas as pd
import numpy as np

s = pd.Series([1, 2, 3])

# Behavior with NumPy >= 2 and pandas 2.3.0
# np.asarray(s, copy=False) # This will now more reliably avoid a copy
#                          # if underlying data allows, per NumPy 2's NEP 51.
```
**📝 チートシート (Cheat Sheet):**
-   **`__array__` methods:** Updated `copy` keyword semantics for NumPy >= 2. 🤝
-   **Benefit:** More predictable copy/view behavior when converting pandas objects to NumPy arrays. ✨

### API Changes with `future.infer_string`

**説明 (Explanation):**
When the `future.infer_string` option is enabled, `Index` set operations (like `union` or `intersection`) will now ignore the `dtype` of an empty `RangeIndex` or an empty `Index` with `object` dtype when determining the resulting `Index`'s `dtype`.

**📝 チートシート (Cheat Sheet):**
-   **`Index` set operations:** Behavior change with `future.infer_string=True` for empty `RangeIndex`/`object` dtype `Index`. 🔀
-   **Impact:** More consistent `dtype` inference for `Index` operations under this future flag.

## Notable Bug Fixes 🛠️

*   **Comparison of different string dtypes:** Pandas now has a defined hierarchy for determining the result `dtype` when comparing `Series` with different string dtypes (e.g., `StringDtype("pyarrow")` vs `StringDtype("python")`), leading to more consistent `boolean[pyarrow]` or `boolean` (NumPy-backed nullable) results.
*   **`Series.mode()` / `DataFrame.mode()` with `dropna=False`:** Fixed sorting issues in the presence of NA values for all dtypes.
*   **`Series.round()` with `object` dtype:** No longer raises a `TypeError`.
*   **String dtype aggregations (`min`, `max`, `sum`) with all NA:** Fixed issues where `min`/`max` returned float instead of string dtype, and `sum` returned 0 instead of an empty string for `StringDtype`.
*   **`Series.replace()` with `StringDtype`:** Correctly upcasts to `object` dtype when replacing with a non-string value.
*   **`DataFrame.to_excel()`:** Fixed bug where decimals were stored as strings instead of numbers.

## 📉 Deprecations

*   **Non-bool `na` in `str.contains/startswith/endswith`:** Deprecated allowing non-boolean values for the `na` parameter in these string methods.
*   **`StringDtype` "pyarrow_numpy" storage:** The `"pyarrow_numpy"` storage option for `StringDtype` is deprecated.
*   **`DataFrameGroupBy.apply(include_groups=True)`:** The deprecation for `include_groups=True` has been promoted to a `FutureWarning`; eventually, only `False` will be allowed.

## 📜 Summary

Pandas 2.3.0 introduces significant enhancements, particularly around `StringDtype` functionality and compatibility with NumPy 2. It also includes important API clarifications and bug fixes that refine existing behaviors. Users should also note the deprecations to prepare for future changes.

For a full list of all changes, please refer to the official [Pandas 2.3.0 Release Notes](https://pandas.pydata.org/docs/whatsnew/v2.3.0.html). 🔗
