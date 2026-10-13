# Power Transformers in Machine Learning

## 1. What Is a Power Transformation?

- A **power transformation** is a feature engineering technique that changes numerical values using a mathematical power function.
- It is commonly used to make **skewed feature distributions more symmetric or Gaussian-like**.
- It can also **reduce the effect of extreme values** and **stabilize variance**.
- Unlike a fixed log or square-root transform, a power transformer **learns a parameter (`lambda`)** from the data.

### Where Does It Fit?

```text
Feature Engineering
       ↓
Numerical Feature Transformation
       ↓
Power Transformation
       ├── Box-Cox
       └── Yeo-Johnson
```

> [!IMPORTANT]
> **Normal input features are not a requirement for Linear Regression.** Transformations may improve performance by making feature–target relationships more suitable for a linear model, but a more normal-looking feature does **not** guarantee better predictions. Normality of **residuals**, rather than input features, matters for certain statistical inference procedures.

---

## 2. Box-Cox Transformation

**Box-Cox** transforms **strictly positive numerical values** using an automatically selected power.

- Helps reduce skewness and make some distributions closer to normal.
- Uses a parameter called **lambda (`λ`)** to control the transformation.
- Scikit-learn estimates `λ` from the training data using **Maximum Likelihood Estimation (MLE)**.
- **Restriction:** Every input value must be **greater than 0**.

### Formula

For \(x > 0\):

\[
T(x;\lambda)=
\begin{cases}
\dfrac{x^\lambda-1}{\lambda}, & \lambda\neq 0 \\
\ln(x), & \lambda=0
\end{cases}
\]

### Understanding Lambda

| Lambda (`λ`) | Transformation behavior |
|---|---|
| `1` | Approximately linear (`x - 1`) |
| `0` | Log transformation |
| `0.5` | Related to square root |
| `-1` | Related to reciprocal |

- **You do not normally choose `λ` manually**; `PowerTransformer` estimates it.
- The video describes a search around **-5 to +5**, but this is **not a fixed Scikit-learn restriction** on lambda.

### Why Can't Box-Cox Handle Zero?

- Its formula uses \(\ln(x)\) when `λ = 0` and is defined for **strictly positive** input values.
- A column containing `0` or negative values cannot be passed directly to Box-Cox.

**Possible workaround for non-negative features:** Add a positive constant (for example, `1`) before transforming.

```python
import numpy as np
from sklearn.preprocessing import PowerTransformer

X = np.array([[0], [1], [2], [5], [10]], dtype=float)
X_positive = X + 1  # Now all values are > 0

box_cox = PowerTransformer(method="box-cox")
X_box_cox = box_cox.fit_transform(X_positive)

print("Lambda:", box_cox.lambdas_)
print("Transformed values:\n", X_box_cox)
```

> [!WARNING]
> Adding `1` works only if it makes **every value strictly positive**. For negative values or changing data ranges, a fixed shift may be unsuitable. **Yeo-Johnson** is usually simpler when zero or negative values are present.

---

## 3. Yeo-Johnson Transformation

**Yeo-Johnson** is an extension of the power transformation idea that supports **positive, zero, and negative values**.

- Similar goal to Box-Cox: reduce skewness and make distributions more Gaussian-like.
- Learns a suitable **lambda (`λ`)** from the data.
- Handles non-positive values **without manually shifting the entire feature**.
- It is the **default method** in Scikit-learn's `PowerTransformer`.

### Python Example

```python
import numpy as np
from sklearn.preprocessing import PowerTransformer

X = np.array([[-5], [-1], [0], [2], [10]], dtype=float)

yj = PowerTransformer(method="yeo-johnson")
X_yj = yj.fit_transform(X)

print("Lambda:", yj.lambdas_)
print("Transformed values:\n", X_yj)
```

### Box-Cox vs. Yeo-Johnson

| Feature | Box-Cox | Yeo-Johnson |
|---|---|---|
| Valid input | Only `x > 0` | Negative, zero, and positive |
| Uses lambda | Yes | Yes |
| Estimates lambda automatically | Yes | Yes |
| Goal | More Gaussian-like distribution | More Gaussian-like distribution |
| Scikit-learn default | No | **Yes** |
| Best starting point | All values are positive | Data includes zero or negatives |

> [!TIP]
> **Remember:** Box-Cox = **positive only**. Yeo-Johnson = **negative + zero + positive**.

---

## 4. How Does Scikit-Learn Choose Lambda?

- Each feature can receive a **different lambda value**.
- Scikit-learn chooses lambda using **Maximum Likelihood Estimation (MLE)** for these methods.
- In simple terms, it searches for a transformation parameter that makes the transformed values fit a normal-distribution model as closely as possible under that criterion.
- The lesson also mentions Bayesian statistics, but **Scikit-learn's standard `PowerTransformer` does not use Bayesian estimation**.

