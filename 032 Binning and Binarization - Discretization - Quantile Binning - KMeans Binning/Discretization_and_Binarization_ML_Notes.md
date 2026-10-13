# Discretization and Binarization in Machine Learning

> **Topic:** Feature Engineering → Numerical Data Encoding  
> **Libraries:** Pandas, NumPy, Scikit-learn  
> **Goal:** Convert numerical values into meaningful categories or binary indicators.

## 1. What Is Numerical Data Encoding?

- **Numerical data encoding** can involve converting continuous numerical features into **discrete categories**.
- This is useful when **ranges** matter more than exact numbers, such as age groups or income brackets.
- Two techniques covered in this lesson are:
  - **Discretization (Binning):** Divide numerical values into multiple intervals or groups.
  - **Binarization:** Convert numerical values into **0 or 1** using a threshold.

**Example:**

```text
Numerical Age:  12   24   42   67
Age Group:      Child  Young Adult  Adult  Senior
```

### Why Use These Techniques?

- Capture **non-linear relationships** that may be easier to model using categories.
- Reduce the influence of small numerical differences within a group.
- Simplify a feature into understandable intervals or indicators.
- Sometimes reduce the impact of extreme values **after values are grouped**.

> [!IMPORTANT]
> Binning **does not remove outliers or guarantee better accuracy**. It also discards some numerical information, so always compare model performance before and after applying it.

---

## 2. Discretization (Binning)

**Discretization** converts continuous numerical values into a limited number of **bins (intervals)**.

**Example:**

```text
Marks:       23, 45, 68, 91
Categories:  Low, Medium, High, Very High
```

The four important strategies are **Equal Width**, **Equal Frequency**, **K-Means**, and **Custom/Domain-Based Binning**.

### A. Equal Width (Uniform) Binning

- Divides the entire numerical range into **intervals of equal width**.
- Each bin covers the same size range, but the **number of observations may differ**.
- Simple to understand and implement.

**Formula:**

\[
\text{Bin Width} = \frac{\text{Maximum Value} - \text{Minimum Value}}{\text{Number of Bins}}
\]

**Example:** Ages from `0` to `80`, divided into `4` bins:

```text
0–20 | 20–40 | 40–60 | 60–80
```

- **Best suited for:** Data where fixed, equally sized intervals make sense.
- **Limitation:** Extreme minimum or maximum values can stretch the range and leave some bins nearly empty.

### B. Equal Frequency (Quantile) Binning

- Creates bins with **approximately equal numbers of observations**.
- Bin widths can be different because they depend on the data distribution.
- Useful for **skewed datasets** where uniform-width bins would be unbalanced.

**Example:** If `100` observations are divided into `4` quantile bins, each bin should contain **approximately 25 observations**.

```text
100 observations
       ↓
  4 quantile bins
       ↓
 ~25 values per bin
```

- **Best suited for:** Skewed numerical features and balanced bin frequencies.
- **Limitation:** Bin widths may be very unequal; repeated values can prevent perfectly equal frequencies.

> [!NOTE]
> Quantile binning balances the **count of observations** across bins. It does **not** make the original numerical values uniformly distributed.

### C. K-Means Binning

- Uses **K-Means clustering** to identify natural groups in a numerical feature.
- Each cluster has a **centroid (center)**.
- Values are grouped according to proximity to these centroids; boundaries are determined between neighboring centers.
- Works well when the feature contains **distinct clusters of values**.

**Example:**

```text
Values: [2, 3, 4]   [48, 50, 52]   [97, 99, 101]
           ↓              ↓                ↓
        Cluster 1      Cluster 2        Cluster 3
```

- **Best suited for:** Features with natural numerical groupings.
- **Limitation:** Computationally more expensive than uniform or quantile binning and not always better.

### D. Custom / Domain-Based Binning

- Defines bin boundaries **manually**, using domain knowledge or business rules.
- Useful when specific thresholds have a clear meaning.

**Example: Age groups**

| Age Range | Category |
|---|---|
| 0–17 | Child |
| 18–34 | Young Adult |
| 35–59 | Adult |
| 60+ | Senior |

