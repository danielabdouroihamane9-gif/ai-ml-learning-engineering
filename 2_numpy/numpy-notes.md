# Efficient Data Manipulation with NumPy

> Study notes based on the *Efficient Data Manipulation Using NumPy* lecture (Matsuo-Iwasawa Lab, University of Tokyo GCI), with practical clarifications and a few useful extensions.

## 1. Why NumPy?

**Data mining** is the process of finding useful knowledge or patterns in data. In a data-science workflow such as CRISP-DM, data preparation is an important step, and NumPy provides efficient tools for working with numerical data.

NumPy is a Python library for scientific computing. Its central structure is the **`ndarray`** (N-dimensional array), which supports:

- Efficient storage of numerical arrays, including multidimensional arrays.
- Vectorized, element-wise operations (ufuncs) without writing an explicit Python loop.
- Common mathematical, statistical, and array-manipulation functions.

Import it using the usual alias:

```python
import numpy as np
```

### Python lists versus NumPy arrays

- A Python list can contain different kinds of values. A typical numerical NumPy array has one common `dtype` (data type), which helps make numerical operations efficient. NumPy can also create `dtype=object` arrays, but these are not the usual choice for numerical work.
- `+` on lists concatenates them; `+` on compatible NumPy arrays adds corresponding elements.
- NumPy arrays naturally represent multidimensional data and support vectorized operations.

```python
values = np.array([1, 2, 3])
```

## 2. Python refresher

The lecture briefly reviews Python variables and assignment, arithmetic and comparison operators, and basic collections:

- **Lists** use zero-based indexing. Nested lists can represent rows of a table.
- **Tuples** are ordered collections; **dictionaries** associate keys with values.
- A `for` loop can iterate over a collection; `range(...)` generates integer sequences.

NumPy becomes especially useful when many numerical operations must be applied to all values in a collection.

## 3. One-dimensional arrays and element-wise operations

### Creating arrays

```python
a = np.array([1, 2, 3])
b = np.array([10, 20, 30])
```

### Universal functions (ufuncs)

A ufunc applies an operation element by element at corresponding positions. For arrays of compatible shape:

| Operator | NumPy function | Example meaning |
| --- | --- | --- |
| `a + b` | `np.add(a, b)` | Add corresponding elements |
| `a - b` | `np.subtract(a, b)` | Subtract corresponding elements |
| `a * b` | `np.multiply(a, b)` | Multiply corresponding elements |
| `a / b` | `np.divide(a, b)` | Divide corresponding elements |

```python
a + b  # array([11, 22, 33])
a * b  # array([10, 40, 90])
```

**Important:** `*` is element-wise multiplication, not matrix multiplication. For matrix multiplication, use `@` or `np.matmul` (see section 10).

Other common ufuncs include `np.exp`, `np.log`, `np.sin`, and `np.cos`. Trigonometric functions expect angles in **radians**.

### Element-wise comparisons

Comparisons return Boolean arrays:

```python
a == 2              # array([False, True, False])
a > 1               # array([False, True, True])
np.equal(a, 2)
np.greater(a, 1)
np.less(a, 3)
```

This differs from comparing whole Python lists: NumPy comparisons are vectorized across elements.

## 4. Broadcasting

**Broadcasting** lets NumPy perform element-wise operations on arrays with different, but compatible, shapes—often without explicitly repeating the smaller array's data.

Rules for two arrays:

1. Compare their shapes starting from the **rightmost** dimensions.
2. Each pair of dimensions must be equal, or one of them must be `1`.
3. Missing dimensions are treated as if they were `1`.
4. If a pair does not meet the rule, the operation raises a shape-mismatch error.

A dimension of size `1` is conceptually expanded to match the other dimension.

```python
x = np.array([[1], [2], [3]])  # shape (3, 1)
y = np.array([10, 20])         # shape (2,)
x + y                          # shape (3, 2)
# array([[11, 21],
#        [12, 22],
#        [13, 23]])
```

The shapes align as `(3, 1)` and `(1, 2)`, producing `(3, 2)`. Broadcasting is a shape rule, not a promise that every combination of arrays can be combined.

## 5. Aggregations and summary statistics

Aggregation functions reduce an array to one or more summary values:

