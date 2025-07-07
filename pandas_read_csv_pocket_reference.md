# Pandas `read_csv()` Pocket Reference 📖

`pandas.read_csv()` is a powerful and versatile function for reading comma-separated values (CSV) files into a DataFrame. This reference covers some of the most commonly used parameters.

**Basic Usage:**
```python
import pandas as pd

# Assuming 'data.csv' exists in the same directory
# df = pd.read_csv('data.csv')
# print(df.head())
```

---

## 📁 File Handling & Path

### `filepath_or_buffer`
-   **Purpose:** Specifies the path to the CSV file (string), a URL, or any object with a `read()` method (e.g., file handle or StringIO).
-   **Example:**
    ```python
    # From a local file
    # df_local = pd.read_csv('my_data.csv')

    # From a URL
    # url = 'https://raw.githubusercontent.com/plotly/datasets/master/auto-mpg.csv'
    # df_url = pd.read_csv(url)
    # print(df_url.head())

    # From a StringIO object (useful for testing or in-memory CSV data)
    from io import StringIO
    csv_data = "col1,col2\n1,A\n2,B"
    df_stringio = pd.read_csv(StringIO(csv_data))
    print(df_stringio)
    ```

---

## 📝 Data Parsing & Structure

### `sep` or `delimiter`
-   **Purpose:** The delimiter used to separate values in the file. If not specified, pandas attempts to infer it (commonly a comma).
-   **Example:**
    ```python
    # For a tab-separated file (TSV)
    # df_tsv = pd.read_csv('data.tsv', sep='\\t')

    # For a semicolon-separated file
    csv_data_semi = "name;age\nAlice;30\nBob;24"
    df_semi = pd.read_csv(StringIO(csv_data_semi), sep=';')
    print(df_semi)
    ```

### `header`
-   **Purpose:** Specifies the row number(s) to use as the column names. Defaults to `0` (first row). Use `None` if the file has no header.
-   **Example:**
    ```python
    # File with no header
    csv_no_header = "1,apple\n2,banana\n3,cherry"
    df_no_head = pd.read_csv(StringIO(csv_no_header), header=None, names=['id', 'fruit'])
    print(df_no_head)

    # File with header on the second row (row index 1)
    # If data.csv was:
    # Junk Line
    # ColA,ColB
    # 1,X
    # 2,Y
    # df_header_row1 = pd.read_csv('data.csv', header=1) # ColA, ColB would be headers
    ```

### `names`
-   **Purpose:** A list of column names to use. Typically used when `header=None` or to override existing headers.
-   **Example:**
    ```python
    csv_vals = "valA,valB\n1,x\n2,y"
    df_custom_names = pd.read_csv(StringIO(csv_vals), header=0, names=['MetricA', 'MetricB']) # Overrides header
    print(df_custom_names)
    ```

### `index_col`
-   **Purpose:** Column(s) to use as the row labels (index) of the DataFrame. Can be an integer (column position), string (column name), or a list for a MultiIndex.
-   **Example:**
    ```python
    csv_with_id = "id,name,score\n1,Jim,88\n2,Pam,92"
    df_indexed = pd.read_csv(StringIO(csv_with_id), index_col='id')
    print(df_indexed)

    # Using the first column (index 0) as index
    df_indexed_pos = pd.read_csv(StringIO(csv_with_id), index_col=0)
    print(df_indexed_pos)
    ```

### `usecols`
-   **Purpose:** A list of column names or integer positions to read from the CSV. Helps in reading only specific columns, saving memory and time.
-   **Example:**
    ```python
    csv_full = "name,age,city,country\nDwight,35,Scranton,USA\nMichael,45,Scranton,USA"
    df_subset = pd.read_csv(StringIO(csv_full), usecols=['name', 'city'])
    print(df_subset)

    # Using column positions
    df_subset_pos = pd.read_csv(StringIO(csv_full), usecols=[0, 2]) # Reads 'name' and 'city'
    print(df_subset_pos)
    ```

### `dtype`
-   **Purpose:** A dictionary specifying the data type for columns. E.g., `{'col_name': str, 'col_id': 'Int64'}`. Use `str` or `object` to preserve leading zeros or mixed types.
-   **Example:**
    ```python
    csv_types = "user_id,value,category\n001,10.5,A\n002,20.0,B"
    df_typed = pd.read_csv(StringIO(csv_types), dtype={'user_id': str, 'value': float})
    print(df_typed.dtypes)
    # Expected dtypes:
    # user_id      object  (or pandas StringDtype if future.infer_string is True)
    # value       float64
    # category     object
    # dtype: object
    ```

