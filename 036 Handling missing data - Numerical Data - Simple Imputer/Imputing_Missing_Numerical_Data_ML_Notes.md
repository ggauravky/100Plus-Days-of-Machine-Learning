# Handling Missing Numerical Data — Imputation

> **Machine Learning · Feature Engineering · Missing Value Treatment**  
> **Focus:** Mean, Median, Arbitrary Value, and End-of-Distribution Imputation

## 1. What Is Missing-Value Imputation?

- **Imputation** means replacing missing values (`NaN` / `None`) with estimated or chosen values instead of deleting those rows.
- It helps prepare data for ML algorithms that cannot directly accept missing values.
- The best method depends on the **feature distribution**, **amount of missingness**, **reason values are missing**, and **model performance**.

```text
Numerical Feature with Missing Values
                  |
                  v
          Choose an Approach
                  |
        +---------+----------+
        |                    |
        v                    v
   Univariate           Multivariate
   Imputation           Imputation
        |                    |
  One column's         Information from
  own values           multiple columns
        |                    |
  Mean / Median        KNN / Iterative
  Constant / Extreme   Imputation
```

### Univariate vs. Multivariate Imputation

| Univariate | Multivariate |
|---|---|
| Uses information from **one feature** | Uses information from **multiple features** |
| Simple and fast | Often more computationally expensive |
| Mean, median, constant, extreme-value imputation | KNN imputation, iterative imputation |
| May ignore feature relationships | Can use relationships between features |

> [!IMPORTANT]
> **Never calculate imputation values from the test set.** Learn them on the **training set only**, then reuse the same values for validation, testing, and prediction. A Scikit-learn `Pipeline` helps prevent data leakage.

---

## 2. Mean Imputation

### Definition

Replace missing values with the **mean (arithmetic average)** of the observed values in that column.

### Formula

$$
\boxed{\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i}
$$

Where:

- $x_i$ = each **non-missing** observed value
- $n$ = number of **non-missing** observations
- $\bar{x}$ = mean used to fill missing entries

### Example

```text
Original:       [20, 25, 30, NaN, 95]
Mean:           (20 + 25 + 30 + 95) / 4 = 42.5
After impute:   [20, 25, 30, 42.5, 95]
```

### When to Use

- Often useful for an **approximately symmetric** numerical feature without major outliers.
- Fast and simple, including at deployment time.

### Limitations

- **Sensitive to outliers:** A very large or small value shifts the mean.
- **Reduces variation:** Repeating the mean places extra observations at the center.
- Can distort the distribution and relationships with other features.

---

## 3. Median Imputation

### Definition

Replace missing values with the **median**, the middle value after sorting the observed data.

### Formula

For sorted non-missing values $x_{(1)}, x_{(2)}, \ldots, x_{(n)}$:

$$
\boxed{
\operatorname{Median}(x)=
\begin{cases}
x_{((n+1)/2)}, & n\text{ is odd}\\[5pt]
\dfrac{x_{(n/2)}+x_{(n/2+1)}}{2}, & n\text{ is even}
\end{cases}
}
$$

### Example

```text
Original:       [20, 25, 30, NaN, 95]
Sorted known:   [20, 25, 30, 95]
Median:         (25 + 30) / 2 = 27.5
After impute:   [20, 25, 30, 27.5, 95]
```

### When to Use

- Usually preferred for **skewed numerical distributions** or when outliers are present.
- Less affected by extreme values than the mean.

### Limitations

- Adds an artificial concentration of values at the median.
- Can still change the distribution, variance, covariance, and correlation.
- May remove useful information carried by the fact that a value was missing.

### Mean vs. Median

| Criterion | Mean | Median |
|---|---|---|
| Typical use | Roughly symmetric data | Skewed data / outliers |
| Affected by extreme values | **Strongly** | **Less** |
| Easy to implement | Yes | Yes |
| Can distort the distribution | Yes | Yes |
| Guaranteed to improve accuracy | No | No |

