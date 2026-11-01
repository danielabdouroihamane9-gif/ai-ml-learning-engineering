# Cleaning Data Using Pandas

> Study notes for the Matsuo-Iwasawa Lab UTokyo GCI lecture *Cleaning Data Using Pandas*. These notes collect the slide topics and add practical context for using Pandas in a VS Code project.

## 1. Where Pandas fits in data science

A common data-science workflow is **Obtain → Scrub → Explore → Model → Interpret**. This lecture focuses on *Scrub*: cleaning, reshaping, combining, and preparing data so it can be explored and modeled reliably. Data preparation often takes a substantial share of a project.

The lecture focuses on **structured, tabular data**, such as CSV/Excel files and relational-database tables. Pandas can also work with time series and text columns. It is useful for data preparation and analysis, though large datasets can be limited by memory use and some operations may be slower than array-oriented or database tools.

## 2. Pandas' core data structures

```python
import pandas as pd
import numpy as np
```

### Series

A **Series** is a one-dimensional labeled array: it has values and an index.

```python
population = pd.Series([100, 200, 150], index=["A", "B", "C"])
```

### DataFrame

A **DataFrame** is a two-dimensional labeled table. Its row labels are the **index** and its column labels are the **columns**. Columns can have different data types—for example, a numeric ID, text city, and numeric birth year. Modern Pandas DataFrames are not necessarily one single NumPy array internally.

```python
people = pd.DataFrame({
    "ID": [100, 101, 102],
    "City": ["Tokyo", "Osaka", "Kyoto"],
    "Birth_year": [1990, 1989, 1992],
    "Name": ["Hiroshi", "Akiko", "Yuki"],
})
```

Useful first checks:

```python
people.head()       # first rows
people.shape        # (number_of_rows, number_of_columns)
people.columns      # column labels
people.index        # row labels
people.dtypes       # dtype of each column
people.info()       # dtypes and non-null counts
people.describe()   # summary statistics for numeric columns by default
```

## 3. Read and write data

The slides note that Pandas supports many formats, including CSV, Excel, and JSON. Common entry points:

```python
df = pd.read_csv("data.csv")
df = pd.read_excel("data.xlsx", sheet_name="Sheet1")
df = pd.read_json("data.json")

# Save a cleaned result; index=False avoids adding the DataFrame index as a CSV column.
df.to_csv("cleaned.csv", index=False)
```

Check import assumptions: use `dtype=`, `parse_dates=`, `encoding=`, or `na_values=` when appropriate. Inspect the result rather than assuming the file was interpreted correctly.

## 4. Selecting and extracting data

Choosing rows and columns depends on whether you mean **labels** or **integer positions**.

| Accessor | Selects by | Typical use |
|---|---|---|
| `.loc[row_labels, column_labels]` | Labels | Human-readable index/column names |
| `.iloc[row_positions, column_positions]` | Zero-based integer positions | Positional selection |
| `.at[row_label, column_label]` | Labels | One scalar value |
| `.iat[row_position, column_position]` | Integer positions | One scalar value |

Suppose the index is `0, 1, 2` and columns include `ID`, `City`, `Birth_year`, and `Name`:

```python
df.loc[1:2, ["ID", "Birth_year"]]  # labels 1 and 2; .loc label slice includes the stop

df.iloc[0:2, [0, 2]]                # row positions 0 and 1; .iloc slice excludes the stop

df.at[2, "Birth_year"]             # scalar by label
df.iat[2, 2]                        # scalar by position
```

**Remember:** `.loc` is label-based, and a label slice includes both endpoints when present. `.iloc` is position-based, and slices follow normal Python's stop-exclusive rule. The DataFrame index is not necessarily the same as row positions.

### Select columns and rows

```python
df["City"]                    # one column as a Series
df[["City", "Name"]]         # multiple columns as a DataFrame
df.loc[0]                     # row with index label 0 as a Series
df.iloc[0:3]                  # first three row positions
```

### Filter with conditions

Comparisons produce Boolean masks. Use `&` for AND and `|` for OR, with parentheses around each condition. Do not use Python's `and` / `or` for element-wise Pandas conditions.

