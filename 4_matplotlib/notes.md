
# Matplotlib — Data Visualization Notes

> **Course:** Matsuo Lab — GCI World 2026
> **Topic:** Visualizing Data Using Matplotlib
> **Purpose:** Learn to visualize, explore, interpret, and communicate information from datasets using Python.

---

## 1. Introduction to Data Visualization

### 1.1 What is Matplotlib?

Matplotlib is a Python library used to create static, animated, and interactive visualizations.

It allows us to represent numerical and categorical data through graphs such as:

- Line plots
- Scatter plots
- Histograms
- Bar charts
- Pie charts
- Box plots
- Heatmaps

The main module used for everyday plotting is `matplotlib.pyplot`, commonly imported as `plt`.

```python
import matplotlib.pyplot as plt
```

### 1.2 Why is data visualization important?

Data visualization helps us:

1. Understand patterns and trends.
2. Identify unusual observations and outliers.
3. Compare different groups.
4. Discover relationships between variables.
5. Communicate analytical findings.
6. Support evidence-based decisions.

A dataset may contain thousands of records. A well-designed graph can reveal patterns that are difficult to recognize from raw numbers.

### 1.3 Data Science Process

Visualization is particularly important during Exploratory Data Analysis (EDA).

A typical data science workflow:

1. **Obtain:** Collect data from relevant sources.
2. **Scrub:** Clean and prepare the data.
3. **Explore:** Identify patterns, trends, and relationships.
4. **Model:** Build predictive or statistical models.
5. **Interpret:** Explain and communicate the results.

Visualization is primarily used during exploration, but it is also valuable when evaluating models and communicating final results.

---

## 2. Exploratory Data Analysis (EDA)

### 2.1 What is EDA?

Exploratory Data Analysis is an iterative process of examining and visualizing data to understand its structure, characteristics, and relationships.

EDA helps us:

- Understand the distribution of variables.
- Identify missing or unusual values.
- Detect outliers.
- Discover correlations.
- Identify possible biases.
- Form and investigate hypotheses.
- Determine whether data is suitable for modelling.

EDA is not simply creating attractive graphs. It is an analytical process.

### 2.2 Examples

#### Example 1: Advertising budget vs. sales

A scatter plot can help investigate whether increased advertising expenditure is associated with increased sales.

#### Example 2: Age distribution

A histogram can show whether a dataset contains more observations in particular age ranges.

### 2.3 Questions to ask during EDA

When exploring a dataset, ask:

- How many observations are present?
- What variables are available?
- What are the minimum and maximum values?
- Are there missing values?
- Are there duplicate records?
- How are numerical variables distributed?
- Are there extreme observations?
- Do two variables appear to be related?
- Are some categories more frequent than others?

---

## 3. Understanding Matplotlib's Architecture

Matplotlib has three important components.

### 3.1 Figure

A **Figure** is the complete container or canvas that holds the visualization.

It can contain one or multiple plotting areas.

### 3.2 Axes

An **Axes** is an individual plotting area where data is displayed.

An Axes contains elements such as:

- Data points and lines.
- X-axis and Y-axis.
- Axis labels.
- Title.
- Legend.
- Grid lines.

A single Figure can contain multiple Axes.

### 3.3 Axis

An **Axis** represents an individual coordinate axis, such as the X-axis or Y-axis.

It manages:

- Coordinate scales.
- Tick positions.
- Tick labels.
- Axis limits.

### 3.4 Relationship

```text
Figure
│
├── Axes 1
│   ├── X-axis
│   └── Y-axis
│
└── Axes 2
    ├── X-axis
    └── Y-axis
```

**Important distinction:**

- Figure = the complete canvas.
- Axes = an individual plotting area.
- Axis = an individual coordinate axis.

---

## 4. Importing Matplotlib

The standard import:

```python
import matplotlib.pyplot as plt
```

`pyplot` provides a convenient interface for creating graphs and is particularly useful for experimentation and learning.

For more structured visualizations, Matplotlib also provides an object-oriented interface.

### 4.1 Two approaches to plotting

#### Approach A: Pyplot interface

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [10, 20, 15, 30, 25]

plt.plot(x, y)

plt.title("Sales Over Time")
plt.xlabel("Time")
plt.ylabel("Sales")