> [!TIP]
> **Rule of thumb:** Try **mean for symmetric data** and **median for skewed data**. Treat this as a starting point, not a fixed rule: compare validation scores.

---

## 4. How Imputation Affects the Data

Imputation is not simply filling empty cells: it can change statistical properties.

### A. Variance

Variance describes how spread out values are around their mean.

$$
\boxed{\sigma^2 = \frac{1}{n}\sum_{i=1}^{n}(x_i-\mu)^2}
$$

- **Mean imputation** usually decreases population variance because the inserted values lie at the mean.
- **Median imputation** can also alter variance; the direction and size depend on the data.
- **Extreme-value imputation** can increase variance substantially.

**Tiny example (population variance):**

```text
Before:              [10, 20, 30]      Variance = 66.67
Add missing -> mean:  [10, 20, 20, 30]  Variance = 50.00
```

### B. Distribution / PDF

- Mean and median imputation create an extra **spike** near the chosen replacement value.
- Arbitrary or extreme values can create a separate spike far from the original data.
- Compare **histograms** or **KDE/PDF plots** before and after imputation.

### C. Covariance and Correlation

**Covariance:** Indicates how two numerical features vary together.

$$
\boxed{\operatorname{Cov}(X,Y)=E[(X-\mu_X)(Y-\mu_Y)]}
$$

**Correlation:** Standardized measure of their linear relationship.

$$
\boxed{\rho_{X,Y}=\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y}}
$$

- Filling one column without using the other can **weaken or change relationships** between features.
- Compare relationships before and after imputation, not just individual distributions.
- Remember: correlations calculated on incomplete data may use a different set of observed pairs.

---

## 5. Arbitrary Value Imputation

### Definition

Replace missing values with a **fixed constant** chosen by us, such as `-1`, `-999`, or `99`.

```text
Original:       [21, NaN, 35, 42, NaN]
Fill with -1:   [21, -1, 35, 42, -1]
```

### Why Use It?

- A distinct value can let the model recognize **which observations were missing**.
- It may be useful when missingness itself contains information about the target.
- It does **not** prove the data is Missing Not At Random (MNAR); the missingness mechanism should be investigated separately.

### Advantages

- Very simple and fast.
- Keeps all rows.
- Can make missing observations visibly different from observed ones.

### Limitations

- May create **artificial outliers** and strongly distort the distribution.
- Can increase variance and change covariance/correlation.
- If the chosen number is a real possible value, **missing and observed values become indistinguishable**.
- Some algorithms may interpret the artificial number as a genuine measurement.

> [!IMPORTANT]
> **Often safer:** Impute with mean/median **and add a missing indicator** (`1` = originally missing, `0` = observed). This preserves the missingness signal without forcing an extreme numerical value into the feature.

---

## 6. End-of-Distribution Imputation

### Definition

An alternative to arbitrary-value imputation: fill missing entries with a value **near or beyond the extremes** of the observed distribution so the model may distinguish them.

### A. Approximately Normal Data

A commonly used heuristic is:

$$
\boxed{x_{\text{fill}} = \mu + 3\sigma}
$$

Sometimes the lower end is used:

$$
\boxed{x_{\text{fill}} = \mu - 3\sigma}
$$

Where:

- $\mu$ = mean of the observed training values
- $\sigma$ = standard deviation of the observed training values

**Example:** If $\mu=50$ and $\sigma=10$:

$$
 x_{\text{fill}}=50+3(10)=\boxed{80}
$$

### B. Skewed Data (IQR-Based Rule)

First calculate the **Interquartile Range (IQR)**:

$$
\boxed{\operatorname{IQR}=Q_3-Q_1}
$$

Common outer-fence candidates:

$$
\boxed{\text{Upper fence}=Q_3+1.5\operatorname{IQR}}
$$

$$
\boxed{\text{Lower fence}=Q_1-1.5\operatorname{IQR}}
$$

**Example:** If $Q_1=10$ and $Q_3=30$:

