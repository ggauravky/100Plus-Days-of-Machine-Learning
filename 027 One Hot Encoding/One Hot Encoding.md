# One-Hot Encoding (OHE)

## What is One-Hot Encoding?

**One-Hot Encoding** converts **nominal categorical data** into numerical form so machine learning algorithms can use it.

Example:

```text
Color
Red
Blue
Green
```

becomes:

| Red | Blue | Green |
|---:|---:|---:|
| 1 | 0 | 0 |
| 0 | 1 | 0 |
| 0 | 0 | 1 |

Each category gets its own **binary column** containing `0` or `1`.

---

# Nominal vs Ordinal Data

## Ordinal Data

Categories have a meaningful order.

```text
Poor < Average < Good < Excellent
```

Use:

**Ordinal Encoding**

## Nominal Data

Categories have **no natural order**.

Examples:

```text
City  → Delhi, Lucknow, Mumbai
Color → Red, Blue, Green
Brand → BMW, Audi, Tata
```

Use:

**One-Hot Encoding**

---

# Why Not Simply Assign Numbers?

Suppose:

```text
Red   → 1
Blue  → 2
Green → 3
```

The model may incorrectly interpret:

```text
Green > Blue > Red
```

But colors have **no actual order**.

OHE avoids this problem.

---

# Number of Columns

If a categorical feature contains:

```text
n categories
```

OHE normally creates:

```text
n binary columns
```

Example:

```text
City = Delhi, Mumbai, Lucknow
```

creates:

```text
Delhi
Mumbai
Lucknow
```

---

# Dummy Variable Trap

Suppose:

```text
Red   = [1, 0, 0]
Blue  = [0, 1, 0]
Green = [0, 0, 1]
```

One column can be determined from the others.

This can create **multicollinearity**, especially in some linear models.

A common solution is to drop one column:

```text
n categories
     ↓
Keep n - 1 columns
```

Example:

```text
Red  → [1, 0]
Blue → [0, 1]
Green → [0, 0]
```

The dropped category is still represented by all zeros.

---

# Using Pandas

```python
import pandas as pd

pd.get_dummies(df["Color"])
```

To drop the first category:

```python
pd.get_dummies(
    df["Color"],
    drop_first=True
)
```

This is useful for quick data analysis and experimentation.

---

# Using Scikit-Learn

For machine learning workflows, use:

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(
    drop="first",
    sparse_output=False,
    handle_unknown="ignore"
)

X_train_encoded = encoder.fit_transform(X_train)
X_test_encoded = encoder.transform(X_test)
```

### Important Parameters

- `drop="first"` → drops one category.
- `sparse_output=False` → returns a normal dense array.
- `handle_unknown="ignore"` → prevents errors when unseen categories appear during transformation.

---

# Train-Test Rule

Always:

```text
Train-Test Split
      ↓
Fit Encoder on X_train
      ↓
Transform X_train
      ↓
Transform X_test
```

Do:

```python
encoder.fit_transform(X_train)
encoder.transform(X_test)
```

Do **not** separately fit the encoder on test data.

This helps prevent **data leakage** and keeps category mapping consistent.

---

# High Cardinality Problem

**High cardinality** means a feature contains many unique categories.

Example:

```text
Car Brand
↓
BMW
Audi
Tata
Toyota
Honda
Ford
...
100+ brands
```

OHE could create hundreds of columns.

This causes:

- Higher memory usage
- More computation
- Increased dimensionality

---

## Handling Rare Categories

Rare categories can be grouped into:

```text
Other
```

Example:

```text
BMW      → BMW
Tata     → Tata
Toyota   → Toyota
RareBrand1 → Other
RareBrand2 → Other
```

Then OHE is applied.

This reduces unnecessary columns.

---

# Pandas vs Scikit-Learn

| Pandas | Scikit-Learn |
|---|---|
| `pd.get_dummies()` | `OneHotEncoder()` |
| Simple and quick | Better for ML pipelines |
| Good for EDA | Remembers learned categories |
| `drop_first=True` | `drop="first"` |
| Less convenient for production pipelines | Works well with `ColumnTransformer` |

---

# Simple Workflow

```text
Categorical Feature
        ↓
Is there an order?
   ↓           ↓
  Yes          No
   ↓           ↓
Ordinal       One-Hot
Encoding      Encoding
                  ↓
          Handle rare categories
                  ↓
             Train Model
```

---

## 🧠 Quick Revision

- **One-Hot Encoding** is mainly used for **nominal categorical features**.
- It creates separate `0/1` columns for categories.
- It avoids creating a false numerical order between categories.
- `n` categories normally produce **n binary columns**.
- Dropping one column gives **n - 1 columns** and can avoid the dummy variable trap in linear models.
- `pd.get_dummies()` provides quick Pandas-based encoding.
- `OneHotEncoder` is preferred for proper ML pipelines.
- Fit the encoder on **training data only**.
- **High cardinality** can create too many columns.
- Rare categories can be grouped into an **Other** category.
- `ColumnTransformer` can later be used to apply different transformations to different columns cleanly.