plt.show()
```

#### Approach B: Object-oriented interface

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [10, 20, 15, 30, 25]

fig, ax = plt.subplots()

ax.plot(x, y)

ax.set_title("Sales Over Time")
ax.set_xlabel("Time")
ax.set_ylabel("Sales")

plt.show()
```

The object-oriented approach is particularly useful when creating multiple plots or building reusable visualization code.

**Recommendation:** Learn both approaches, but gradually prefer `fig, ax = plt.subplots()` for more structured projects.

---

## 5. Creating Your First Graph

A basic visualization requires:

1. Preparing the data.
2. Selecting an appropriate graph.
3. Passing the data to the plotting function.
4. Displaying the graph.

### 5.1 Basic line plot

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [10, 20, 15, 30, 25]

plt.plot(x, y)

plt.show()
```

### 5.2 Understanding the commands

| Function           | Purpose                   |
| ------------------ | ------------------------- |
| `plt.plot()`       | Creates a line plot       |
| `plt.scatter()`    | Creates a scatter plot    |
| `plt.show()`       | Displays the graph        |
| `plt.title()`      | Adds a title              |
| `plt.xlabel()`     | Labels the X-axis         |
| `plt.ylabel()`     | Labels the Y-axis         |
| `plt.figure()`     | Creates a Figure          |
| `plt.subplots()`   | Creates a Figure and Axes |

---

## 6. Customizing Graphs

A graph should be understandable without requiring the reader to inspect the source code.

### 6.1 Changing figure size

Use `figsize` to control the dimensions of the Figure.

```python
plt.figure(figsize=(10, 5))

plt.plot([1, 2, 3, 4], [10, 20, 15, 30])

plt.show()
```

The format is:

```python
plt.figure(figsize=(width, height))
```

The dimensions are measured in inches.

### 6.2 Adding a title

```python
plt.title("Monthly Revenue")
```

### 6.3 Adding axis labels

```python
plt.xlabel("Month")
plt.ylabel("Revenue")
```

### 6.4 Changing colors and line styles

```python
plt.plot(
    [1, 2, 3, 4],
    [10, 20, 15, 30],
    color="blue",
    linestyle="--",
    linewidth=2
)
```

Common line styles:

| Style | Meaning |
| -------- | ------------- |
| `"-"` | Solid line |
| `"--"` | Dashed line |
| `":"` | Dotted line |
| `"-."` | Dash-dot line |

### 6.5 Changing markers

Markers represent individual observations.

```python
plt.plot(
    [1, 2, 3, 4],
    [10, 20, 15, 30],
    marker="o"
)
```

Common markers:

| Marker | Meaning |
| ------- | ----------- |
| `"o"` | Circle |
| `"s"` | Square |
| `"^"` | Triangle |
| `"x"` | X marker |
| `"+"` | Plus marker |
| `"*"` | Star |

Official marker reference:

<https://matplotlib.org/stable/api/markers_api.html>

### 6.6 Adding a legend

A legend identifies different data series.

```python
plt.plot(
    [1, 2, 3],
    [10, 20, 30],
    label="Product A"
)

plt.plot(
    [1, 2, 3],
    [15, 25, 35],
    label="Product B"
)

plt.legend()
plt.show()
```

The `label` argument defines the name displayed in the legend.

### 6.7 Adding grid lines

```python
plt.grid(True)
```

Grid lines help readers estimate values from the graph.

### 6.8 Changing font size

```python
plt.title("Monthly Revenue", fontsize=16)
plt.xlabel("Month", fontsize=12)
plt.ylabel("Revenue", fontsize=12)
```

### 6.9 Controlling axis limits

Use `xlim()` and `ylim()` to control the displayed coordinate ranges.

```python
plt.xlim(0, 12)
plt.ylim(0, 100)
```

This is useful when you want to focus on a particular region of the data.

---

## 7. Creating Multiple Graphs

A single Figure can contain several Axes.

This is useful when comparing different variables or visualizations.

### 7.1 Using subplot()

Syntax:

```python
plt.subplot(rows, columns, position)
```

Example:

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))

# First graph
plt.subplot(2, 1, 1)
plt.plot([1, 2, 3], [10, 20, 30])
plt.title("Graph 1")

# Second graph
plt.subplot(2, 1, 2)
plt.plot([1, 2, 3], [30, 20, 10])
plt.title("Graph 2")

plt.tight_layout()
plt.show()
```

Explanation:

- `2` = number of rows.
- `1` = number of columns.
- `1` = position of the first graph.
- `2` = position of the second graph.