$$
\operatorname{IQR}=30-10=20
$$

$$
\text{Upper fence}=30+1.5(20)=\boxed{60}
$$

**Important distinctions:**

- These are **heuristics**, not guaranteed optimal imputation values.
- A fence may still lie inside the range of observed values, so it may not uniquely identify missingness.
- Some approaches use more extreme multiples; select them deliberately rather than mechanically.
- Fit the mean, standard deviation, and quartiles **only on training data**.

### Limitations

- Can create artificial outliers or exaggerate them.
- Often changes variance and feature relationships substantially.
- May be harmful for linear models sensitive to numerical magnitudes.

> [!NOTE]
> The video summary omitted the actual end-of-distribution equations. The **$\mu\pm3\sigma$** and **$Q_1-1.5\operatorname{IQR}$ / $Q_3+1.5\operatorname{IQR}$** expressions above are standard *illustrative heuristics*, not a verified quotation of the video's exact formulas.

---

## 7. Python Implementation — Pandas

Use a tiny Titanic-style example:

```python
import numpy as np
import pandas as pd

X = pd.DataFrame({
    "Age":  [22, 38, np.nan, 35, 28, np.nan, 54, 19, 41, 30, 27, 45],
    "Fare": [7.25, 71.28, 8.05, np.nan, 13.0, 512.33,
             26.0, np.nan, 10.5, 14.0, 7.75, 80.0],
})

# Check missing values
print(X.isna().sum())
print((X.isna().mean() * 100).round(2))  # % missing per column
```

### A. Mean and Median Imputation

```python
# Demonstration only: full-dataset statistics are fine for exploration,
# but train a predictive model using train-only statistics (see below).
mean_age = X["Age"].mean()
median_fare = X["Fare"].median()

X_basic = X.copy()
X_basic["Age"] = X_basic["Age"].fillna(mean_age)
X_basic["Fare"] = X_basic["Fare"].fillna(median_fare)
```

### B. Arbitrary Value Imputation

```python
X_constant = X.copy()
X_constant["Age_was_missing"] = X_constant["Age"].isna().astype(int)
X_constant["Age"] = X_constant["Age"].fillna(-1)
```

### C. End-of-Distribution Imputation

```python
X_extreme = X.copy()

# Normal-style heuristic for Age
age_upper = X["Age"].mean() + 3 * X["Age"].std()
X_extreme["Age"] = X_extreme["Age"].fillna(age_upper)

# IQR-style heuristic for skewed Fare
q1 = X["Fare"].quantile(0.25)
q3 = X["Fare"].quantile(0.75)
iqr = q3 - q1
fare_upper = q3 + 1.5 * iqr
X_extreme["Fare"] = X_extreme["Fare"].fillna(fare_upper)
```

**Note:** These Pandas examples introduce the operations. For actual ML training, calculate replacement values using **training data only**.

---

## 8. Scikit-Learn — SimpleImputer and ColumnTransformer

### A. Use Different Strategies on Different Features

```python
from sklearn.model_selection import train_test_split
from sklearn.impute import SimpleImputer
from sklearn.compose import ColumnTransformer
import pandas as pd

# Split BEFORE learning replacement values
X_train, X_test = train_test_split(
    X, test_size=0.25, random_state=42
)

preprocessor = ColumnTransformer(
    transformers=[
        ("mean_age", SimpleImputer(strategy="mean"), ["Age"]),
        ("median_fare", SimpleImputer(strategy="median"), ["Fare"]),
    ],
    verbose_feature_names_out=False,
)

# Fit on training data; apply same learned values to test data
train_array = preprocessor.fit_transform(X_train)
test_array = preprocessor.transform(X_test)

X_train_clean = pd.DataFrame(
    train_array, columns=preprocessor.get_feature_names_out(),
    index=X_train.index
)
X_test_clean = pd.DataFrame(
    test_array, columns=preprocessor.get_feature_names_out(),
    index=X_test.index
)

print(X_train_clean.isna().sum())
```