```python
tokyo = df.loc[df["City"] == "Tokyo"]
selected = df.loc[(df["Birth_year"] >= 1990) & (df["City"] != "Osaka")]
```

### Assign values safely

```python
df["Age_estimate"] = 2025 - df["Birth_year"]
df.loc[df["City"] == "Tokyo", "Region"] = "Kanto"
df = df.assign(Is_Tokyo=df["City"].eq("Tokyo"))
```

Prefer assigning through the original DataFrame with `.loc` rather than chained indexing such as `df[condition]["column"] = value`; chained assignment may not update the intended object and can raise warnings.

## 5. Sorting values or labels

```python
df.sort_values("Birth_year")
df.sort_values("Birth_year", ascending=False)
df.sort_values(["City", "Birth_year"], ascending=[True, False])
df.sort_index()  # sort by row-index labels
```

Sorting usually returns a new DataFrame; assign the result if you want to keep it:

```python
df = df.sort_values("Birth_year")
```

For a stable order among tied values, use `kind="stable"` when stability matters.

## 6. Combining tables with `merge`

`pd.merge` combines columns from two DataFrames by matching **key values** (a database-style join). The default is an **inner join**: only key values present on both sides are retained.

```python
people = pd.DataFrame({
    "id": [100, 101, 102, 103],
    "city": ["Tokyo", "Osaka", "Kyoto", "Sapporo"],
})
scores = pd.DataFrame({
    "id": [100, 101, 102, 105],
    "math": [50, 43, 33, 76],
})

pd.merge(people, scores, on="id")  # inner join: ids 100, 101, 102
```

### Join types

| `how=` | What is retained |
|---|---|
| `"inner"` | Keys found in both tables (the default) |
| `"left"` | All keys/rows from the left table; unmatched right-side fields become missing |
| `"right"` | All keys/rows from the right table |
| `"outer"` | All keys from either table; unmatched fields become missing |

```python
pd.merge(people, scores, on="id", how="left")
pd.merge(people, scores, on="id", how="outer")
```

Specify the key explicitly whenever possible. The lecture lists `on`, `left_on`, `right_on`, `left_index`, and `right_index`:

```python
# Same key name in both tables
pd.merge(left, right, on="id", how="left")

# Different key names
pd.merge(left, right, left_on="person_id", right_on="id", how="inner")

# Use index labels as keys
pd.merge(left, right, left_index=True, right_index=True)
```

If no key is specified, Pandas uses the column names shared by both inputs. This can accidentally join on more columns than intended—prefer `on=` to make the match rule clear. If both tables contain non-key columns with the same name, Pandas adds suffixes (default `_x` and `_y`); customize with `suffixes=("_left", "_right")`.

### Important merge checks

- **Duplicate keys can multiply rows.** If one key appears twice on the left and three times on the right, that key produces six matched rows. This is a many-to-many join, not necessarily a bug, but check whether it is intended.
- Use `validate="one_to_one"`, `"one_to_many"`, or `"many_to_one"` to assert an expected key relationship.
- Use `indicator=True` to see whether each output row matched on the left, right, or both.
- Pandas can match null merge keys to other null merge keys, unlike typical SQL behavior. Clean or explicitly handle null keys if that matters.
- Check row counts and key uniqueness before and after merges to catch accidental row expansion or loss.

```python
joined = pd.merge(
    people,
    scores,
    on="id",
    how="left",
    validate="one_to_one",
    indicator=True,
)
```

## 7. Stacking tables with `concat`

Use `pd.concat` to append or align objects. Unlike `merge`, concatenation does not match records using a key column.

### Vertical concatenation: add rows

```python
combined = pd.concat([df1, df2], axis=0)
```

This stacks rows. Columns are aligned by **column labels**; with the default outer behavior, the result includes the union of columns. Where a source lacks a column, the result has missing values (`NaN`/`pd.NA`). The row indexes are preserved by default, so duplicate index labels can occur.

```python
combined = pd.concat([df1, df2], ignore_index=True)  # create a fresh 0..n-1 index
```

