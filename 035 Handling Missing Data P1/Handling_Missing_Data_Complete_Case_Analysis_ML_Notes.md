# Handling Missing Data — Complete Case Analysis (CCA)

> **Machine Learning · Feature Engineering · Data Preprocessing**  
> **Focus:** Missing-data basics, Complete Case Analysis (CCA), validation, and an overview of imputation.

## 1. What Is Missing Data?

- **Missing data** means a value is not available for one or more observations (rows) in a dataset.
- In Pandas, missing values are commonly represented by **`NaN`**, **`None`**, or **`pd.NA`**.
- Missing values may occur because of incomplete forms, sensor failures, data-entry errors, or unavailable information.
- Many commonly used ML estimators cannot directly accept missing values, so we must **remove, impute, or otherwise handle** them before training. Some algorithms do support missing values natively.

### Two Main Approaches

| Approach | What It Does | Main Trade-off |
|---|---|---|
| **Deletion** | Removes rows (or sometimes columns) with missing values | Loses information |
| **Imputation** | Replaces missing values with estimated or chosen values | Introduces assumptions about missing values |

```text
Dataset with Missing Values
           |
           +-------------------------+
           |                         |
    Delete Missing Rows       Fill Missing Values
           |                         |
 Complete Case Analysis           Imputation
           |                         |
     Smaller Dataset           More Rows Retained
```

---

## 2. What Is Complete Case Analysis (CCA)?

**Complete Case Analysis (CCA)**, also called **listwise deletion**, removes any observation with a missing value in the *variables selected for the analysis*.

### Example

**Original dataset:**

| Student | Age | Score |
|---|---:|---:|
| A | 19 | 85 |
| B | — | 74 |
| C | 21 | 90 |
| D | 20 | — |
| E | 22 | 88 |

**After CCA:**

| Student | Age | Score |
|---|---:|---:|
| A | 19 | 85 |
| C | 21 | 90 |
| E | 22 | 88 |

- Rows **B** and **D** are removed because a required value is missing.
- The remaining rows contain complete values for the selected columns.
- The number of rows decreases from **5 to 3**; **40%** of the rows are lost.

### Mathematical Representation

A row is retained only if all required variables are observed:

$$
R_i = \begin{cases}
1, & \text{if all required values in row } i \text{ are observed} \\
0, & \text{otherwise}
\end{cases}
$$

$$
D_{\mathrm{CCA}} = \{\,x_i \in D : R_i = 1\,\}
$$

Where:

- **\(D\)** = original dataset.
- **\(D_{\mathrm{CCA}}\)** = dataset after deleting incomplete rows.
- **\(R_i\)** = indicator that row **\(i\)** is complete.

---

## 3. Understanding MCAR, MAR, and MNAR

The reason values are missing matters as much as the amount of missing data.

| Mechanism | Meaning | Simple Example |
|---|---|---|
| **MCAR** — Missing Completely at Random | Missingness is unrelated to observed and unobserved values | A random technical failure removes some responses |
| **MAR** — Missing at Random | Missingness can depend on *observed* variables, but not on the missing value once those variables are considered | Income responses are missing more often in a particular recorded age group |
| **MNAR** — Missing Not at Random | Missingness depends on unobserved information, possibly the missing value itself | People with very high incomes are less likely to report their income |

### Why Is MCAR Important for CCA?

Under **MCAR**, complete cases are effectively a random subset of the original observations. Therefore, CCA can preserve population distributions **on average**, although the smaller sample will still have sampling variation.

> [!IMPORTANT]
> **MCAR is a useful, strong assumption for CCA—not a guarantee that every CCA analysis outside MCAR is invalid.** Whether CCA introduces bias also depends on the model, estimand, and missingness process. For introductory analysis, CCA is safest to consider when missingness is small and plausibly MCAR.

**Remember:** Similar before/after plots provide evidence about stability, but **do not prove MCAR**.

---

## 4. When Should We Use CCA?

CCA may be reasonable when:

- Only a **small proportion of rows** is lost (often **less than about 5%** as a starting heuristic).
- Missingness is plausibly **MCAR** or otherwise unlikely to bias the intended analysis.
- The reduced dataset still contains enough observations for training and evaluation.
- The retained data represent important groups and target classes adequately.

### When Should We Avoid It?