For a 2-row, 3-column layout:

```python
plt.subplot(2, 3, 1)
plt.subplot(2, 3, 2)
plt.subplot(2, 3, 3)
plt.subplot(2, 3, 4)
plt.subplot(2, 3, 5)
plt.subplot(2, 3, 6)
```

### 7.2 Recommended approach: plt.subplots()

```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(2, 2, figsize=(12, 8))

axes[0, 0].plot([1, 2, 3], [10, 20, 30])
axes[0, 0].set_title("Line Plot")

axes[0, 1].scatter([1, 2, 3], [10, 20, 30])
axes[0, 1].set_title("Scatter Plot")

axes[1, 0].bar(["A", "B", "C"], [10, 20, 30])
axes[1, 0].set_title("Bar Chart")

axes[1, 1].hist([10, 20, 20, 30, 30, 30])
axes[1, 1].set_title("Histogram")

plt.tight_layout()
plt.show()
```

This creates four plots in one Figure.

**Additional concept:**

- `axes[0, 0]` refers to the first row and first column.
- `axes[0, 1]` refers to the first row and second column.
- `axes[1, 0]` refers to the second row and first column.

`plt.tight_layout()` automatically adjusts spacing to reduce overlapping labels and titles.

---

## 8. Histograms

### 8.1 What is a histogram?

A histogram displays the distribution of numerical data.

It groups numerical observations into intervals called **bins** and displays how many observations fall within each interval.

A histogram helps us understand:

- Data distribution.
- Frequency.
- Concentration of observations.
- Skewness.
- Possible outliers.
- Whether data is approximately symmetric.

### 8.2 Creating a histogram

```python
import matplotlib.pyplot as plt

ages = [18, 20, 22, 25, 25, 28, 30, 32, 35, 40, 45, 50]

plt.hist(ages)

plt.title("Age Distribution")
plt.xlabel("Age")
plt.ylabel("Frequency")

plt.show()
```

### 8.3 Understanding bins

A bin is an interval used to group numerical observations.

Example:

```python
plt.hist(ages, bins=5)
```

This requests five bins.

The number of bins changes how the distribution appears.

**Smaller bins:**

- Reveal more local details.
- Can expose concentrated regions.
- May make the overall distribution harder to understand.

**Larger bins:**

- Show the overall distribution more clearly.
- Reduce visual detail.
- May hide local patterns.

The best bin count depends on the dataset and the analytical question.

### 8.4 Controlling histogram range

Use the `range` parameter to restrict the displayed data interval.

```python
plt.hist(ages, bins=10, range=(20, 40))
```

This focuses the histogram on values between 20 and 40.

**Important:** Values outside the specified range are excluded from the histogram calculation. This does not mean the original dataset has been cleaned or those observations are invalid.

### 8.5 Understanding histogram shape

Common distribution patterns:

| Shape        | Interpretation                                             |
| ------------ | ---------------------------------------------------------- |
| Symmetric    | Values are distributed relatively evenly around the center |
| Right-skewed | Long tail extends toward higher values                     |
| Left-skewed  | Long tail extends toward lower values                      |
| Bimodal      | Two prominent peaks                                        |
| Uniform      | Values are distributed relatively evenly across the range  |

A histogram helps identify these patterns, but it does not establish why they occur.

---

## 9. Summary Statistics and Percentiles

### 9.1 What are summary statistics?

Summary statistics describe important characteristics of a dataset using numerical measures.

Common examples:

| Statistic          | Meaning                           |
| ------------------ | --------------------------------- |
| Count              | Number of observations            |
| Mean               | Arithmetic average                |
| Median             | Middle value after sorting        |
| Standard deviation | Measure of spread around the mean |
| Minimum            | Smallest value                    |
| Maximum            | Largest value                     |
| 25th percentile    | First quartile (Q1)               |
| 50th percentile    | Median (Q2)                       |
| 75th percentile    | Third quartile (Q3)               |

### 9.2 Understanding percentiles

A percentile indicates the value below which a specified percentage of observations falls.

For example:

- 10th percentile: approximately 10% of observations are at or below this value.
- 50th percentile: approximately 50% are at or below this value.
- 90th percentile: approximately 90% are at or below this value.

Percentiles are useful for understanding the relative position of observations.

### 9.3 Using Pandas describe()