**Pandas implementation:**

```python
import numpy as np
import pandas as pd

df["Age_Group"] = pd.cut(
    df["Age"],
    bins=[0, 18, 35, 60, np.inf],
    labels=["Child", "Young Adult", "Adult", "Senior"],
    right=False
)
```

- **Best suited for:** Age categories, income slabs, grade bands, or established domain thresholds.
- **Limitation:** Poorly chosen boundaries may discard useful distinctions.

---

## 3. Comparison of Binning Strategies

| Strategy | How Bins Are Created | Main Advantage | Main Limitation |
|---|---|---|---|
| **Uniform** | Equal-width intervals | Simple and interpretable | Sensitive to extreme range values |
| **Quantile** | Approximately equal observation counts | Useful for skewed data | Unequal interval widths |
| **K-Means** | Clusters based on centroids | Captures natural value groups | More computational work |
| **Custom** | Manually defined thresholds | Domain-relevant categories | Depends on boundary selection |

---

## 4. Scikit-learn: `KBinsDiscretizer`

`KBinsDiscretizer` converts continuous numerical features into a fixed number of bins.

**Main parameters:**

| Parameter | Purpose | Common Values |
|---|---|---|
| `n_bins` | Number of bins | `3`, `5`, `10` |
| `strategy` | How boundaries are chosen | `'uniform'`, `'quantile'`, `'kmeans'` |
| `encode` | How bins are represented | `'ordinal'`, `'onehot'`, `'onehot-dense'` |

### Example: Quantile Binning

```python
import numpy as np
from sklearn.preprocessing import KBinsDiscretizer

X = np.array([[10], [20], [35], [50], [70], [90]])

binner = KBinsDiscretizer(
    n_bins=3,
    strategy="quantile",
    encode="ordinal"
)

X_binned = binner.fit_transform(X)
print(X_binned.ravel())
print(binner.bin_edges_[0])  # Learned bin boundaries
```

### `ordinal` vs. `onehot` Encoding

- **Ordinal:** Represents each bin with an integer label, such as `0`, `1`, or `2`.
- **One-hot:** Creates separate binary columns for the bins.

**Example:**

```text
Ordinal bins:  0  1  2

One-hot bins:
Bin 0  Bin 1  Bin 2
  1      0      0
  0      1      0
  0      0      1
```

> [!TIP]
> **One-hot encoding** is often a good choice for a linear model such as Logistic Regression because numeric bin labels can otherwise imply a restrictive linear relationship. For tree-based models, ordinal bin labels are often sufficient.

---

## 5. Binarization

**Binarization** is a special case of discretization that converts numerical values into **two categories: `0` and `1`**.

For Scikit-learn's `Binarizer`, the rule is:

\[
f(x) =
\begin{cases}
0, & x \leq t \\
1, & x > t
\end{cases}
\]

Where `t` is the **threshold**.

**Example:** Threshold = `50`

```text
Original:    20   50   75   90
Binary:       0    0    1    1
```

> [!IMPORTANT]
> With `Binarizer`, a value **equal to the threshold becomes 0**, not 1.

### Scikit-learn: `Binarizer`

```python
import numpy as np
from sklearn.preprocessing import Binarizer

X = np.array([[20], [50], [75], [90]])

binarizer = Binarizer(threshold=50)
X_binary = binarizer.transform(X)

print(X_binary.ravel())  # [0 0 1 1]
```

### Real-World Applications

- **Image processing:** Classify pixels as black or white using an intensity threshold.
- **Education:** Indicate whether marks exceed a threshold.
- **Banking:** Flag transactions above a specified amount.
- **Titanic:** Create a feature showing whether a passenger has family aboard.

**Titanic example:**

```python
# SibSp: siblings/spouses aboard
# Parch: parents/children aboard

df["Has_Family"] = ((df["SibSp"] + df["Parch"]) > 0).astype(int)
df["Is_Alone"] = 1 - df["Has_Family"]
```