```python
x = np.array([1, 2, 3])
np.max(x)   # 3
np.min(x)   # 1
np.sum(x)   # 6
np.mean(x)  # 2.0
np.std(x)   # about 0.8165
```

NumPy's `np.std` defaults to the **population** standard deviation (`ddof=0`). For the sample standard deviation, use `np.std(x, ddof=1)` when appropriate.

For multidimensional arrays, the `axis` argument determines which dimension is reduced; see section 7.

## 6. One-dimensional indexing and slicing

NumPy indexing is zero-based. Negative indexes count from the end:

```python
a = np.array([10, 20, 30, 40, 50])
a[0]    # 10
a[-1]   # 50

### Slices

The general form is `a[start:stop:step]`:

- `start` is included.
- `stop` is excluded.
- Omitted `start` or `stop` defaults to the beginning or end.
- Omitted `step` defaults to `1`.

```python
a[1:4]   # array([20, 30, 40])
a[::2]   # array([10, 30, 50])
a[::-1]  # reversed order
```

### Lecture example: wind speed

The lecture records wind speed every 30 minutes, beginning at 12:00:

```python
wind = np.array([12, 13, 15, 14, 9, 11, 10, 13, 9, 7, 8, -3, -5, -11, -9, -12, -8, 0, 1])
```

- 13:00 is index `2`, so `wind[2]` is `15`.
- From 13:00 up to, but not including, 16:00: `wind[2:8]` gives `[15, 14, 9, 11, 10, 13]`.
- Every 60 minutes (every other 30-minute sample): `wind[::2]` gives `[12, 15, 9, 10, 9, 8, -5, -9, -8, 1]`.

### Selecting several positions

Integer-array (fancy) indexing selects the requested positions:

```python
a = np.array([10, 20, 30, 40, 50])
a[[1, 4]]  # array([20, 50])
```

## 7. Two-dimensional arrays and `axis`

A 2D array can be constructed from nested lists:

```python
scores = np.array([
    [75, 60, 82, 80],
    [90, 73, 75, 85],
    [82, 92, 80, 63],
])
```

Element-wise ufuncs work across corresponding cells, subject to broadcasting rules.

### Understanding axis reductions

For a 2D array:

- `axis=0` collapses the **row dimension** and returns one result **per column** (down each column).
- `axis=1` collapses the **column dimension** and returns one result **per row** (across each row).
- Omitting `axis` generally reduces the whole array to one result.

```python
np.max(scores)             # 92
np.max(scores, axis=0)     # array([90, 92, 82, 85]) -- maximum of each column
np.max(scores, axis=1)     # array([82, 90, 92])     -- maximum of each row
np.mean(scores, axis=0)    # mean of each column
np.sum(scores, axis=1)     # sum of each row
```

A reliable way to interpret an axis reduction is: **which dimension disappears, and what is one result per?**

## 8. Two-dimensional indexing

Use `a[row, column]` to select an individual element:

```python
a = np.arange(1, 13).reshape(3, 4)
# array([[ 1,  2,  3,  4],
#        [ 5,  6,  7,  8],
#        [ 9, 10, 11, 12]])

