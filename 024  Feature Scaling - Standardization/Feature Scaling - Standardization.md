# Feature Scaling and Standardization

## What is Feature Scaling?

**Feature Scaling** is a preprocessing technique used to bring numerical features into a similar scale.

Example:

```text
Age     → 18 to 60
Salary  → 20,000 to 500,000
```

Without scaling, features with larger numerical values may dominate some machine learning algorithms.

---

## Why is Feature Scaling Needed?

Consider two features:

```text
Age = 25
Salary = 80,000
```

For distance-based algorithms, the salary value is much larger than age.

This may cause:

```text
Large-scale feature
        ↓
Dominates distance calculation
        ↓
Biased model behaviour
```

Scaling helps give features a more comparable influence.

---

# Main Types of Feature Scaling

Two common techniques are:

```text
Feature Scaling
│
├── Standardization
└── Normalization
```

This lesson mainly focuses on **Standardization**.

---

# Standardization

**Standardization** transforms a feature so that it has approximately:

- **Mean = 0**
- **Standard Deviation = 1**

It is also called **Z-score standardization**.

## Formula

\[
z = \frac{x-\mu}{\sigma}
\]

Where:

- \(x\) = original value
- \(\mu\) = mean of the feature
- \(\sigma\) = standard deviation
- \(z\) = standardized value

---

## Simple Example

Suppose:

```text
Value = 70
Mean = 50
Standard Deviation = 10
```

Then:

\[
z = \frac{70-50}{10}
\]

\[
z = 2
\]

This means the value is **2 standard deviations above the mean**.

---

# How Standardization Works

Standardization mainly performs two steps:

### 1. Mean Centering

```text
x - mean
```

This shifts the data so that the center becomes approximately:

```text
Mean = 0
```

### 2. Scaling

The centered values are divided by the standard deviation.

```text
(x - mean) / standard deviation
```

This adjusts the spread so that:

```text
Standard Deviation = 1
```

---

# Standardization Using Scikit-Learn

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)
```

---

# Correct Way: Train-Test Split First

Always split the dataset before fitting the scaler.

```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)

scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Why?

Use:

```python
scaler.fit_transform(X_train)
```

on training data.

But use only:

```python
scaler.transform(X_test)
```

on testing data.

> [!IMPORTANT]
> The test data should be transformed using the **mean and standard deviation learned from the training data**.

This helps avoid **data leakage**.

---

# Effect on Distribution

Standardization changes:

- Mean
- Scale
- Standard deviation

But it generally keeps the **overall shape of the distribution** similar.

```text
Original Distribution
       ↓
Standardization
       ↓
Same general shape
Mean ≈ 0
Std ≈ 1
```

---

# Standardization and Outliers

Standardization does **not remove outliers**.

Example:

```text
Original:
10, 12, 13, 15, 100
               ↑
             Outlier
```

After standardization, the extreme value can still remain far away from the other values.

> StandardScaler is sensitive to extreme values because the **mean and standard deviation** themselves can be affected by outliers.

---

# Algorithms Where Scaling is Important

## K-Nearest Neighbors — KNN

KNN uses distance calculations.

```text
Feature Scaling
      ↓
More meaningful distance
      ↓
Better neighbour selection
```

---

## K-Means Clustering

K-Means also uses distances between data points and cluster centers.

Therefore, scaling is usually important.

---

## PCA

**Principal Component Analysis** is influenced by feature variance.

Large-scale features can dominate PCA if data is not scaled.

---

## Gradient-Based Algorithms

Scaling can also help optimization algorithms converge efficiently.

Examples:

- Linear Regression with gradient-based optimization
- Logistic Regression
- Neural Networks

---

# Algorithms Where Scaling is Usually Not Required

Tree-based algorithms mainly make decisions such as:

```text
Age < 30?
Salary > 50,000?
```

They do not depend directly on Euclidean distance.

Examples:

- Decision Tree
- Random Forest
- Gradient Boosting Trees

Therefore, scaling is generally **not necessary** for these models.

---

# Quick Comparison

| Algorithm | Scaling Usually Needed? |
|---|---|
| KNN | Yes |
| K-Means | Yes |
| PCA | Yes |
| Logistic Regression | Often helpful |
| Neural Networks | Usually yes |
| Decision Tree | Usually no |
| Random Forest | Usually no |
| Gradient Boosting Trees | Usually no |

---

# Simple Workflow

```text
Dataset
   ↓
Train-Test Split
   ↓
Fit StandardScaler on Training Data
   ↓
Transform Training Data
   ↓
Transform Test Data
   ↓
Train Model
```

---

## 🧠 Quick Revision

- **Feature Scaling** brings numerical features to comparable scales.
- Two common methods are **Standardization** and **Normalization**.
- Standardization gives data approximately **mean = 0** and **standard deviation = 1**.
- Formula: \(z = (x-\mu)/\sigma\).
- Always **split the data before fitting the scaler**.
- Fit the scaler on **training data only**.
- Use the same scaler to transform the test data.
- Standardization does **not remove outliers**.
- Scaling is important for **KNN, K-Means, PCA, and many gradient-based models**.
- Tree-based algorithms such as **Decision Trees and Random Forests** usually do not require scaling.