- A large fraction of rows would be deleted.
- Missingness is associated with relevant variables or outcomes.
- Rare or important categories would almost disappear.
- The training data would become too small.
- The deployed system must make predictions on records containing missing values.

> [!NOTE]
> **5% missing rows** and **5% missing values in a column** are not the same thing. Even several columns with small amounts of missingness can cause substantial **row loss** when CCA is applied to all columns.

---

## 5. Important Formulas

### A. Missing Percentage for a Column

$$
\text{Missing \%} = \frac{\text{Missing values in the column}}{\text{Total rows}}\times 100
$$

**Example:** If 20 of 500 rows have a missing `Age` value:

$$
\text{Missing \%} = \frac{20}{500}\times100=4\%
$$

### B. Percentage of Rows Lost After CCA

$$
\text{Rows Lost \%} = \frac{N_{\mathrm{before}}-N_{\mathrm{after}}}{N_{\mathrm{before}}}\times100
$$

### C. Percentage of Rows Retained

$$
\text{Rows Retained \%} = \frac{N_{\mathrm{after}}}{N_{\mathrm{before}}}\times100
$$

**Example:** A dataset shrinks from 1,000 to 940 rows:

$$
\text{Rows Lost \%} = \frac{1000-940}{1000}\times100=6\%
$$

$$
\text{Rows Retained \%} = 94\%
$$

### D. Category Proportion

$$
P(\text{Category}=c)=\frac{\text{Number of non-missing records in category }c}{\text{Total non-missing records for that feature}}
$$

Compare category proportions **before and after CCA** to see whether important groups are under- or over-represented.

---

## 6. Practical Implementation — Pandas + Titanic Dataset

We'll work with `age`, `fare`, and `embarked`, and use `survived` as the target.

### Step 1 — Load and Inspect the Dataset

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Seaborn's Titanic dataset (internet may be required for first download)
df = sns.load_dataset("titanic")
df = df[["survived", "age", "fare", "embarked"]].copy()

print(df.head())
print(df.info())
```

### Step 2 — Measure Missing Values

```python
# Missing values per column
print(df.isna().sum())

# Missing percentage per column
missing_pct = (df.isna().mean() * 100).round(2)
print(missing_pct.sort_values(ascending=False))

# Rows that have at least one missing value
incomplete_rows_pct = df.isna().any(axis=1).mean() * 100
print(f"Incomplete rows: {incomplete_rows_pct:.2f}%")
```

- **`isna().sum()`** → Number of missing values in each column.
- **`isna().mean() * 100`** → Percentage of missing values per column.
- **`isna().any(axis=1)`** → Identifies rows with at least one missing value.

### Step 3 — Apply Complete Case Analysis

```python
required_columns = ["survived", "age", "fare", "embarked"]

cca_df = df.dropna(subset=required_columns).copy()

before = len(df)
after = len(cca_df)

print("Before CCA:", before)
print("After CCA:", after)
print(f"Rows lost: {(before - after) / before * 100:.2f}%")
print(f"Rows retained: {after / before * 100:.2f}%")
```

**Key detail:** `dropna()` without `subset=` considers **all columns**, which can unnecessarily remove rows because of columns that are not being used in the model.

### Step 4 — Compare Numerical Distributions

```python
for col in ["age", "fare"]:
    plt.figure(figsize=(7, 4))

    sns.kdeplot(data=df, x=col, label="Before CCA")
    sns.kdeplot(data=cca_df, x=col, label="After CCA")

    plt.title(f"{col}: Before vs After CCA")
    plt.xlabel(col)
    plt.legend()
    plt.tight_layout()
    plt.show()
```

**Interpretation:**

- Similar curves → CCA has not visibly changed that feature's observed distribution much.
- Different curves → CCA may have changed the composition of the retained sample.
- Compare **medians, means, and other statistics** too; plots alone are not sufficient.

```python
print("Before CCA:\n", df[["age", "fare"]].describe())
print("After CCA:\n", cca_df[["age", "fare"]].describe())
```

### Step 5 — Compare Categorical Distributions

```python
before_pct = (
    df["embarked"].value_counts(normalize=True, dropna=True) * 100
)
after_pct = (
    cca_df["embarked"].value_counts(normalize=True, dropna=True) * 100
)

