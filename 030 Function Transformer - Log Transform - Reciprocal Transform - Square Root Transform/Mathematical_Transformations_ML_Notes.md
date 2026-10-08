# Mathematical Transformations in Machine Learning

> **Part of:** Feature Engineering → Numerical Feature Transformation  
> **Goal:** Change the shape or scale of numerical features when it helps a model learn.

## 1. What Are Mathematical Transformations?

- A **mathematical transformation** applies a function (such as `log`, `sqrt`, or `x²`) to a numerical feature.
- Common reasons to transform data:
  - **Reduce skewness**, especially a long right tail.
  - **Limit the influence of extreme values**.
  - **Stabilize variance** or make a relationship easier to model.
  - Sometimes make data **closer to a normal (Gaussian) distribution**.

> [!IMPORTANT]
> **Normally distributed input features are NOT a requirement for Linear Regression or Logistic Regression.** In linear regression, normality of *residuals* matters for some statistical inference methods—not for making predictions. A transformation is useful only if it helps the analysis or model.

## 2. Normal Distribution and Skewness

- **Normal distribution:** A symmetric, bell-shaped distribution. In an ideal normal distribution, **mean = median = mode**.
- **Right-skewed (positive skew):** Long tail to the right; a few unusually large values. *Example: income or ticket fare.*
- **Left-skewed (negative skew):** Long tail to the left; a few unusually small values. *Example: scores on an easy exam.*

| Shape | Skewness | Common observation |
|---|---|---|
| Symmetric | Near `0` | Both tails are roughly balanced |
| Right-skewed | `> 0` | Some very large values |
| Left-skewed | `< 0` | Some very small values |

**Remember:** Skewness near zero means *rough symmetry*, not necessarily a normal distribution.

## 3. How to Check a Feature's Distribution

### Histogram / KDE (distribution plot)

- Shows where values are concentrated and whether a tail is long.
- Useful for spotting **skewness** and **outliers**.

### Skewness value

- `df["Fare"].skew()` gives a numerical measure of asymmetry.
- Positive → usually **right-skewed**; negative → usually **left-skewed**.

### Q-Q (Quantile-Quantile) plot

- Compares the feature's **observed quantiles** with those of a theoretical normal distribution.
- Points **roughly along the reference line** → approximately normal.
- Curved pattern → distribution may be skewed; tail departures → tails differ from normal.
- The reference line is **not always exactly 45°** because axis scales can differ.

```python
import seaborn as sns
import matplotlib.pyplot as plt
from scipy import stats

s = df["Fare"].dropna()

sns.histplot(s, kde=True)
plt.show()

print("Skewness:", s.skew())
stats.probplot(s, dist="norm", plot=plt)
plt.show()
```

## 4. Common Mathematical Transformations

| Transformation | Formula | Common use | Main caution |
|---|---|---|---|
| **Log** | `log(x)` | Strongly right-skewed positive data | `log(0)` is undefined |
| **Square root** | `√x` | Moderately right-skewed, non-negative data | Cannot use negative real inputs |
| **Square** | `x²` | Sometimes useful for left-skewed **non-negative** data | Magnifies large values; changes ordering across negative/positive values |
| **Reciprocal** | `1/x` | Special cases with positive, skewed values | Undefined at `0`; unstable near `0` |

### A. Log Transformation

- **Compresses large positive values** more than small positive values.
- Often tried for highly **right-skewed** features (e.g., `Fare`, sales, income).
- `np.log1p(x)` means `log(1 + x)` and supports **zero** for non-negative data.

```python
df["Fare_log"] = np.log1p(df["Fare"])
```

**Intuition:** `10 → 1`, `100 → 2`, `1000 → 3` when using `log10`.

### B. Square Root Transformation

- A gentler way to compress large **non-negative** values.
- Often useful for counts and moderately right-skewed features.

```python
df["Fare_sqrt"] = np.sqrt(df["Fare"])
```

**Example:** `1, 4, 9, 16 → 1, 2, 3, 4`.

### C. Square Transformation

- Makes larger magnitudes grow faster; can sometimes reduce left skew for **non-negative** data.
- **Not a universal fix** for left skew; always inspect the result.

```python
df["feature_squared"] = df["feature"] ** 2
```

**Example:** `1, 2, 3, 4 → 1, 4, 9, 16`.

### D. Reciprocal Transformation

