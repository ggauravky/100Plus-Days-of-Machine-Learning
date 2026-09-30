# 📘 Standardization in Machine Learning

## 1. What is Feature Scaling?

**Feature Scaling** is a preprocessing technique used to bring numerical features into a similar scale.

For example:

| Feature | Range |
|---|---:|
| Age | 18 – 60 |
| Salary | ₹20,000 – ₹5,00,000 |
| Experience | 0 – 20 years |

Here, **Salary** has much larger numerical values than Age and Experience.

Some Machine Learning algorithms may give more importance to Salary simply because its numbers are larger.

Feature Scaling helps solve this problem.

---

# 2. What is Standardization?

**Standardization** is a feature scaling technique that transforms numerical values so that the data has approximately:

```text
Mean = 0

Standard Deviation = 1
```

Standardization is also called:

> **Z-Score Standardization**

---

# 3. Formula of Standardization

## ⭐ Standardization Formula

```text
             x - μ
      z = -----------
               σ
```

Or simply:

```text
Standardized Value = (Original Value - Mean) / Standard Deviation
```

Where:

```text
z = Standardized value

x = Original value

μ = Mean

σ = Standard Deviation
```

---

# 4. Formula of Mean

Before finding Standard Deviation, we first calculate the mean.

```text
         Sum of all values
Mean = ---------------------
         Number of values
```

Mathematically:

```text
        Σx
μ = --------
        n
```

---

# 5. Formula of Standard Deviation

## ⭐ Population Standard Deviation

```text
                 Σ(x - μ)²
σ = √  -------------------------
                     n
```

Or in simple words:

```text
Standard Deviation

        = Square Root of

          Sum of (Value - Mean)²
          -----------------------
          Number of Values
```

Where:

```text
σ = Standard Deviation

x = Individual value

μ = Mean

n = Number of values
```

---

# 6. Real-Life Example

Suppose we have the monthly salaries of **5 employees**.

| Employee | Salary |
|---|---:|
| A | ₹30,000 |
| B | ₹40,000 |
| C | ₹50,000 |
| D | ₹60,000 |
| E | ₹70,000 |

We want to standardize the **Salary** feature.

---

# 7. Step 1 — Calculate Mean

Salary values:

```text
30,000
40,000
50,000
60,000
70,000
```

Formula:

```text
         Sum of all salaries
Mean = -----------------------
        Number of employees
```

Calculation:

```text
30,000 + 40,000 + 50,000 + 60,000 + 70,000
------------------------------------------------
                     5
```

```text
250,000
--------- = 50,000
    5
```

Therefore:

```text
Mean (μ) = ₹50,000
```

---

# 8. Step 2 — Subtract Mean From Every Value

Formula:

```text
x - μ
```

Where:

```text
μ = 50,000
```

Now calculate:

| Employee | Salary (x) | Mean (μ) | x - μ |
|---|---:|---:|---:|
| A | 30,000 | 50,000 | -20,000 |
| B | 40,000 | 50,000 | -10,000 |
| C | 50,000 | 50,000 | 0 |
| D | 60,000 | 50,000 | 10,000 |
| E | 70,000 | 50,000 | 20,000 |

This process is called:

> **Mean Centering**

Notice:

```text
Value < Mean  → Negative

Value = Mean  → 0

Value > Mean  → Positive
```

---

# 9. Step 3 — Calculate Standard Deviation

Standard Deviation formula:

```text
                 Σ(x - μ)²
σ = √  -------------------------
                     n
```

First, square all the differences.

| x | x - μ | (x - μ)² |
|---:|---:|---:|
| 30,000 | -20,000 | 400,000,000 |
| 40,000 | -10,000 | 100,000,000 |
| 50,000 | 0 | 0 |
| 60,000 | 10,000 | 100,000,000 |
| 70,000 | 20,000 | 400,000,000 |

Add all squared differences:

```text
400,000,000
+ 100,000,000
+ 0
+ 100,000,000
+ 400,000,000
----------------
1,000,000,000
```

