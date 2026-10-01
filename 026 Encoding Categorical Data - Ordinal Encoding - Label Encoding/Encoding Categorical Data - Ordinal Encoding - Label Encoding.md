# Ordinal Encoding and Label Encoding

Machine learning models mainly work with **numerical data**, so categorical values often need to be converted into numbers.

---

# Types of Categorical Data

Categorical data is mainly divided into:

```text
Categorical Data
│
├── Nominal
└── Ordinal
```

## 1. Nominal Data

Categories have **no natural order**.

Examples:

```text
City   → Lucknow, Delhi, Mumbai
Branch → CSE, IT, ECE
Color  → Red, Blue, Green
```

For nominal data, **One-Hot Encoding** is commonly used.

---

## 2. Ordinal Data

Categories have a meaningful **order or hierarchy**.

Examples:

```text
Poor < Average < Good < Excellent
```

or:

```text
High School < Bachelor's < Master's < PhD
```

For such features, **Ordinal Encoding** is useful.

---

# Ordinal Encoding

**Ordinal Encoding** converts ordered categorical input features into numerical values while preserving their order.

Example:

```text
Poor      → 0
Average   → 1
Good      → 2
Excellent → 3
```

The important point is that:

```text
Poor < Average < Good < Excellent
```

must remain meaningful after encoding.

---

## Using Scikit-Learn

```python
from sklearn.preprocessing import OrdinalEncoder

encoder = OrdinalEncoder(
    categories=[
        ["Poor", "Average", "Good", "Excellent"]
    ]
)

X_train = encoder.fit_transform(X_train)
X_test = encoder.transform(X_test)
```

> [!IMPORTANT]
> The category order should be specified correctly according to the real-world hierarchy.

---

# Example with Education

```text
High School
Bachelor's
Master's
PhD
```

Encoding:

```text
High School → 0
Bachelor's  → 1
Master's    → 2
PhD         → 3
```

This preserves:

```text
High School < Bachelor's < Master's < PhD
```

---

# Label Encoding

**Label Encoding** is commonly used to convert the **target variable (`y`)** in classification problems into numerical class labels.

Example:

```text
Purchased
Yes
No
Yes
```

can become:

```text
No  → 0
Yes → 1
```

---

## Using LabelEncoder

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

y_train = encoder.fit_transform(y_train)
y_test = encoder.transform(y_test)
```

Example:

```text
Original:
Yes, No, Yes, No

Encoded:
1, 0, 1, 0
```

For classification targets, these numbers represent **class labels**, not numerical magnitude.

---

# Ordinal Encoding vs Label Encoding

| Ordinal Encoding | Label Encoding |
|---|---|
| Usually used on input features `X` | Usually used on target `y` |
| Used for ordered categories | Used for class labels |
| Order must be specified correctly | Converts classes into integers |
| Example: Poor → Excellent | Example: No → 0, Yes → 1 |

---

# Train-Test Split

Perform the split **before fitting the encoder**.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

Then:

```text
Training Data
      ↓
Fit Encoder
      ↓
Transform Training Data
      ↓
Transform Test Data
```

> [!IMPORTANT]
> Fit the encoder only on the **training data** to avoid **data leakage**.

---

# Which Encoding Should You Use?

```text
Categorical Variable
       ↓
Does it have natural order?
       ↓
     Yes ──→ Ordinal Encoding
       │
       No ──→ One-Hot Encoding
```

For target labels:

```text
Classification Target y
       ↓
Label Encoding
```

---

## 🧠 Quick Revision

- Categorical data can be **nominal** or **ordinal**.
- **Nominal** → no meaningful order.
- **Ordinal** → meaningful hierarchy.
- **OrdinalEncoder** is mainly used for ordered input features `X`.
- Category order must be defined correctly.
- **LabelEncoder** is commonly used for classification target `y`.
- For nominal input features, **One-Hot Encoding** is generally preferred.
- Perform **train-test split before fitting encoders**.
- Fit encoders on training data and use the same fitted encoder on test data.