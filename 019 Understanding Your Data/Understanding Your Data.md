# Data Understanding

**Data Understanding** is the first step of a data science project. Before building a model, we need to understand the **size, structure, quality, and relationships** within the dataset.

Example dataset: **Titanic**

---

## 1. Understand the Dataset Size

Check the number of **rows and columns**.

```python
df.shape
```

Example:

```text
(891, 12)
```

* `891` → rows / records
* `12` → columns / features

---

## 2. View the Data

### First Few Rows

```python
df.head()
```

Helps understand:

* Column names
* Data format
* Initial values

### Random Samples

```python
df.sample(5)
```

* Random samples can provide a more representative view.
* Useful when the dataset is ordered in a particular way.

---

## 3. Understand Data Types

```python
df.info()
```

Shows:

* Column names
* Number of non-null values
* Data types
* Memory usage

Common types:

```text
int64
float64
object
bool
```

> **Memory usage** can help identify opportunities for data-type optimization.

---

## 4. Check Missing Values

```python
df.isnull().sum()
```

Example:

```text
Age         177
Cabin       687
Embarked      2
```

Missing values may require:

* **Imputation**
* **Removing rows/columns**
* Other appropriate preprocessing

---

## 5. Statistical Summary

```python
df.describe()
```

Provides statistics such as:

* **Count**
* **Mean**
* **Standard deviation**
* **Minimum**
* **25% percentile**
* **50% percentile (median)**
* **75% percentile**
* **Maximum**

Useful for understanding the **distribution and possible outliers** in numerical columns.

### Standard Deviation

Standard deviation indicates how spread out values are around the mean.

---

## 6. Check Duplicate Records

```python
df.duplicated().sum()
```

To remove duplicates:

```python
df.drop_duplicates(inplace=True)
```

Duplicates should be investigated before removing them because sometimes repeated records can be legitimate.

---

## 7. Correlation Analysis

Correlation measures the **relationship between numerical variables**.

```python
df.corr(numeric_only=True)
```

Example:

```text
Age       Fare
Age       1.00     0.10
Fare      0.10     1.00
```

* Positive correlation → variables tend to increase together.
* Negative correlation → one tends to increase when the other decreases.
* Near `0` → little linear relationship.

> [!IMPORTANT]
> **Correlation does not mean causation.** A high correlation does not prove that one variable causes the other.

Correlation can help with **feature understanding and selection**, but it should not be the only factor used to decide which features to keep.

---

## Data Understanding Workflow

```text
Dataset
   ↓
Check Shape
   ↓
View Data
   ↓
Check Data Types
   ↓
Check Missing Values
   ↓
Statistical Summary
   ↓
Check Duplicates
   ↓
Correlation Analysis
   ↓
Understand Data Quality
   ↓
EDA / Data Cleaning
```

---

## 🧠 Quick Revision

* **Data Understanding** is the first step before detailed analysis and model building.
* `df.shape` → dataset dimensions.
* `df.head()` → first rows.
* `df.sample()` → random records.
* `df.info()` → data types, null counts, and memory usage.
* `df.isnull().sum()` → missing values.
* `df.describe()` → statistical summary.
* `df.duplicated().sum()` → duplicate records.
* `df.corr()` → numerical relationships.
* **Correlation ≠ causation.**
* This understanding prepares the dataset for **EDA, preprocessing, and machine learning**.