- Converts large positive values to smaller ones, and vice versa.
- Reverses the ordering of positive values and is **very sensitive near zero**.

```python
# Only when all values are safely away from zero
df["feature_reciprocal"] = 1 / df["feature"]
```

**Example:** `1, 2, 10 → 1, 0.5, 0.1`.

> [!TIP]
> A transform does **not** guarantee normality or better accuracy. Try it on the **specific feature** that needs it, and measure the result.

## 5. Scikit-Learn Tools

| Tool | What it does | When useful |
|---|---|---|
| **`FunctionTransformer`** | Applies your chosen Python/NumPy function | `log1p`, `sqrt`, custom formulas |
| **`PowerTransformer`** | Learns a power-transformation parameter | Box-Cox or Yeo-Johnson |
| **`QuantileTransformer`** | Maps feature ranks/quantiles to a target distribution | Non-linear mapping to normal/uniform |

```python
import numpy as np
from sklearn.preprocessing import (
    FunctionTransformer, PowerTransformer, QuantileTransformer
)

log_tf = FunctionTransformer(np.log1p)
# log_tf.transform(...) for non-negative values

power_tf = PowerTransformer(method="yeo-johnson")
# power_tf.fit_transform(...) learns from training data
```

- **Box-Cox** needs strictly **positive** values.
- **Yeo-Johnson** accepts positive, zero, and negative values.
- **QuantileTransformer** can map values to a normal distribution but may distort distances and is sensitive to the fitted data range.

## 6. Titanic Dataset: Practical Workflow

**Task:** Predict `Survived` using numerical features such as `Age` and `Fare`.

```text
Train/test split or cross-validation
            ↓
Fill missing Age/Fare values (fit on training data)
            ↓
Inspect distributions of training features
            ↓
Try log1p(Fare), leave Age unchanged
            ↓
Train Logistic Regression / Decision Tree
            ↓
Compare cross-validation scores
```

### What to Compare

1. **Baseline:** Train using the original `Age` and `Fare`.
2. **Transformed:** Try `log1p(Fare)` while keeping `Age` unchanged.
3. Evaluate both with the **same cross-validation splits**.
4. Keep the transformation only if it improves the relevant metric or interpretability.

**Why cross-validation?** A single train/test split can be lucky or unlucky. Cross-validation provides a more stable performance estimate.

> [!WARNING]
> **Avoid data leakage:** Learn missing-value replacements, power-transform parameters, and quantile mappings **only from the training fold**. Use a Scikit-Learn `Pipeline` / `ColumnTransformer` when evaluating models.

## 7. Do Different Models Benefit Equally?

| Logistic Regression | Decision Tree |
|---|---|
| Learns a **linear decision boundary** in its input feature space | Learns **threshold-based splits** |
| Transformations can help represent non-linear feature relationships | Usually unchanged by **strictly monotonic** transformations |
| Does **not** require normally distributed inputs | Does **not** require normally distributed inputs |
| Evaluate the change; improvement is not guaranteed | Often little or no benefit from log/sqrt of the same feature |

**Example:** Applying `log1p(Fare)` changes the numerical relationship available to Logistic Regression. For a Decision Tree, it generally preserves the order of passengers by fare, so the tree can make essentially the same splits.

## 8. Best Practices

- **Inspect first:** Use a histogram, skewness, and optionally a Q-Q plot.
- **Transform selectively:** Different columns may need different functions—or none.
- **Check the domain:** Log, root, and reciprocal transforms have input restrictions.
- **Handle missing values:** Impute safely inside each training fold.
- **Measure model performance:** A more normal-looking plot does not guarantee better predictions.
- **Compare fairly:** Use cross-validation and appropriate metrics, not just a single accuracy score.

## 🧠 Quick Revision

- **Mathematical transformations** change numerical features to improve their representation.
- **Right skew:** Try **log** or **square root**; check the result.
- **Left skew:** **Square** can help for some non-negative features, but not always.
- **Q-Q plot:** Points near the reference line suggest approximate normality.
- **`FunctionTransformer`:** Apply your own function; **`PowerTransformer`:** Box-Cox/Yeo-Johnson; **`QuantileTransformer`:** Quantile mapping.
- **Logistic Regression** may benefit; **Decision Trees** are generally insensitive to strictly monotonic transforms.
- **Prevent leakage:** Fit learned preprocessing within training folds.
- **Golden rule:** Choose transformations by **validation results**, not by the goal of forcing every feature to be normal.
