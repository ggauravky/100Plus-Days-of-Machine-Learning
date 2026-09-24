# Univariate Analysis in EDA

## What is EDA?

**EDA (Exploratory Data Analysis)** is the process of exploring a dataset to understand:

* Patterns
* Distributions
* Trends
* Relationships
* Data quality issues

Types of analysis:

```text
Univariate   → One variable
Bivariate    → Two variables
Multivariate → More than two variables
```

---

## What is Univariate Analysis?

**Univariate Analysis** studies **one variable at a time**.

Its main purpose is to understand:

* Distribution
* Frequency
* Central values
* Spread
* Skewness
* Outliers

Example:

```text
Analyze only "Age"
or
Analyze only "Gender"
```

---

# Types of Data

## 1. Numerical Data

Represents measurable quantities.

Examples:

* Age
* Salary
* Height
* Price
* Fare

---

## 2. Categorical Data

Represents categories or labels.

Examples:

* Gender
* Country
* Ticket class
* City

---

# Univariate Analysis of Categorical Data

## 1. Count Plot

Shows how many records belong to each category.

```python
import seaborn as sns

sns.countplot(x="Survived", data=df)
```

Example:

```text
Survived = 0 → 549 passengers
Survived = 1 → 342 passengers
```

Useful for understanding **category frequency**.

---

## 2. Pie Chart

Shows the **percentage contribution** of each category.

```python
df["Survived"].value_counts().plot(kind="pie", autopct="%.2f")
```

Example:

```text
Not Survived → 61.6%
Survived     → 38.4%
```

Best for quickly understanding category proportions.

---

# Univariate Analysis of Numerical Data

## 1. Histogram

A **Histogram** shows the distribution of numerical data using ranges called **bins**.

```python
import matplotlib.pyplot as plt

plt.hist(df["Age"], bins=20)
plt.show()
```

It helps identify:

* Where most values lie
* Spread of data
* Shape of distribution

Example:

```text
Age

0–10   → Few passengers
20–30  → Large number
30–40  → Large number
60+    → Few passengers
```

---

## 2. Distribution Plot

A distribution plot combines:

* Histogram
* **KDE curve**

Example:

```python
sns.distplot(df["Age"])
```

The **KDE (Kernel Density Estimation)** curve gives a smooth representation of the distribution.

It helps understand whether data is:

* Normally distributed
* Right-skewed
* Left-skewed

---

## 3. Box Plot

A **Box Plot** helps understand:

* Distribution
* Spread
* Median
* Quartiles
* Outliers

```python
sns.boxplot(x=df["Age"])
```

It represents the five-number summary:

```text
Minimum
   ↓
Q1
   ↓
Median
   ↓
Q3
   ↓
Maximum
```

Values far outside the main range may appear as **outliers**.

---

# Descriptive Statistics

Useful functions:

```python
df["Age"].mean()
df["Age"].min()
df["Age"].max()
df["Age"].median()
```

Or:

```python
df["Age"].describe()
```

Example output:

```text
count
mean
std
min
25%
50%
75%
max
```

This gives a quick numerical summary of the column.

---

# Skewness

**Skewness** tells us whether the data distribution is symmetrical or tilted toward one side.

```python
df["Age"].skew()
```

### Types

```text
Symmetrical
      ↓
Balanced distribution
```

```text
Positive Skew
      ↓
Long tail toward right
```

```text
Negative Skew
      ↓
Long tail toward left
```

Skewness helps understand the shape of numerical data.

---

# Outliers

**Outliers** are values that are very different from most observations.

Example:

```text
Most ages → 20–60
One age   → 100
```

Box plots are commonly used to visually identify such values.

Outliers may require further investigation before model building.

---

# Simple Workflow

```text
Select One Column
      ↓
Identify Data Type
      ↓
Categorical?
 ├── Count Plot
 └── Pie Chart

Numerical?
 ├── Histogram
 ├── Distribution Plot
 ├── Box Plot
 ├── Descriptive Statistics
 └── Skewness
```

---

## 🧠 Quick Revision

* **Univariate Analysis** studies one variable at a time.
* Data can mainly be **numerical** or **categorical**.
* **Count Plot** → category frequency.
* **Pie Chart** → category percentages.
* **Histogram** → numerical distribution.
* **Distribution Plot** → histogram + KDE curve.
* **Box Plot** → spread, quartiles, median, and outliers.
* **Skewness** shows whether the distribution is symmetric, right-skewed, or left-skewed.
* Univariate analysis is usually one of the first steps in **EDA**.
