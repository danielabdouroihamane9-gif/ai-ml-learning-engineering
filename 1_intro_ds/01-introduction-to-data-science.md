# Introduction to Data Science

> Study notes for the *What is Data Science?* introductory slide deck (Matsuo-Iwasawa Laboratory, The University of Tokyo, 2025). Sections marked **Extra context** add practical concepts that help avoid common beginner mistakes and were not fully covered in the slides.

## 1. What is data science?

Data science combines data collection, analysis, programming, mathematics, statistics, and domain knowledge to answer questions and create useful value. It is not just “building a machine-learning model”: understanding the problem and data, communicating findings, and using results responsibly are also part of the work.

A useful mental model is:

> Data → analysis/model → insight → decision or value

Examples:

- **Video recommendations:** estimate which videos a user may want to watch.
- **Retail forecasting:** predict item demand so a store can plan inventory.
- **Sports recruitment:** compare performance and salary data to inform recruitment.

The prediction is useful only if it supports a clearly stated objective. More watch time, lower stock-outs, or better recruitment decisions are examples of outcomes; a model score by itself is not business value.

## 2. Types of data

### Structured data

Data organized into rows and columns, such as a spreadsheet or database table. Each row usually represents an observation (for example, one car) and each column a feature (for example, engine size or price).

- **Quantitative / numeric:** measurable numbers.
  - **Discrete:** counts, such as number of items sold.
  - **Continuous:** measurements, such as height or temperature.
- **Qualitative / categorical:** labels or groups.
  - **Nominal:** categories without a natural order, such as department.
  - **Ordinal:** categories with an order, such as small, medium, large.

A number used as a label (such as postal code or product ID) is not automatically a meaningful numeric measurement.

### Unstructured data

Data that does not naturally fit into a simple table, such as images, video, audio, and free-form documents. It can still be analyzed, often after extracting useful features or representations.

**Extra context:** Some data is **semi-structured**: it has some organization but not a fixed rectangular table, such as JSON, XML, and event logs. Real projects commonly combine structured, semi-structured, and unstructured sources. Most tabular datasets also mix numeric and categorical columns.

## 3. Where data comes from

Common sources include:

- Surveys and experiments
- Company or operational records
- Open data, including government statistics and public-institution datasets
- Competition datasets and problem statements (the slides mention Kaggle and SIGNATE)

Before using a dataset, check its source, collection method, date range, definitions, coverage, licensing, and whether it is appropriate for the question. “Freely available” does not always mean unrestricted reuse.

**Extra context:** Consider privacy, consent, sensitive attributes, and possible sampling or measurement bias. Ask who is missing from the data and whether the labels or measurements are trustworthy.

## 4. The iterative data-science workflow

The slides present four steps. They are a cycle, not a one-way checklist:

1. **Understand the data** — inspect its structure, distributions, missingness, and possible relevance to the objective; form hypotheses.
2. **Preprocess the data** — clean and transform it into a usable format.
3. **Build a model** — split data, select an appropriate approach, and train it.
4. **Evaluate the model** — measure performance on data not used to fit the model; revisit earlier steps if results are inadequate.

### Step 1: Understand the data

Start by clarifying the question and inspecting the dataset:

- What does each row represent? What does each column mean and in what units?
- How many rows and columns are there? Are there duplicate records?
- What are the data types, ranges, category counts, and missing-value rates?
- How are numeric values distributed? Are there outliers or skew?
- Do visualizations suggest relationships worth investigating?

Basic summaries include the **mean**, **median**, and **variance**. Histograms help inspect a numeric distribution; bar charts show category counts; scatter plots can reveal relationships between two numeric variables.

A plot can suggest a hypothesis, not prove that one variable causes another. For example, larger engines may be associated with higher car prices, but other factors (brand, age, condition, and trim) may also matter.

### Step 2: Preprocess the data

Preprocessing includes cleaning and transforming data. Keep a record of choices because each can affect the conclusions.

#### Missing values and outliers

- A **missing value** was not recorded or is unavailable.
- An **outlier** is unusually far from much of the data. It may be an error, a rare valid case, or an important signal.
- Possible responses include correcting a known error, imputing a value, adding a missingness indicator, or removing a record/feature when justified.
- Do not automatically replace every missing value with the mean or delete every outlier. Investigate why it is missing or unusual and whether the choice would bias the analysis.

#### Scaling / standardization

Features can use very different units or ranges. **Standardization** transforms a numeric feature using its training-set mean and standard deviation:

`z = (x - mean) / standard_deviation`

The transformed training values have mean approximately 0 and standard deviation approximately 1 (equivalently, variance approximately 1, subject to convention and calculation details). Scaling is important for some methods, such as distance-based models and many optimization-based models; tree-based models often do not require it.

#### Encoding categories

Models often need categories represented numerically. **One-hot (dummy) encoding** creates indicator columns. For example, a `Department` field might become `Sales`, `R&D`, and `HR` columns, with one column set to 1 for each row and the others 0.

The slide example uses two department indicators, so the omitted category is represented by all zeros. This is a common reference-category encoding. If all categories receive their own indicator, each row has one 1. Avoid treating arbitrary category labels (e.g., HR=1, Sales=2, R&D=3) as quantities unless the order and numeric meaning are real.

**Important:** Fit preprocessing steps (imputation values, scaling parameters, category mappings) on training data only, then apply the learned transformations to validation/test data. This prevents information from the test set leaking into training.

## 5. Model building

A **model** is a set of computational rules that maps inputs to an output for a defined task. In a simple linear model:

`y = a x + b`

- `x`: input feature
- `y`: predicted output / target
- `a`, `b`: parameters learned from training data