```python
import pandas as pd

data = {
    "age": [18, 20, 22, 25, 30, 35, 40, 45]
}

df = pd.DataFrame(data)

print(df["age"].describe())
```

`.describe()` calculates several summary statistics together.

For numerical columns, the output normally includes:

- count
- mean
- std
- min
- 25%
- 50%
- 75%
- max

For categorical columns, it provides different summaries, such as count, unique values, most frequent value, and frequency.

---

## 10. Choosing the Right Graph

The choice of graph depends on the type of data and the question being investigated.

Two important data categories are:

### 10.1 Quantitative data

Quantitative data represents numerical quantities that can be meaningfully measured or calculated.

Examples:

- Age
- Price
- Revenue
- Temperature
- Time
- Height

Quantitative data can be:

**Discrete:** Countable values, such as number of purchases.

**Continuous:** Measurements that can take values within a range, such as height or temperature.

### 10.2 Categorical data

Categorical data represents groups, labels, or characteristics.

Examples:

- Product category
- Country
- Department
- Payment method
- Education level

A value can be represented by a number but still be categorical.

For example, if education levels are encoded as 1, 2, and 3, those numbers represent categories. They should not automatically be treated as quantities.

### 10.3 Graph selection reference

| Data and purpose                                  | Recommended graph | Function          |
| ------------------------------------------------- | ----------------- | ----------------- |
| Distribution of one numerical variable            | Histogram         | `plt.hist()`      |
| Spread and outliers of one numerical variable     | Box plot          | `plt.boxplot()`   |
| Compare categories                                | Bar chart         | `plt.bar()`       |
| Compare proportions                               | Pie chart         | `plt.pie()`       |
| Relationship between two numerical variables      | Scatter plot      | `plt.scatter()`   |
| Trend over time                                   | Line plot         | `plt.plot()`      |
| Relationships across multiple numerical variables | Heatmap           | `sns.heatmap()`   |
| Compare categories across groups                  | Grouped bar chart | `plt.bar()`       |

The graph should answer a specific analytical question.

---

## 11. Pie Charts

### 11.1 What is a pie chart?

A pie chart represents how categories contribute to a whole.

Each slice represents a category's share of the total.

Example:

```python
import matplotlib.pyplot as plt

categories = ["Food", "Transport", "Education", "Other"]
expenses = [300, 150, 250, 100]

plt.pie(
    expenses,
    labels=categories,
    autopct="%1.1f%%",
    startangle=90
)

plt.title("Expense Distribution")
plt.show()
```

### 11.2 Important arguments

| Argument       | Purpose                                     |
| -------------- | ------------------------------------------- |
| `x`            | Values represented by the slices            |
| `labels`       | Category names                              |
| `colors`       | Slice colors                                |
| `explode`      | Separates selected slices from the center   |
| `startangle`   | Controls the starting angle                 |
| `autopct`      | Displays percentage labels                  |

Example using `explode`:

```python
plt.pie(
    expenses,
    labels=categories,
    explode=(0.1, 0, 0, 0),
    autopct="%1.1f%%"
)
```

### 11.3 Limitations of pie charts

Pie charts should be used carefully.

They work reasonably well when:

- There are only a few categories.
- One or two categories dominate.
- The objective is to show part-to-whole proportions.

They become difficult to interpret when:

- There are many categories.
- Categories have similar values.
- Labels overlap.
- Readers need to compare small differences.

A bar chart is often a better alternative because lengths are easier to compare than angles or areas.

---

## 12. Correlation

### 12.1 What is correlation?

Correlation measures the strength and direction of an association between two variables.

The Pearson correlation coefficient ranges from -1 to +1.

| Value       | Interpretation                     |
| ----------- | ---------------------------------- |
| Close to +1 | Strong positive linear association |
| Close to -1 | Strong negative linear association |
| Close to 0  | Weak or no linear association      |

### 12.2 Positive correlation

When one variable increases, the other tends to increase.

Example:

- Advertising expenditure and sales may increase together.

### 12.3 Negative correlation

When one variable increases, the other tends to decrease.

Example:

- Product price and quantity demanded may have a negative association.

### 12.4 No or weak correlation

There is little or no linear association between the variables.

However, a coefficient close to zero does not rule out a nonlinear relationship.

### 12.5 Visualizing correlation with a scatter plot