comparison = pd.concat(
    [before_pct.rename("Before CCA (%)"),
     after_pct.rename("After CCA (%)")],
    axis=1
).fillna(0).round(2)

print(comparison)
```

- The **category percentages** help identify changes in group representation.
- Check the **target distribution** (`survived`) as well if you're preparing a classification dataset.

### Step 6 — Decide Whether CCA Is Acceptable

Check these questions before keeping `cca_df`:

1. **How many rows did we lose?**
2. **Did numeric distributions or category proportions change substantially?**
3. **Is missingness plausibly random?**
4. **Do we still have enough data for useful modeling?**
5. **Can our production system handle future missing values?**

> [!WARNING]
> **Do not use distribution similarity as proof that CCA causes no bias.** Also, avoid deciding preprocessing based on the test set. For reliable model evaluation, use a proper train/test split and keep data-dependent preprocessing within the training workflow.

---

## 7. Advantages and Limitations

| Advantages | Limitations |
|---|---|
| Very simple and quick to implement | Can discard useful observations |
| Does not invent replacement values | May substantially reduce sample size |
| Convenient baseline for low missingness | Can introduce selection bias |
| No imputation model required | Does not solve missing values in live predictions |
| Can work well under appropriate assumptions | Even MCAR produces smaller samples and less statistical precision |

---

## 8. Other Missing-Data Strategies — Roadmap

### A. Univariate Imputation

Uses information from an individual feature to fill its missing values.

| Method | Basic Idea | Typical Use |
|---|---|---|
| **Mean** | Replace with feature mean | Approximately symmetric numerical data |
| **Median** | Replace with feature median | Skewed numerical data or outliers |
| **Most Frequent** | Use the mode | Categorical data |
| **Constant** | Use a chosen value, e.g., `"Missing"` | Meaningful missing category or placeholder |
| **Random Sample** | Sample from observed values | Preserve some observed variation, with caveats |
| **End-of-Distribution** | Use an extreme chosen value | When missingness might carry information; validate carefully |

**Scikit-learn:** `SimpleImputer`

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy="median")
X_filled = imputer.fit_transform(df[["age", "fare"]])
```

### B. Multivariate Imputation

Uses relationships between multiple features.

- **`KNNImputer`** → Estimates missing values using nearby similar observations.
- **`IterativeImputer`** → Predicts missing features from other features in an iterative process; an approach related to MICE.

### C. Missing Indicator

Creates a new feature identifying whether a value was originally missing.

```python
# 1 = missing, 0 = observed
df["age_was_missing"] = df["age"].isna().astype(int)
```

- Can preserve information about **the missingness itself**.
- Often used alongside imputation rather than as a complete replacement.

> [!IMPORTANT]
> For ML training, fit imputers on **training data only** and apply the fitted imputers to validation, test, and production data. A Scikit-learn **Pipeline** helps prevent data leakage.

---

## 9. CCA vs. Imputation — Quick Comparison

| Factor | Complete Case Analysis | Imputation |
|---|---|---|
| Method | Delete incomplete rows | Fill missing values |
| Dataset size | Usually decreases | Usually preserves rows |
| Complexity | Low | Low to high, depending on method |
| Main risk | Information loss and selection bias | Inaccurate replacement and distorted relationships |
| Best suited to | Low missingness with suitable assumptions | Situations where retaining observations is important |
| Deployment | Cannot handle missing predictors by itself | Can support missing predictors using a fitted preprocessing pipeline |

---

## 🧠 Quick Revision

- **Missing data** must be handled unless the chosen algorithm supports it natively.
- **CCA (listwise deletion)** removes rows with missing values in the **required columns**.
- **MCAR** means missingness is unrelated to both observed and unobserved information.
- CCA is most attractive when **row loss is small** and missingness is plausibly MCAR; **5% is a heuristic, not a rule**.
- Measure missingness using **`isna().sum()`**, **`isna().mean()`**, and incomplete-row counts.
- Apply CCA using **`dropna(subset=...)`** and calculate the **rows lost %**.
- Compare **numerical distributions**, **categorical proportions**, and **target balance** before and after deletion.
- For larger or non-random missingness, consider **SimpleImputer, KNNImputer, IterativeImputer**, or a **missing indicator**.

> **One-line takeaway:** **CCA is easy, but deleting rows is safe only when the amount and mechanism of missingness make the remaining sample suitable for the task.**