Now divide by the number of values:

```text
1,000,000,000
---------------- = 200,000,000
       5
```

Now take the square root:

```text
σ = √200,000,000
```

Therefore:

```text
σ ≈ 14,142.14
```

So:

```text
Standard Deviation ≈ ₹14,142.14
```

---

# 10. Step 4 — Apply Standardization Formula

Formula:

```text
             x - μ
      z = -----------
               σ
```

We already know:

```text
Mean (μ) = 50,000

Standard Deviation (σ) = 14,142.14
```

---

## Employee A

Salary:

```text
x = 30,000
```

Apply formula:

```text
      30,000 - 50,000
z = -------------------
          14,142.14
```

```text
      -20,000
z = -----------
      14,142.14
```

```text
z ≈ -1.414
```

---

## Employee B

```text
      40,000 - 50,000
z = -------------------
          14,142.14
```

```text
      -10,000
z = -----------
      14,142.14
```

```text
z ≈ -0.707
```

---

## Employee C

```text
      50,000 - 50,000
z = -------------------
          14,142.14
```

```text
z = 0
```

---

## Employee D

```text
      60,000 - 50,000
z = -------------------
          14,142.14
```

```text
      10,000
z = ----------
     14,142.14
```

```text
z ≈ 0.707
```

---

## Employee E

```text
      70,000 - 50,000
z = -------------------
          14,142.14
```

```text
      20,000
z = ----------
     14,142.14
```

```text
z ≈ 1.414
```

---

# 11. Final Standardized Table

| Employee | Original Salary | Standardized Salary |
|---|---:|---:|
| A | ₹30,000 | -1.414 |
| B | ₹40,000 | -0.707 |
| C | ₹50,000 | 0.000 |
| D | ₹60,000 | 0.707 |
| E | ₹70,000 | 1.414 |

Before Standardization:

```text
30,000
40,000
50,000
60,000
70,000
```

After Standardization:

```text
-1.414
-0.707
 0.000
 0.707
 1.414
```

The actual information has not changed.

Only the **scale of the values has changed**.

---

# 12. How to Understand Z-Score

A standardized value is also called a **Z-Score**.

### If:

```text
z = 0
```

It means:

```text
Value is equal to the Mean.
```

### If:

```text
z = 1
```

It means:

```text
Value is 1 Standard Deviation above the Mean.
```

### If:

```text
z = -1
```

It means:

```text
Value is 1 Standard Deviation below the Mean.
```

Example:

```text
₹50,000 → z = 0
```

because ₹50,000 is exactly the mean.

And:

```text
₹70,000 → z ≈ 1.414
```

means ₹70,000 is approximately **1.414 Standard Deviations above the Mean**.

---

# 13. Standardization With Multiple Features

Suppose we have:

| Person | Age | Salary |
|---|---:|---:|
| A | 20 | 30,000 |
| B | 30 | 50,000 |
| C | 40 | 70,000 |

Before scaling:

```text
Age    → 20, 30, 40

Salary → 30,000, 50,000, 70,000
```

Salary has much larger numerical values.

After Standardization, the values may become:

| Person | Standardized Age | Standardized Salary |
|---|---:|---:|
| A | -1.225 | -1.225 |
| B | 0.000 | 0.000 |
| C | 1.225 | 1.225 |

Now both features are on a comparable scale.

---

# 14. Does Standardization Convert Values Between 0 and 1?

**No.**

Standardization does not normally produce values between `0` and `1`.

For example:

```text
-1.414

-0.707

0

0.707

1.414
```

Negative values are completely normal.

The main goal is:

```text
Mean ≈ 0

Standard Deviation ≈ 1
```

---

# 15. Standardization vs Normalization

| Standardization | Normalization |
|---|---|
| Mean becomes approximately 0 | Values usually become 0 to 1 |
| Standard Deviation becomes approximately 1 | Uses minimum and maximum |
| Uses Mean and Standard Deviation | Uses Min and Max |
| Negative values are possible | Usually values remain between 0 and 1 |