Use `join="inner"` to keep only shared columns. `sort=True` sorts the non-concatenation axis when relevant; it does not sort the row values.

### Horizontal concatenation: add columns

```python
wide = pd.concat([df1, df2], axis=1)
```

With `axis=1`, columns are placed side by side and **aligned by index label**, not necessarily by row position. Nonmatching index labels create missing values. If you mean to match records using an ID, use `merge(..., on="id")` instead.

### Quick rule

- `merge`: match records using key column(s) or index.
- `concat(axis=0)`: stack rows; align columns by name.
- `concat(axis=1)`: place columns side by side; align rows by index label.

## 8. Removing rows or columns

Use `.drop` and explicitly state whether labels refer to rows or columns:

```python
df = df.drop(index=[2, 4])
df = df.drop(columns=["temporary_column"])
```

Other common choices:

```python
df = df.drop_duplicates()
df = df.drop_duplicates(subset=["id"], keep="first")
df = df.loc[df["Age"].notna()]  # keep rows with a non-missing Age
```

Removing duplicates is a data decision: first decide which columns define a duplicate and which record to keep.

## 9. Missing values

Missing data can appear after a merge/concat or be present in the source file. Pandas uses several missing markers, including `np.nan`, `pd.NA`, and `NaT` for datetime-like data.

### Detect and count missing values

```python
df.isna()                 # Boolean mask

df.isna().sum()           # number missing per column
df.isna().mean()          # fraction missing per column
df["City"].notna()        # non-missing values
```

Do not test missing values using `== np.nan`; use `.isna()` / `.notna()`.

### Drop or fill missing values

```python
df.dropna()                         # drop rows with any missing value

df.dropna(subset=["Age"])          # drop rows missing Age

df.dropna(axis=1, how="all")       # drop columns that are entirely missing

df["Age"] = df["Age"].fillna(df["Age"].median())
df["City"] = df["City"].fillna("Unknown")
```

Choose a treatment based on what missingness means. Dropping rows can bias a dataset; filling with zero can incorrectly imply a real zero. For time-ordered data, methods such as forward fill (`ffill`) or interpolation may be suitable only when justified by the context.

## 10. Aggregation and `groupby`

`groupby` follows a **split → apply → combine** pattern: split rows into groups, calculate something per group, and combine the results.

```python
sales = pd.DataFrame({
    "store": ["A", "A", "A", "B"],
    "city": ["Osaka", "Osaka", "Tokyo", "Osaka"],
    "product": ["P1", "P2", "P1", "P1"],
    "amount": [200, 100, 500, 800],
})

sales.groupby("store")["amount"].sum()
sales.groupby(["store", "city"])["amount"].agg(["sum", "mean", "count"])
```

Named aggregations make output columns clear:

```python
summary = sales.groupby("store", as_index=False).agg(
    total_sales=("amount", "sum"),
    average_sale=("amount", "mean"),
    records=("amount", "count"),
)
```

Related patterns:

- `.agg(...)`: produce one or more summary values per group.
- `.transform(...)`: return values aligned to the original rows, e.g. group means for each row.
- `.filter(...)`: keep or remove whole groups based on a condition.

By default, groups with missing group keys are excluded (`dropna=True`). If those rows should form a group, use `dropna=False` and check the result.

## 11. Binning numeric data: `cut` and `qcut`

Binning converts continuous numeric values into intervals/categories.

- `pd.cut(..., bins=5)` makes **equal-width intervals** spanning the data range.
- `pd.qcut(..., q=5)` makes **quantile bins** intended to contain roughly equal numbers of observations.

```python
rng = np.random.default_rng(42)
data = pd.DataFrame({"value": rng.normal(loc=50, scale=10, size=100)})

data["equal_width_bin"] = pd.cut(data["value"], bins=5)
data["equal_count_bin"] = pd.qcut(data["value"], q=5)
```

The exact values in the lecture's random sample will vary unless a random seed is set. Tied values can make equal-frequency bin edges impossible; `pd.qcut(..., duplicates="drop")` can reduce the number of bins. Binning creates categories; it does not automatically make bins equally meaningful for a particular analysis.