a[0, 1]       # 2
```

Useful forms:

```python
a[0]          # first row; equivalent to a[0, :]
a[:, 1]       # second column
a[:2, 1:]     # first two rows, columns 1 onward
# array([[2, 3, 4],
#        [6, 7, 8]])
```

A colon (`:`) means “all entries along this dimension”; slices keep the selected range.

### Advanced indexing: paired coordinates

When integer index arrays are supplied for multiple axes, NumPy pairs their entries element by element:

```python
a[[0, 2], [1, 3]]  # array([2, 12])
```

This selects `(row 0, col 1)` and `(row 2, col 3)`—it does **not** select every combination of the rows and columns.

```python
a[0, [1, 3]]       # array([2, 4])
```

To select the Cartesian combination of selected rows and columns, use `np.ix_`:

```python
a[np.ix_([0, 2], [1, 3])]
# array([[ 2,  4],
#        [10, 12]])
```

You can combine an integer-array selection with a slice:

```python
a[[0, 2], 1:]
# array([[ 2,  3,  4],
#        [10, 11, 12]])
```

### Boolean indexing

A comparison can create a Boolean mask, which can then select matching elements:

```python
mask = (a % 3 == 0)
a[mask]  # array([3, 6, 9, 12])
```

For a multidimensional array, Boolean indexing returns the selected values as a one-dimensional result in traversal order. Combine multiple Boolean conditions with `&` and `|`, and put parentheses around each comparison:

```python
a[(a > 3) & (a < 10)]
```

Use `np.logical_and` / `np.logical_or` if preferred. Python's `and` and `or` do not perform element-wise array logic.

## 9. Creating and reshaping arrays

### `zeros`, `ones`, and `arange`

```python
np.zeros((2, 3))       # 2-by-3 array of zeros
np.ones((2, 3))        # 2-by-3 array of ones
np.arange(8)           # 0 through 7
np.arange(2, 8)        # 2 through 7
np.arange(2, 8, 2)     # 2, 4, 6
```

`np.arange` uses an **exclusive** stop, as with slicing. `zeros` and `ones` default to floating-point values; specify `dtype` when another type is wanted.

### Shape and reshape

```python
x = np.arange(12)
x.shape                 # (12,)
x.reshape(3, 4)         # 3 rows, 4 columns
x.reshape(2, -1)        # infer the second dimension: shape (2, 6)
```

A reshape must preserve the total number of elements. Only one dimension can be inferred using `-1`.

## 10. Practical clarifications and common pitfalls

### Inspecting arrays

```python
x.shape    # dimensions, e.g. (3, 4)
x.ndim     # number of dimensions
x.size     # total number of elements
x.dtype    # element data type
x.itemsize # bytes used by one element
```

The lecture also demonstrates `int64` as an example `dtype`. Use `astype` to explicitly convert values:

```python
x_float = x.astype(np.float64)
```

### Element-wise multiplication versus matrix multiplication

```python
A * B       # element-wise multiplication (shapes must be compatible)
A @ B       # matrix multiplication
np.matmul(A, B)
```

For 2D arrays, matrix multiplication requires the inner dimensions to match: `(m, n) @ (n, p)` produces shape `(m, p)`.

### Views and copies

- Basic slicing (such as `a[1:4]`) usually returns a **view** into the original array. Changing the slice may change the original.
- Advanced indexing (integer-array or Boolean indexing) generally returns a **copy**.
- If you need an independent array, use `.copy()` explicitly.

Do not rely on an indexing result being independent unless you know which kind of indexing produced it.

### Floating-point and missing/invalid values

- Floating-point calculations are approximate; values such as `0.1` may not be represented exactly.
- Integer arrays cannot represent fractional values. Be deliberate about `dtype` when doing calculations that need fractions.
- Floating-point division by zero can produce `inf` or `nan` and a warning rather than a Python `ZeroDivisionError`.
- `np.nan` marks a missing/invalid floating-point value. Functions such as `np.nanmean` and `np.nanmax` ignore NaNs, unlike their ordinary counterparts. Check the data and choose a missing-value policy intentionally.

### Keeping dimensions after a reduction

`keepdims=True` retains reduced axes with length 1, which can make later broadcasting easier:

```python
row_means = np.mean(scores, axis=1, keepdims=True)  # shape (3, 1)
centered = scores - row_means
```

## 11. Short practice checklist

Before moving on, make sure you can:

1. Create an array with `np.array` and inspect its `shape`, `ndim`, and `dtype`.
2. Predict element-wise arithmetic and comparison results.
3. Check whether two shapes are broadcast-compatible.
4. Use `sum`, `mean`, `max`, and `std`, including along a chosen axis.
5. Translate a time range into a slice, remembering that the stop index is excluded.
6. Distinguish paired advanced indexing from Cartesian row/column selection.
7. Use a Boolean mask to filter array values.
8. Tell when `*` is appropriate and when matrix multiplication (`@`) is intended.

## 12. Scope of the lecture

The material focuses on NumPy fundamentals for **one- and two-dimensional arrays**: ufuncs, broadcasting, aggregations, axes, indexing, and basic creation/reshaping. Higher-dimensional array techniques, advanced linear algebra, and complex machine-learning applications are outside the lecture's main scope.