```python
import matplotlib.pyplot as plt

advertising = [10, 20, 30, 40, 50, 60]
sales = [15, 25, 28, 40, 45, 60]

plt.scatter(advertising, sales)

plt.title("Advertising vs Sales")
plt.xlabel("Advertising Budget")
plt.ylabel("Sales")

plt.show()
```

### 12.6 Correlation is not causation

This is one of the most important principles in data science.

Correlation does not prove that one variable causes another.

Two variables may be correlated because:

- One variable influences the other.
- A third variable influences both.
- The relationship is coincidental.
- The data contains a hidden bias.

Example:

Ice cream sales and swimming accidents might increase during the same period.

That does not mean ice cream causes swimming accidents. Hot weather may influence both.

To investigate causality, we need additional reasoning, domain knowledge, and potentially controlled experiments or appropriate causal inference methods.

---

## 13. Scatter Plots

A scatter plot displays the relationship between two numerical variables.

Each point represents one observation.

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [10, 15, 20, 25, 30]

plt.scatter(x, y)

plt.title("Relationship Between X and Y")
plt.xlabel("X")
plt.ylabel("Y")

plt.show()
```

Scatter plots help identify:

- Positive and negative associations.
- Clusters.
- Outliers.
- Nonlinear relationships.
- Possible patterns between variables.

A scatter plot does not establish causation.

---

## 14. Line Plots

Line plots are particularly useful for displaying trends and changes across an ordered sequence, especially time.

```python
import matplotlib.pyplot as plt

months = ["Jan", "Feb", "Mar", "Apr", "May"]
revenue = [1000, 1200, 1100, 1500, 1800]

plt.plot(months, revenue, marker="o")

plt.title("Monthly Revenue")
plt.xlabel("Month")
plt.ylabel("Revenue")

plt.show()
```

Use line plots when the order of observations matters.

**Important:** Connecting unrelated categorical observations with lines can imply a continuous progression that does not actually exist.

---

## 15. Box Plots and Quartiles

### 15.1 What is a box plot?

A box plot summarizes the distribution of numerical data using quartiles and potential outliers.

It helps us understand:

- Median.
- Lower and upper quartiles.
- Spread of the data.
- Potential outliers.
- Differences between distributions.

### 15.2 Understanding the components

| Component              | Meaning                                                                   |
| ---------------------- | ------------------------------------------------------------------------- |
| Q1                     | 25th percentile                                                           |
| Q2                     | 50th percentile or median                                                 |
| Q3                     | 75th percentile                                                           |
| IQR                    | Interquartile range                                                       |
| Whiskers               | Extend to the most extreme non-outlier observations under the chosen rule |
| Points beyond whiskers | Potential outliers                                                        |

The Interquartile Range is:

```text
IQR = Q3 - Q1
```

A common box plot rule identifies potential outliers as values:

```text
Below Q1 - 1.5 × IQR
```

or

```text
Above Q3 + 1.5 × IQR
```

These are statistical flags, not automatic proof that observations are errors.

### 15.3 Creating a box plot

```python
import matplotlib.pyplot as plt

ages = [18, 20, 22, 25, 25, 28, 30, 32, 35, 80]

plt.boxplot(ages)

plt.title("Age Distribution")
plt.ylabel("Age")

plt.show()
```

The value 80 may appear as a potential outlier depending on the distribution.

Do not automatically remove outliers. Investigate whether they are valid observations.

---

## 16. Bar Charts

A bar chart compares values across categories.

```python
import matplotlib.pyplot as plt

categories = ["Food", "Transport", "Education"]
expenses = [300, 150, 250]

plt.bar(categories, expenses)

plt.title("Expenses by Category")
plt.xlabel("Category")
plt.ylabel("Amount")

plt.show()
```

### 16.1 Important functions

| Function | Purpose |
| ---------------- | ----------------------------------------- |
| `plt.bar()` | Creates vertical bars |
| `plt.barh()` | Creates horizontal bars |
| `plt.xticks()` | Controls X-axis tick positions and labels |
| `plt.yticks()` | Controls Y-axis tick positions and labels |

### 16.2 Horizontal bar charts

Useful when category names are long.

```python
plt.barh(categories, expenses)

plt.title("Expenses by Category")
plt.xlabel("Amount")
plt.ylabel("Category")

plt.show()
```

### 16.3 Grouped bar charts

Grouped bars allow us to compare multiple groups within the same categories.

```python
import matplotlib.pyplot as plt
import numpy as np