## Standardization Formula

```text
             x - μ
      z = -----------
               σ
```

## Min-Max Normalization Formula

```text
             x - x(min)
x(new) = -------------------
          x(max) - x(min)
```

### Easy Memory Trick

```text
Standardization
       ↓
Mean = 0
Standard Deviation = 1
```

```text
Normalization
       ↓
Usually Range = 0 to 1
```

---

# 16. Standardization and Outliers

Standardization **does not remove outliers**.

Example:

```text
10, 12, 13, 15, 100
```

Here:

```text
100
```

is an outlier.

Even after Standardization, `100` will remain relatively far from the other values.

This happens because StandardScaler uses:

```text
Mean

and

Standard Deviation
```

Both can be affected by extreme values.

Therefore:

> **StandardScaler is sensitive to outliers.**

---

# 17. Algorithms Where Standardization is Important

| Algorithm | Scaling | Reason |
|---|---|---|
| KNN | ✅ Yes | Uses distance |
| K-Means | ✅ Yes | Uses distance |
| PCA | ✅ Yes | Depends on variance |
| SVM | ✅ Usually | Sensitive to scale |
| Logistic Regression | ✅ Helpful | Improves optimization |
| Neural Networks | ✅ Usually | Helps gradient-based training |

---

# 18. Algorithms Where Scaling is Usually Not Required

Tree-based algorithms usually do not require Standardization.

Examples:

```text
Decision Tree

Random Forest

Gradient Boosting Trees
```

A Decision Tree works using conditions like:

```text
Age < 30?
```

or:

```text
Salary > 50,000?
```

It mainly cares about the ordering and split points.

Therefore, changing the scale generally does not affect it much.

---

# 19. Correct Workflow

Always perform **Train-Test Split before fitting the scaler**.

```text
Dataset
   ↓
Separate Features (X) and Target (y)
   ↓
Train-Test Split
   ↓
Fit StandardScaler on X_train
   ↓
Transform X_train
   ↓
Transform X_test using the SAME scaler
   ↓
Train ML Model
```

Remember:

```text
Training Data
    ↓
fit_transform()
```

```text
Testing Data
    ↓
transform()
```

The scaler should learn:

```text
Mean

and

Standard Deviation
```

only from the **training data**.

This prevents:

> **Data Leakage**

---

# 20. Python Code

```python
import pandas as pd
from sklearn.preprocessing import StandardScaler

data = {
    "Salary": [30000, 40000, 50000, 60000, 70000]
}

df = pd.DataFrame(data)

scaler = StandardScaler()

df["Standardized_Salary"] = scaler.fit_transform(
    df[["Salary"]]
)

print(df)
```

## Output

```text
   Salary  Standardized_Salary
0   30000            -1.414214
1   40000            -0.707107
2   50000             0.000000
3   60000             0.707107
4   70000             1.414214
```

The output is the same as our manual calculation.

---

# 21. StandardScaler With Train-Test Split

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

Important:

```text
X_train → fit_transform()

X_test  → transform()
```

---

# 22. ⭐ Important Formulas

## Mean

```text
        Σx
μ = --------
        n
```

---

## Standard Deviation

```text
                 Σ(x - μ)²
σ = √  -------------------------
                     n
```

---

## Standardization

```text
             x - μ
      z = -----------
               σ
```

---

# 🧠 Quick Revision

```text
Feature Scaling
      ↓
Brings numerical features to comparable scales
```

```text
Standardization
      ↓
Subtract Mean
      ↓
Divide by Standard Deviation
```

```text
Mean ≈ 0

Standard Deviation ≈ 1
```

### Formula

```text
             x - μ
      z = -----------
               σ
```

### Remember

```text
Standardization
= Subtract Mean
+ Divide by Standard Deviation
```

> **Standardization = Mean 0 + Standard Deviation 1**