- `Has_Family = 1` means the passenger has at least one family member aboard.
- `Is_Alone = 1` means no family members are recorded in `SibSp` or `Parch`.

---

## 6. Practical Workflow: Titanic Dataset

**Task:** Predict passenger survival (`Survived`) and test whether binning numerical features improves model performance.

**Example features:**

- `Age` — Passenger age.
- `Fare` — Ticket fare.
- `Survived` — Prediction target (`0` or `1`).

### Workflow

```text
Age / Fare (Numerical Data)
             ↓
      Handle Missing Data
             ↓
   Inspect Feature Distribution
             ↓
     Try Different Binning
             ↓
       Encode the Bins
             ↓
      Train ML Model
             ↓
   Compare Cross-Validation Scores
```

### Compare Original vs. Binned Features

The following assumes a Pandas DataFrame `df` with columns `Age`, `Fare`, and `Survived`.

```python
from sklearn.impute import SimpleImputer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import StratifiedKFold, cross_val_score
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import KBinsDiscretizer, StandardScaler

X = df[["Age", "Fare"]]
y = df["Survived"]

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

# Model 1: Original numerical features
baseline = make_pipeline(
    SimpleImputer(strategy="median"),
    StandardScaler(),
    LogisticRegression(max_iter=1000)
)

# Model 2: Quantile-binned, one-hot encoded features
binned = make_pipeline(
    SimpleImputer(strategy="median"),
    KBinsDiscretizer(n_bins=5, strategy="quantile", encode="onehot"),
    LogisticRegression(max_iter=1000)
)

baseline_score = cross_val_score(baseline, X, y, cv=cv, scoring="accuracy")
binned_score = cross_val_score(binned, X, y, cv=cv, scoring="accuracy")

print("Original features:", baseline_score.mean())
print("Binned features:  ", binned_score.mean())
```

**What to observe:**

- Does model accuracy improve, decrease, or stay similar after binning?
- Does `Fare` benefit from **quantile** rather than **uniform** bins?
- Would a different number of bins change the result?
- Would a tree-based model benefit as much as Logistic Regression?

> [!IMPORTANT]
> Put imputation and learned binning **inside the pipeline** so that the median and bin boundaries are learned only from each training fold. This prevents **data leakage** during cross-validation.

---

## 7. Discretization vs. Binarization

| Discretization | Binarization |
|---|---|
| Creates **multiple** bins | Creates exactly **two** groups |
| Can use uniform, quantile, K-Means, or custom thresholds | Uses a binary threshold rule |
| Example: Age → Child / Adult / Senior | Example: Fare > 50 → 0 / 1 |
| Common tool: `KBinsDiscretizer` | Common tool: `Binarizer` |

---

## 8. When Should You Use Binning?

**Consider binning when:**

- Meaningful thresholds or ranges exist.
- A numerical feature has a non-linear relationship with the target.
- You need understandable categorical groups.
- You want to test a different representation of skewed features.

**Be careful when:**

- Exact numerical differences carry useful information.
- Too many bins cause sparse, unstable groups.
- Too few bins hide important patterns.
- Outliers distort uniform-width bin boundaries.
- Bin thresholds are chosen using the entire dataset before validation.

**Practical rule:** Compare **no binning**, **different strategies**, and **different bin counts** using cross-validation. Choose based on the model and dataset, not a universal preference.

---

## 🧠 Quick Revision

- **Discretization:** Converts continuous numerical values into several categories or bins.
- **Uniform Binning:** Equal-width intervals; observation counts can differ.
- **Quantile Binning:** Approximately equal observation counts; interval widths can differ.
- **K-Means Binning:** Groups nearby numerical values using cluster centroids.
- **Custom Binning:** Uses manually selected, domain-based boundaries.
- **Binarization:** Converts a feature into `0` or `1` using a threshold.
- **`KBinsDiscretizer`:** Supports `uniform`, `quantile`, and `kmeans`; **`Binarizer`** handles threshold-based conversion.
- **Most important:** Binning may improve a model, but it loses detail. **Evaluate using cross-validation and avoid data leakage.**