categories = ["A", "B", "C"]

group_a = [20, 35, 30]
group_b = [25, 30, 40]

x = np.arange(len(categories))
width = 0.35

plt.bar(x - width / 2, group_a, width, label="Group A")
plt.bar(x + width / 2, group_b, width, label="Group B")

plt.xticks(x, categories)
plt.xlabel("Category")
plt.ylabel("Value")
plt.title("Grouped Bar Chart")
plt.legend()

plt.show()
```

The bars are shifted in opposite directions by half their width to place them side by side.

### 16.4 Stacked bar charts

Stacked bars display multiple components within a single bar.

```python
import matplotlib.pyplot as plt

categories = ["A", "B", "C"]

group_a = [20, 35, 30]
group_b = [25, 30, 40]

plt.bar(categories, group_a, label="Group A")
plt.bar(categories, group_b, bottom=group_a, label="Group B")

plt.title("Stacked Bar Chart")
plt.xlabel("Category")
plt.ylabel("Value")
plt.legend()

plt.show()
```

The `bottom` argument determines the vertical position where the next group begins.

---

## 17. Matplotlib vs. Seaborn

Seaborn is another Python visualization library built on top of Matplotlib.

It provides a higher-level interface, particularly useful for statistical graphics and tabular datasets.

### Matplotlib example

```python
import matplotlib.pyplot as plt

plt.scatter(df["advertising"], df["sales"])

plt.xlabel("Advertising")
plt.ylabel("Sales")

plt.show()
```

### Seaborn example

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.scatterplot(
    data=df,
    x="advertising",
    y="sales"
)

plt.show()
```

### Main differences

| Matplotlib                                 | Seaborn                                        |
| ------------------------------------------ | ---------------------------------------------- |
| Lower-level plotting interface             | Higher-level statistical interface             |
| Detailed customization                     | Convenient statistical visualizations          |
| Explicitly passes data values              | Can use DataFrame column names                 |
| Greater control over individual components | Useful default styles and statistical features |

Seaborn does not replace Matplotlib. It builds on Matplotlib and can be used alongside it.

---

## 18. Additional Concepts You Should Know

The following topics extend the lecture and are important for practical data science.

### 18.1 Saving figures

Use `savefig()` to save a graph.

```python
plt.savefig(
    "monthly_revenue.png",
    dpi=300,
    bbox_inches="tight"
)
```

Important arguments:

- `dpi`: Controls image resolution.
- `bbox_inches="tight"`: Reduces unnecessary surrounding whitespace.
- `format`: Can specify PNG, PDF, SVG, and other supported formats.

Save the figure before calling `plt.show()` when using the pyplot interface.

### 18.2 Transparency

Use `alpha` to control transparency.

```python
plt.scatter(
    [1, 2, 3, 4],
    [10, 20, 15, 30],
    alpha=0.5
)
```

This is useful when many points overlap.

### 18.3 Figure and Axes labels

With the object-oriented interface:

```python
fig, ax = plt.subplots(figsize=(10, 5))

ax.plot([1, 2, 3], [10, 20, 30])

ax.set_title("Sales Trend")
ax.set_xlabel("Month")
ax.set_ylabel("Sales")

fig.savefig("sales_trend.png", dpi=300, bbox_inches="tight")

plt.show()
```

`fig` refers to the Figure, while `ax` refers to the Axes.

### 18.4 Heatmaps

Heatmaps use color intensity to represent numerical values in a matrix.

They are useful for:

- Correlation matrices.
- Comparing values across two dimensions.
- Identifying patterns in tabular data.

Example using Seaborn:

```python
import seaborn as sns
import matplotlib.pyplot as plt

correlation = df.corr(numeric_only=True)

sns.heatmap(
    correlation,
    annot=True,
    cmap="coolwarm"
)

plt.title("Correlation Matrix")
plt.show()
```

A correlation heatmap visualizes association, not causation.

### 18.5 Handling overlapping plots

When multiple graphs overlap or labels are difficult to read:

```python
plt.tight_layout()
```

For more control, use:

```python
fig, ax = plt.subplots(constrained_layout=True)
```

### 18.6 Working with dates

For time-series data, make sure dates are parsed correctly.

```python
import pandas as pd
import matplotlib.pyplot as plt

df["date"] = pd.to_datetime(df["date"])

plt.plot(df["date"], df["revenue"])

plt.title("Revenue Over Time")
plt.xlabel("Date")
plt.ylabel("Revenue")

plt.xticks(rotation=45)
plt.tight_layout()

plt.show()
```