```python
from sklearn.preprocessing import PowerTransformer

pt = PowerTransformer(method="yeo-johnson")
X_transformed = pt.fit_transform(X_train)

print(pt.lambdas_)  # Learned lambda for each feature
```

**Important:** Call `fit` only on **training data**, then use that fitted transformer to transform validation/test data.

---

## 5. Standardization Happens by Default

Scikit-learn's `PowerTransformer` has **`standardize=True` by default**.

Its process is:

```text
Original Numerical Feature
           ↓
Learn Lambda from Training Data
           ↓
Apply Box-Cox / Yeo-Johnson
           ↓
Standardize (Mean ≈ 0, SD ≈ 1)
           ↓
Transformed Feature
```

- **Power transformation:** Changes the *shape* of the distribution.
- **Standardization:** Changes its *center and scale*.
- Standardization alone **does not remove skewness**.
- Set `standardize=False` if you want only the power transformation.

```python
pt = PowerTransformer(
    method="yeo-johnson",
    standardize=False
)
```

---

## 6. Practical Example: Concrete Strength Prediction

The lesson demonstrates a **regression problem** using concrete material-related features to predict a numerical outcome such as **concrete strength**.

### Step-by-Step Workflow

1. **Load the dataset** and separate numerical features (`X`) from the target (`y`).
2. **Handle missing values** before applying a power transformation.
3. **Train a baseline Linear Regression model** on the original features.
4. Use **histograms / KDE plots and Q-Q plots** to inspect feature distributions.
5. Apply **Box-Cox** (only where values are positive) or **Yeo-Johnson**.
6. Plot the transformed distributions again to see whether skewness decreased.
7. Compare **R² scores** before and after transformation.
8. Prefer **cross-validation** over relying on a single train-test split.

### Distribution Plot and Q-Q Plot

```python
import seaborn as sns
import matplotlib.pyplot as plt
from scipy import stats

feature = X["cement"]  # Replace with an actual numerical column

sns.histplot(feature.dropna(), kde=True)
plt.title("Original Distribution")
plt.show()

stats.probplot(feature.dropna(), dist="norm", plot=plt)
plt.title("Original Q-Q Plot")
plt.show()
```

**How to interpret:**

- **Histogram / KDE:** Shows skewness and distribution shape.
- **Q-Q plot:** Points closer to the reference line suggest a more normal-like distribution; systematic deviations indicate departures from normality.

### Safe Performance Comparison With Cross-Validation

Assume `X` contains the **numerical features** and `y` contains the **regression target**. This compares the baseline with Yeo-Johnson; Box-Cox can be evaluated separately on suitable positive features.

```python
from sklearn.impute import SimpleImputer
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import cross_val_score
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import PowerTransformer

baseline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("model", LinearRegression())
])

yj_model = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("power", PowerTransformer(method="yeo-johnson")),
    ("model", LinearRegression())
])

for name, model in [("Baseline", baseline), ("Yeo-Johnson", yj_model)]:
    scores = cross_val_score(model, X, y, cv=5, scoring="r2")
    print(f"{name}: Mean R² = {scores.mean():.3f}")
```

- The **Pipeline** fits imputation and transformations separately **inside each training fold**, reducing data leakage.
- The video compares **baseline, Box-Cox, and Yeo-Johnson** R² performance; the exact scores were not provided in these notes.
- **Higher R²** generally indicates better predictive fit on the evaluated data, but a transform is only helpful if it improves reliable validation performance.

---

## 7. FunctionTransformer vs. PowerTransformer

| FunctionTransformer | PowerTransformer |
|---|---|
| Applies a function you specify | Learns an appropriate lambda |
| Examples: log, square root, reciprocal | Methods: Box-Cox and Yeo-Johnson |
| Does not automatically optimize the function | Estimates parameters from training data |
| Flexible and simple | Useful for correcting certain skewed distributions |

**Which should you use?**

- Try a **simple fixed transform** when its behavior makes sense for the feature.
- Try a **power transform** when you want a data-driven adjustment of skewness.
- Check the value restrictions and **compare validated model performance**.
- Do not assume power transforms always outperform function transforms.

---

## 🧠 Quick Revision

- **Power transformations** modify numerical feature distributions using a learned **lambda (`λ`)**.
- **Box-Cox** works only on **strictly positive (`> 0`)** values.
- **Yeo-Johnson** accepts **negative, zero, and positive** values; it is the Scikit-learn default.
- `PowerTransformer` estimates lambda using **Maximum Likelihood Estimation (MLE)**.
- By default, `PowerTransformer` also **standardizes** features (`standardize=True`).
- **Histograms, skewness, and Q-Q plots** help evaluate distribution changes.
- **Linear Regression does not require normally distributed input features**.
- **Always compare cross-validated R² scores** before choosing a transformation.
