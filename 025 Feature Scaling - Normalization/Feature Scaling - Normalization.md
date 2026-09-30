# Normalization in Machine Learning

## What is Normalization?

**Normalization** is a feature scaling technique used to bring numerical features into a common range.

### Main Goal

- Remove the effect of different units.
- Prevent large-magnitude features from dominating.
- Make features easier to compare.

### Example

```text
Weight   → 70 kg
Distance → 5000 meters
```

Different scales can affect some machine learning algorithms.

---

# 1. Min-Max Normalization

**Min-Max Scaling** converts values into the range:

```text
0 to 1
```

## Formula

$$
X_{new} = \frac{X - X_{min}}{X_{max} - X_{min}}
$$

Where:

- $X$ = original value
- $X_{min}$ = minimum value
- $X_{max}$ = maximum value

### Example

Suppose:

```text
Minimum = 10
Maximum = 50
Value   = 30
```

Then:

$$
X_{new}
=
\frac{30 - 10}{50 - 10}
=
\frac{20}{40}
=
0.5
$$

So:

```text
30 → 0.5
```

---

## Using Scikit-Learn

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

> **Important**
>
> First perform the **train-test split**.
>
> Fit the scaler only on the **training data**, then use the same scaler on the test data.

---

# 2. Mean Normalization

**Mean Normalization** centers the data around the mean.

## Formula

$$
X_{new}
=
\frac{X - \text{Mean}}
{X_{max} - X_{min}}
$$

The resulting values are generally centered around:

```text
0
```

and often lie roughly between:

```text
-1 to 1
```

Useful when centered data is required.

---

# 3. Max-Abs Scaling

Each value is divided by the **maximum absolute value**.

## Formula

$$
X_{new} = \frac{X}{|X_{max}|}
$$

### Example

```text
Original → [-10, 0, 5, 10]

Scaled   → [-1, 0, 0.5, 1]
```

Useful for **sparse datasets** containing many zeros.

---

# 4. Robust Scaling

**Robust Scaling** uses:

- Median
- Interquartile Range (**IQR**)

instead of mean and standard deviation.

## Formula

$$
X_{new}
=
\frac{X - \text{Median}}
{IQR}
$$

where:

```text
IQR = Q3 - Q1
```

### Main Advantage

It is less affected by **outliers**.

### Example

```text
10, 12, 13, 15, 500
                ↑
             Outlier
```

Min-Max Scaling may be strongly affected by `500`, while Robust Scaling handles such extreme values better.

---

# Normalization vs Standardization

| Normalization | Standardization |
|---|---|
| Usually scales values to a fixed range | Centers values around 0 |
| Min-Max commonly gives `0–1` | Mean ≈ `0` |
| Uses minimum and maximum | Uses mean and standard deviation |
| Useful when bounds are meaningful | Common default for many ML problems |
| Sensitive to outliers | Also affected by outliers |

---

# When to Use Min-Max Scaling

Useful when minimum and maximum values are naturally known.

### Example: Images

Pixel intensity:

```text
0 to 255
```

Normalize using:

```text
Pixel / 255
```

Then:

```text
0   → 0
128 → ~0.50
255 → 1
```

This converts pixel values into:

```text
0 to 1
```

---

# Scaling Techniques Summary

| Technique | Main Idea | Common Use |
|---|---|---|
| **Min-Max Scaling** | Scale to `0–1` | Known feature bounds |
| **Mean Normalization** | Center around mean | Centered data |
| **Max-Abs Scaling** | Divide by max absolute value | Sparse data |
| **Robust Scaling** | Uses median and IQR | Data with outliers |

---

# Simple Workflow

```text
Dataset
   ↓
Train-Test Split
   ↓
Choose Scaling Method
   ↓
Fit Scaler on Training Data
   ↓
Transform Training Data
   ↓
Transform Test Data
   ↓
Train Model
```

---

## 🧠 Quick Revision

- **Normalization** brings features to a comparable scale.
- **Min-Max Scaling** usually maps values between `0` and `1`.
- Formula: `(X - min) / (max - min)`.
- **Mean Normalization** centers data using the mean.
- **Max-Abs Scaling** is useful for sparse datasets.
- **Robust Scaling** uses median and IQR and works better with outliers.
- Perform **train-test split before scaling**.
- Fit the scaler only on the **training data**.
- **Standardization** is often a good default.
- **Min-Max Scaling** is useful when feature bounds are meaningful, such as image pixel values.
- Try different scaling methods and compare model performance.