Correct date handling is important because the order and spacing of time observations matter.

### 18.7 Avoid misleading visualizations

Good visualizations should represent data accurately.

Avoid:

- Truncated axes that exaggerate differences.
- Inappropriate chart types.
- Excessive colors.
- Too many categories in pie charts.
- Unclear axis labels.
- Missing units.
- Unexplained transformations.
- Treating correlation as causation.

Always ask whether the graph communicates the data honestly.

---

## 19. Practical Visualization Workflow

When working with a new dataset, use this process:

### Step 1 — Understand the data

```python
df.head()
df.info()
df.describe()
df.isnull().sum()
```

### Step 2 — Identify variable types

Separate numerical and categorical variables.

```python
numerical_columns = df.select_dtypes(include="number").columns

categorical_columns = df.select_dtypes(
    include=["object", "category"]
).columns
```

### Step 3 — Examine individual variables

Use:

- Histograms for numerical distributions.
- Box plots for spread and potential outliers.
- Bar charts for category frequencies.

### Step 4 — Examine relationships

Use:

- Scatter plots for numerical relationships.
- Line plots for ordered or time-based trends.
- Grouped bar charts for categorical comparisons.
- Heatmaps for correlation matrices.

### Step 5 — Interpret findings

For each graph, explain:

1. What variables are represented?
2. What pattern is visible?
3. Are there outliers?
4. What might explain the pattern?
5. What can and cannot be concluded?

### Step 6 — Communicate the results

Use clear titles, labels, units, legends, and appropriate scales.

---

## 20. Common Mistakes

| Mistake                                      | Better approach                                  |
| -------------------------------------------- | ------------------------------------------------ |
| Using a histogram for categories             | Use a bar chart                                  |
| Using a pie chart with many categories       | Use a bar chart                                  |
| Connecting unrelated observations with lines | Use a scatter or bar chart                       |
| Assuming correlation proves causation        | Investigate other explanations                   |
| Removing every outlier                       | Investigate its validity first                   |
| Using too many bins                          | Choose bins appropriate to the data              |
| Forgetting axis labels                       | Always label axes and units                      |
| Creating unreadable graphs                   | Adjust figure size and layout                    |
| Using only one visualization                 | Explore data from multiple perspectives          |
| Treating a graph as proof                    | Combine visualization with statistical reasoning |

---

## 21. Practice Exercises

Complete these exercises independently.

### Exercise 1 — Basic line plot

Create a line plot representing monthly revenue.

Requirements:

- Use five or more months.
- Add a title.
- Label both axes.
- Add markers.
- Display grid lines.

### Exercise 2 — Histogram

Create a dataset containing at least 30 numerical observations.

Requirements:

- Create a histogram.
- Try different bin counts.
- Change the histogram range.
- Explain how the appearance changes.

### Exercise 3 — Bar chart

Create a bar chart showing expenses by category.

Requirements:

- Include at least five categories.
- Label the axes.
- Give the graph a meaningful title.
- Identify the category with the highest expense.

### Exercise 4 — Scatter plot

Create a scatter plot representing advertising expenditure and sales.

Requirements:

- Include at least 10 observations.
- Identify the direction of the relationship.
- Explain why the graph cannot establish causation.

### Exercise 5 — Box plot

Create a dataset with normal observations and one unusually large value.

Requirements:

- Create a box plot.
- Identify the potential outlier.
- Explain the IQR rule.
- Discuss whether the observation should be removed.

### Exercise 6 — Multiple visualizations

Create a Figure containing four different graphs:

1. Histogram.
2. Scatter plot.
3. Bar chart.
4. Line plot.

Use `plt.subplots()`.

### Exercise 7 — Real-world EDA

Use a real dataset, preferably the WQU Mexican real-estate dataset.

Perform the following:

- Inspect the dataset.
- Identify numerical and categorical variables.
- Plot at least three numerical distributions.
- Create a categorical bar chart.
- Explore at least two relationships.
- Identify possible outliers.
- Write five observations supported by your visualizations.

---

## 22. Knowledge Check

Before considering this topic understood, make sure you can answer these questions without looking at the notes.