---

## 🧹 Data Cleaning & Handling Missing Values

### `na_values`
-   **Purpose:** A scalar, string, list, or dictionary of strings to recognize as NA/NaN.
-   **Example:**
    ```python
    csv_missing = "name,value\nKevin,100\nOscar,N/A\nAngela,90\nStanley,missing"
    df_na = pd.read_csv(StringIO(csv_missing), na_values=['N/A', 'missing', ''])
    print(df_na)
    #     name  value
    # 0  Kevin  100.0
    # 1  Oscar    NaN
    # 2 Angela   90.0
    # 3 Stanley   NaN
    ```

### `skiprows`
-   **Purpose:** Number of lines to skip at the beginning of the file (integer) or a list of 0-indexed row numbers to skip.
-   **Example:**
    ```python
    csv_with_junk = "This is some junk text\\nAnother line of junk\\nname,value\\nCreed,10"
    # Skip the first 2 lines
    df_skipped = pd.read_csv(StringIO(csv_with_junk), skiprows=2)
    print(df_skipped)

    # Skip specific rows (e.g., row 0 and row 2 of the data part, after header)
    csv_data_to_skip_rows = "colA,colB\nAAA,111\nBBB,222\nCCC,333\nDDD,444"
    # skiprows=[1, 3] will skip the first data line ('AAA,111') and the third data line ('CCC,333').
    # Note: row indices for skiprows are relative to the start of the file after 'header' rows are accounted for.
    # If header=0 (default), skiprows=[1] skips the second line of the file.
    df_skip_specific = pd.read_csv(StringIO(csv_data_to_skip_rows), skiprows=[1, 3])
    print("DataFrame with specific rows skipped:")
    print(df_skip_specific)
    # Expected output:
    # DataFrame with specific rows skipped:
    #   colA colB
    # 0  BBB  222
    # 1  DDD  444
    ```

### `skipfooter`
-   **Purpose:** Number of lines to skip at the end of the file (integer). Requires `engine='python'`.
-   **Example:**
    ```python
    csv_with_footer = "colX,colY\n10,20\n30,40\nThis is a footer\nAnother footer line"
    # df_skip_foot = pd.read_csv(StringIO(csv_with_footer), skipfooter=2, engine='python')
    # print(df_skip_foot) # Will show first two data rows
    ```

### `nrows`
-   **Purpose:** Number of rows of the file to read. Useful for reading a small sample of a large file.
-   **Example:**
    ```python
    # Assuming 'large_file.csv' is very big
    # df_sample = pd.read_csv('large_file.csv', nrows=100) # Reads only the first 100 data rows
    # print(df_sample.shape)
    ```

---

## ⚙️ Performance & Other

### `chunksize`
-   **Purpose:** If specified, returns a `TextFileReader` object for iterating over chunks of the file. Useful for processing large files that don't fit in memory.
-   **Example:**
    ```python
    # Assuming 'very_large_file.csv' exists
    # chunk_iter = pd.read_csv('very_large_file.csv', chunksize=10000)
    # for chunk_df in chunk_iter:
    #     # Process each chunk_df here
    #     print(f"Processing chunk with shape: {chunk_df.shape}")
    ```

### `encoding`
-   **Purpose:** Specifies the file encoding (e.g., 'utf-8', 'latin1', 'iso-8859-1').
-   **Example:**
    ```python
    # df_encoded = pd.read_csv('data_with_special_chars.csv', encoding='latin1')
    ```

### `on_bad_lines`
-   **Purpose:** Specifies what to do with lines containing too many fields.
    -   `'error'`: Raise an exception (default).
    -   `'warn'`: Issue a warning and skip the bad line.
    -   `'skip'`: Skip bad lines without warning.
-   **Example:**
    ```python
    csv_bad_lines_data = "a,b,c\n1,2,3\n4,5,6,EXTRA_VALUE\n7,8,9" # Line 2 has an extra value
    df_skip_bad = pd.read_csv(StringIO(csv_bad_lines_data), on_bad_lines='skip')
    print("DataFrame with bad lines skipped:")
    print(df_skip_bad)
    # Expected output:
    # DataFrame with bad lines skipped:
    #    a  b  c
    # 0  1  2  3
    # 1  7  8  9
    ```

---
This is not an exhaustive list, but covers many common use cases.
Refer to the [official pandas documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html) for all parameters and more details.