For car-price prediction, engine size might be an input and price the target. With multiple inputs, a model may use engine size, age, mileage, and other features.

### Three model-building activities

1. **Data splitting:** reserve data for evaluation rather than using every example to train.
2. **Model selection:** choose a model family that matches the target and objective.
3. **Model training:** use training data to learn patterns and model parameters.

### Common task types

- **Regression:** predict a numeric quantity, such as a house price or sales amount.
- **Classification:** assign an observation to a class, such as spam/not spam or poisonous/safe.
- **Clustering:** group similar observations without predefined target labels, such as exploratory customer segmentation.

### Train/test split and cross-validation

- **Hold-out:** split data into a training set and a test set. Train on the former; use the latter for an estimate of performance on unseen data.
- **k-fold cross-validation:** divide the data into `k` folds; train and validate `k` times, using a different fold for validation each time. Average the scores. This uses data more efficiently and reduces dependence on one split, but costs more computation.

**Extra context:** In practical projects, use three roles where appropriate:

- **Training set:** fit model parameters.
- **Validation set (or cross-validation):** compare models and tune choices.
- **Test set:** final, relatively unbiased check after choices are complete.

Do not repeatedly tune against the test set; doing so gradually makes it part of the model-selection process. For classification with uneven class frequencies, stratified splits can preserve class proportions. For time-dependent data, split by time (train on the past, evaluate on the future), not randomly. Related records from the same person/entity may also need to stay in one split.

## 6. Model evaluation

Evaluate whether the trained model meets the objective on unseen data. Metrics depend on the task; choose them based on the real cost of different mistakes.

### Regression: Mean Squared Error

For actual values `yᵢ` and predictions `ŷᵢ` across `n` examples:

`MSE = (1/n) × Σ (yᵢ - ŷᵢ)²`

- MSE is non-negative; lower is better when comparing on the same target and dataset.
- Squaring makes large errors count disproportionately and gives the metric squared units (for example, dollars²).
- Because of the scale of the target, MSE is not directly comparable across differently scaled targets.

Slide example:

| Actual price | Predicted price | Absolute error | Squared error |
| ---: | ---: | ---: | ---: |
| 47,000 | 46,400 | 600 | 360,000 |
| 20,000 | 21,500 | 1,500 | 2,250,000 |
| 32,000 | 32,900 | 900 | 810,000 |

`MSE = (360,000 + 2,250,000 + 810,000) / 3 = 1,140,000`

**Extra context:** Also learn **MAE** (mean absolute error; easier to interpret in the target's original units), **RMSE** (square root of MSE; original units), and **R²** (a relative fit statistic with important caveats). No single metric is best for every use case.

### Classification: accuracy and the confusion matrix

**Accuracy** is the fraction of predictions that are correct:

`Accuracy = correct predictions / total predictions`

In the slide's mushroom example, 35 poisonous and 50 safe examples are correctly classified out of 100, so accuracy is `85 / 100 = 0.85` (85%).

The confusion matrix also shows error types:

|                     | Actual poisonous | Actual safe |
|:--------------------|-----------------:|------------:|
| Predicted poisonous |                35|          10 |
| Predicted safe      |                 5|         50  |

**Extra context:** Accuracy can be misleading when classes are imbalanced or when mistake costs differ. For example, missing a poisonous mushroom may be much worse than incorrectly flagging a safe one. Learn **precision**, **recall/sensitivity**, **specificity**, and **F1 score**, and inspect the confusion matrix. Select the metric that reflects the consequences of errors.

## 7. Common pitfalls to remember

- **Correlation is not causation:** an observed relationship does not establish that one variable causes another.
- **Data leakage:** information unavailable at prediction time, or information from the test set, accidentally influences training/preprocessing.
- **Overfitting:** a model fits training examples well but performs poorly on new data. A proper validation strategy helps detect it.
- **Unrepresentative data:** performance on a test set is only informative if the test data resembles the intended real-world use—or if differences are explicitly evaluated.
- **Unclear objective:** define what a useful result means before choosing a model or metric.
- **Responsible use:** check privacy, fairness, security, and the impact of decisions made from predictions.

## 8. A beginner's project checklist

1. Write the question and intended decision in one or two sentences.
2. Identify the observation (row), target (what to predict), and candidate features (inputs).
3. Inspect data types, units, distributions, missing values, duplicates, and class balance.
4. Create a sensible train/validation/test strategy before fitting transformations or models.
5. Build a simple baseline first; choose a metric appropriate to the task.
6. Fit preprocessing only on training data; apply the same transformations to held-out data.
7. Compare results with the baseline, inspect errors, and document limitations.
8. Revisit the data, objective, features, or model if evaluation is not good enough.

## 9. Quick review

- What are the four workflow steps? **Data understanding → preprocessing → model building → model evaluation.**
- Which task predicts a number? **Regression.**
- Which task predicts a label? **Classification.**
- What is the purpose of a test set? **Estimate performance on data not used to fit the model.**
- What does a smaller MSE indicate on the same dataset? **Smaller average squared prediction error.**
- Why might accuracy alone be insufficient? **It hides which kinds of errors occur and can mislead with imbalanced classes or unequal error costs.**

## Glossary

- **Feature:** an input variable used by a model.
- **Target / label:** the value or class the model is intended to predict.
- **Observation / sample:** one example or row in a dataset.
- **Parameter:** a value learned by a model during training.
- **Training:** fitting a model to training examples.
- **Generalization:** how well a model performs on new, unseen examples.
- **Metric:** a numerical measure used to assess model performance.
- **Data leakage:** accidental use of information that should not be available during fitting or evaluation.