1. What is Matplotlib, and why is it used in data science?
2. What is Exploratory Data Analysis?
3. What is the difference between Figure, Axes, and Axis?
4. What is the difference between `plt.plot()` and `plt.scatter()`?
5. What does a histogram represent?
6. What is a bin?
7. How does changing the number of bins affect a histogram?
8. What is a percentile?
9. What is the difference between quantitative and categorical data?
10. When should you use a pie chart?
11. Why is a bar chart often better than a pie chart?
12. What does a correlation coefficient represent?
13. Why does correlation not prove causation?
14. What information does a box plot provide?
15. What is the Interquartile Range?
16. What is the purpose of `plt.subplots()`?
17. What is the difference between Matplotlib and Seaborn?
18. How do you save a graph to a file?
19. Why should you label the axes?
20. How would you choose a graph for a new dataset?

---

## 23. Essential Functions Reference

| Function             | Purpose                    |
| -------------------- | -------------------------- |
| `plt.figure()`       | Create a Figure            |
| `plt.subplots()`     | Create a Figure and Axes   |
| `plt.plot()`         | Line plot                  |
| `plt.scatter()`      | Scatter plot               |
| `plt.hist()`         | Histogram                  |
| `plt.bar()`          | Vertical bar chart         |
| `plt.barh()`         | Horizontal bar chart       |
| `plt.pie()`          | Pie chart                  |
| `plt.boxplot()`      | Box plot                   |
| `plt.subplot()`      | Add/select a subplot       |
| `plt.title()`        | Add title                  |
| `plt.xlabel()`       | Label X-axis               |
| `plt.ylabel()`       | Label Y-axis               |
| `plt.legend()`       | Display legend             |
| `plt.grid()`         | Display grid               |
| `plt.xlim()`         | Set X-axis limits          |
| `plt.ylim()`         | Set Y-axis limits          |
| `plt.xticks()`       | Configure X-axis ticks     |
| `plt.yticks()`       | Configure Y-axis ticks     |
| `plt.tight_layout()` | Adjust layout              |
| `plt.savefig()`      | Save a figure              |
| `plt.show()`         | Display a figure           |
| `ax.set_title()`     | Set Axes title             |
| `ax.set_xlabel()`    | Set X-axis label           |
| `ax.set_ylabel()`    | Set Y-axis label           |

---

## 24. Official Documentation and References

- [Matplotlib Official Documentation](https://matplotlib.org/stable/)
- [Matplotlib Pyplot Tutorial](https://matplotlib.org/stable/tutorials/pyplot.html)
- [Matplotlib Plot Types](https://matplotlib.org/stable/plot_types/index.html)
- [Matplotlib Gallery](https://matplotlib.org/stable/gallery/index.html)
- [Matplotlib Markers Reference](https://matplotlib.org/stable/api/markers_api.html)
- [Matplotlib Boxplot Documentation](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.boxplot.html)
- [Seaborn Official Documentation](https://seaborn.pydata.org/)

---

## 25. Completion Checklist

### Fundamentals

- [ ] Understand the purpose of Matplotlib.
- [ ] Understand EDA and the data science workflow.
- [ ] Understand Figure, Axes, and Axis.
- [ ] Import and use `matplotlib.pyplot`.

### Creating and customizing graphs

- [ ] Create line plots.
- [ ] Create scatter plots.
- [ ] Customize titles and axis labels.
- [ ] Change figure dimensions.
- [ ] Customize colors, markers, and line styles.
- [ ] Add legends and grid lines.
- [ ] Create multiple subplots.
- [ ] Save visualizations.

### Statistical visualization

- [ ] Create histograms and understand bins.
- [ ] Interpret percentiles and summary statistics.
- [ ] Create box plots and understand IQR.
- [ ] Interpret potential outliers.
- [ ] Understand correlation and its limitations.

### Choosing and interpreting graphs

- [ ] Distinguish quantitative and categorical data.
- [ ] Select appropriate graphs for different analytical questions.
- [ ] Explain when pie charts are inappropriate.
- [ ] Create grouped and stacked bar charts.
- [ ] Understand the role of Seaborn.

### Independent demonstration

- [ ] Complete the practice exercises.
- [ ] Perform EDA on a real dataset.
- [ ] Explain the findings from each visualization.
- [ ] Create a small visualization report independently.

**Final learning objective:** Be able to take an unfamiliar dataset, choose suitable visualizations, create clear and accurate graphs, identify meaningful patterns, and communicate evidence-based observations.
