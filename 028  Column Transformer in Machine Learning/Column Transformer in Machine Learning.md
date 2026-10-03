# ColumnTransformer in Scikit-Learn

## What is ColumnTransformer?

**ColumnTransformer** is a Scikit-Learn utility used to apply **different preprocessing techniques to different columns** of a dataset.

Example:

```text
Age        → Missing Value Imputation
Education  → Ordinal Encoding
City       → One-Hot Encoding
```

Instead of transforming each column separately and manually combining everything, `ColumnTransformer` performs all transformations in one place.

---

## Why Do We Need It?

A dataset may contain:

- Numerical columns
- Ordinal categorical columns
- Nominal categorical columns
- Missing values

Manual preprocessing:

```text
Imputer
   ↓
OrdinalEncoder
   ↓
OneHotEncoder
   ↓
Manually concatenate arrays
```

This becomes:

- Lengthy
- Difficult to manage
- Error-prone

With `ColumnTransformer`:

```text
Different Columns
      ↓
ColumnTransformer
      ↓
Single Transformed Dataset
```

---

# Basic Syntax

```python
from sklearn.compose import ColumnTransformer

transformer = ColumnTransformer(
    transformers=[
        ("name", transformer_object, ["column"])
    ]
)
```

Each transformer is written as:

```text
(Name, Transformer, Columns)
```

---

# Practical Example

Suppose we have:

| Age | Fever | Education | City |
|---:|---|---|---|
| 25 | 100 | Graduate | Delhi |
| NaN | 98 | School | Mumbai |
| 32 | 102 | Postgraduate | Lucknow |

We want:

```text
Age       → Fill missing values
Education → Ordinal Encoding
City      → One-Hot Encoding
```

---

## Import Required Classes

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OrdinalEncoder, OneHotEncoder
```

---

## Create ColumnTransformer

```python
transformer = ColumnTransformer(
    transformers=[
        (
            "age_imputer",
            SimpleImputer(strategy="mean"),
            ["Age"]
        ),

        (
            "education_encoder",
            OrdinalEncoder(
                categories=[
                    ["School", "Graduate", "Postgraduate"]
                ]
            ),
            ["Education"]
        ),

        (
            "city_encoder",
            OneHotEncoder(
                handle_unknown="ignore"
            ),
            ["City"]
        )
    ],
    remainder="passthrough"
)
```

Then:

```python
X_transformed = transformer.fit_transform(X)
```

---

# Understanding `remainder`

Columns not mentioned inside `ColumnTransformer` can be handled using `remainder`.

## Drop Remaining Columns

```python
remainder="drop"
```

This is the default behavior.

Unspecified columns are removed.

---

## Keep Remaining Columns

```python
remainder="passthrough"
```

Unspecified columns remain unchanged.

Example:

```text
Fever
```

If no transformer is applied to `Fever`, `passthrough` keeps it in the final dataset.

---

# Multiple Transformations Together

```text
Original Dataset
      ↓
┌──────────────────────────┐
│ ColumnTransformer        │
│                          │
│ Age → SimpleImputer      │
│ Education → Ordinal      │
│ City → OneHotEncoder     │
│ Fever → Passthrough      │
└──────────────────────────┘
      ↓
Final Numerical Dataset
```

---

# Train-Test Rule

First split the data:

```python
from sklearn.model_selection import train_test_split

X_train, X_test = train_test_split(
    X,
    test_size=0.2,
    random_state=42
)
```

Then:

```python
X_train_transformed = transformer.fit_transform(X_train)
X_test_transformed = transformer.transform(X_test)
```

> [!IMPORTANT]
> Fit the transformer only on **training data** to avoid **data leakage**.

---

# Manual Approach vs ColumnTransformer

| Manual Approach | ColumnTransformer |
|---|---|
| Separate code for each column | All transformations together |
| Manual concatenation required | Automatically combines results |
| More code | Cleaner code |
| More chance of mistakes | Easier to maintain |
| Harder for pipelines | Works directly with pipelines |

---

# Works Well with Pipelines

`ColumnTransformer` is commonly combined with **Scikit-Learn Pipeline**.

```text
Raw Data
   ↓
ColumnTransformer
   ↓
Preprocessing
   ↓
Machine Learning Model
```

Example workflow:

```text
Missing Value Handling
       ↓
Categorical Encoding
       ↓
Feature Scaling
       ↓
Model Training
```

All of these steps can later be organized inside a single **Pipeline**.

---

## 🧠 Quick Revision

- **ColumnTransformer** applies different transformations to different columns.
- It reduces manual preprocessing code.
- Each transformation is defined as:
  `("name", transformer, columns)`.
- Use `SimpleImputer` for missing values.
- Use `OrdinalEncoder` for ordered categorical data.
- Use `OneHotEncoder` for nominal categorical data.
- `remainder="drop"` removes unspecified columns.
- `remainder="passthrough"` keeps unspecified columns.
- Use `fit_transform()` on training data.
- Use only `transform()` on test data.
- `ColumnTransformer` works very well with **Scikit-Learn Pipelines**.