## 12. Hierarchical indexes (MultiIndex)

A **MultiIndex** has multiple levels on rows or columns. It can organize related dimensions and make filtering and aggregation easier without changing the underlying facts.

```python
store_sales = pd.DataFrame({
    "Store": ["A", "A", "A", "A", "A", "A", "B"],
    "Place": ["Osaka", "Osaka", "Osaka", "Tokyo", "Tokyo", "Tokyo", "Osaka"],
    "Product": ["Product 1", "Product 1", "Product 2", "Product 1", "Product 1", "Product 2", "Product 1"],
    "Color": ["Red", "Blue", "Red", "Red", "Blue", "Red", "Red"],
    "Sales": [200, 0, 100, 500, 300, 400, 800],
})

hierarchical = store_sales.pivot_table(
    index=["Store", "Place"],
    columns=["Product", "Color"],
    values="Sales",
    aggfunc="sum",
)
```

Here the row index has levels `Store` and `Place`, and the columns have levels `Product` and `Color`. The lecture's example uses this layout to make store/place and product/color comparisons easier. A blank cell means no matching record for that combination; it is not automatically the same thing as a recorded sales value of zero.

Useful MultiIndex tools:

```python
hierarchical.index.names
hierarchical.columns.names
hierarchical.loc[("A", "Osaka")]  # select a row by both index levels
hierarchical.xs("Osaka", level="Place")  # cross-section by one level
hierarchical.reset_index()       # turn index levels back into columns
```

Use hierarchical indexes when the structure helps analysis; reset them when a flat table is easier to work with or export.

## 13. A practical cleaning workflow

A safe first pass on a new table:

1. **Load** the file and inspect `head`, `shape`, `info`, and `dtypes`.
2. **Check labels and keys:** inspect duplicate rows, duplicate IDs, unexpected spaces, and inconsistent categories.
3. **Check missingness:** count missing values and decide how each column should be handled.
4. **Select and correct:** filter rows, select columns, convert types, and assign cleaned values deliberately.
5. **Combine:** choose `merge` when matching keys, `concat` when stacking/alignment is intended; check resulting row counts.
6. **Summarize:** use `groupby`, aggregation, or binning to inspect patterns.
7. **Validate and save:** compare dimensions, key counts, and representative records before exporting.

Keep the raw input unchanged where possible, and save cleaned output to a separate file. Record assumptions (for example, why a missing value was filled) so the cleaning is reproducible.

## 14. Common mistakes to avoid

- Confusing `.loc` labels with `.iloc` positions.
- Forgetting that `.loc` label slices include the stop, while `.iloc` slices exclude it.
- Using `and` / `or` instead of `&` / `|` for Series conditions, or omitting parentheses.
- Assuming `merge` uses the intended key automatically; specify `on=`.
- Not checking duplicate keys, which can multiply joined rows.
- Using `concat(axis=1)` when you intended a key-based join; it aligns by index.
- Assuming concatenated inputs get a fresh index; use `ignore_index=True` if desired.
- Treating missing values as zero or dropping them without considering their meaning.
- Using chained assignment and then wondering why the source table did not change.
- Assuming `qcut` always produces exactly the requested number of bins when many values are tied.

## 15. Lecture scope and what to learn next

The slides introduce DataFrame selection/extraction, assignment, sorting, merging, deletion, aggregation/group operations, missing values, binning, and hierarchical indexing. They emphasize that the goal is a brief practical overview—not memorizing every method.

The most important concepts to retain are:

- **Series versus DataFrame**, and the roles of index and columns.
- **Label-based versus position-based** selection (`loc`/`at` versus `iloc`/`iat`).
- **Join by key versus concatenation by axis/index** (`merge` versus `concat`).
- How missing values arise and how to inspect them before deciding what to do.
- How `groupby` and MultiIndex support grouped summaries.
- **Equal-width versus equal-frequency bins** (`cut` versus `qcut`).

As you continue, practice type conversion, date/time parsing, string cleanup, duplicate handling, reshaping (`pivot`, `melt`), and robust validation of joins and cleaning assumptions.