### B. Constant Value with SimpleImputer

```python
constant_imputer = SimpleImputer(strategy="constant", fill_value=-1)

X_train_constant = constant_imputer.fit_transform(X_train[["Age"]])
X_test_constant = constant_imputer.transform(X_test[["Age"]])
```

### C. Add a Missing Indicator

```python
# A useful alternative to an extreme sentinel
indicator_imputer = SimpleImputer(
    strategy="median",
    add_indicator=True,
)

X_train_with_flag = indicator_imputer.fit_transform(X_train[["Age"]])
X_test_with_flag = indicator_imputer.transform(X_test[["Age"]])

print(indicator_imputer.get_feature_names_out(["Age"]))
```

- `add_indicator=True` adds a binary column for features found missing **during fitting**.
- If a feature had no missing values in training but has some later, this option does not automatically create a new indicator for it.
- In a complete model workflow, put the preprocessing step inside an ML **Pipeline**, so each cross-validation fold learns its own training statistics.

---

## 9. How to Check Whether Imputation Helped

### Statistical and Visual Checks

1. Calculate **% missing** per column.
2. Inspect the original shape (histogram/KDE), skewness, and outliers.
3. Apply one imputation strategy using training data only.
4. Compare **mean, median, variance, and feature correlations** before and after.
5. Train and compare models using **cross-validation** (fit imputation inside each fold).
6. Keep the method that performs well **without creating unrealistic feature values**.

```python
# Compare distribution statistics (exploratory example)
comparison = pd.DataFrame({
    "Original Age": X["Age"].describe(),
    "Mean-imputed Age": X_basic["Age"].describe(),
    "Extreme-imputed Age": X_extreme["Age"].describe(),
})
print(comparison)

# Compare correlation matrices
print("Before:\n", X.corr())
print("After mean/median:\n", X_basic.corr())
```

### Distribution Plot

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.histplot(X["Fare"], stat="density", color="steelblue",
             alpha=0.4, label="Original")
sns.histplot(X_basic["Fare"], stat="density", color="darkorange",
             alpha=0.4, label="Median imputation")
plt.legend()
plt.title("Fare: Before vs After Imputation")
plt.show()
```

> [!WARNING]
> **A natural-looking histogram is not the same as better predictive performance.** Use validation metrics and check whether missingness itself is informative.

---

## 10. Quick Comparison

| Method | Replacement Value | Often Useful When | Main Risk |
|---|---|---|---|
| **Mean** | Observed mean | Roughly symmetric feature | Outlier sensitivity; reduced variance |
| **Median** | Observed median | Skewed feature / outliers | Artificial spike near median |
| **Arbitrary** | Fixed constant (e.g., `-1`) | Explicit missingness signal may matter | Artificial outliers; value collision |
| **End-of-distribution** | A chosen extreme / fence | Want missing values separated from typical values | Strong distortion of scale, variance, and relationships |
| **Mean/median + indicator** | Central value + 0/1 flag | Missingness may contain predictive information | Extra feature; not always helpful |

---

## 🧠 Quick Revision

- **Imputation** fills missing values instead of dropping rows.
- **Univariate** uses one column; **multivariate** uses several columns.
- **Mean** suits approximately symmetric data; **median** often suits skewed data.
- Mean/median can change **variance, PDF, covariance, and correlation**.
- **Arbitrary-value imputation** uses a constant but may introduce artificial outliers.
- **End-of-distribution imputation** uses an extreme-value heuristic, such as $\mu+3\sigma$ or $Q_3+1.5\operatorname{IQR}$.
- `SimpleImputer` handles common strategies; `ColumnTransformer` applies different strategies to different columns.
- **Always fit preprocessing on training data only**, and compare results using cross-validation.

> **Remember:** The best imputation technique is the one that handles missing values reliably and **improves or preserves performance on unseen data**—not necessarily the one that makes a chart look most normal.
