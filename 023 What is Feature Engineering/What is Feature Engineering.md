# Feature Engineering

## What is Feature Engineering?

**Feature Engineering** is the process of using **domain knowledge** to transform or create useful features from raw data so that a machine learning model can perform better.

```text
Raw Data
   ↓
Feature Engineering
   ↓
Better Features
   ↓
Machine Learning Model
```

> [!IMPORTANT]
> Good features can sometimes improve a model more than simply changing the machine learning algorithm.

Feature engineering often depends on:

- Domain knowledge
- Experience
- Data understanding
- Experimentation

---

# Types of Feature Engineering

```text
Feature Engineering
│
├── Feature Transformation
├── Feature Construction
├── Feature Selection
└── Feature Extraction
```

---

## 1. Feature Transformation

**Feature Transformation** modifies existing features into a more useful form.

### Missing Value Imputation

Used when some values are missing.

Example:

```text
Age
20
25
NaN
30
```

Possible solutions:

- Remove rows
- Fill using **mean**
- Fill using **median**
- Fill using **mode**

Example:

```python
df["Age"].fillna(df["Age"].median())
```

---

### Handling Categorical Data

Machine learning models usually require numerical input.

Example:

```text
Gender
Male
Female
Male
```

Convert categories into numbers using techniques such as **One-Hot Encoding**.

```text
Male   → [1, 0]
Female → [0, 1]
```

---

### Outlier Detection

**Outliers** are observations that are significantly different from most data points.

Example:

```text
Salary:
25k, 30k, 28k, 32k, 500k
                    ↑
                 Outlier
```

Outliers may affect model predictions and may require detection and handling.

---

### Feature Scaling

Feature scaling brings features to comparable ranges.

Example:

```text
Age    → 20–60
Salary → 20,000–500,000
```

Without scaling, the large salary values may dominate calculations in algorithms such as **K-Nearest Neighbors (KNN)**.

Common techniques include:

- Standardization
- Normalization

---

## 2. Feature Construction

**Feature Construction** means creating a new feature from existing features using domain knowledge.

### Titanic Example

Suppose we have:

```text
SibSp  → Number of siblings/spouses
Parch  → Number of parents/children
```

We can create:

```text
FamilySize = SibSp + Parch + 1
```

```python
df["FamilySize"] = df["SibSp"] + df["Parch"] + 1
```

The new feature may provide the model with more useful information.

---

## 3. Feature Selection

**Feature Selection** means keeping only the most useful features and removing irrelevant or redundant ones.

Example:

```text
Original Features
↓
Age
Name
Fare
Passenger Class
Ticket Number
Cabin
...
```

After selection:

```text
Age
Fare
Passenger Class
```

Benefits:

- Faster training
- Less computation
- Simpler model
- May improve model performance

### Image Example

For handwritten digit recognition, some pixels may contain very little useful information.

Instead of using every pixel, we may select only the more informative ones.

---

## 4. Feature Extraction

**Feature Extraction** creates a **new set of features** from the original features.

A common technique is:

**PCA — Principal Component Analysis**

Example:

```text
100 Original Features
        ↓
       PCA
        ↓
10 New Features
```

The aim is to reduce dimensions while preserving important information.

---

# Feature Selection vs Feature Extraction

| Feature Selection | Feature Extraction |
|---|---|
| Selects existing features | Creates new features |
| Removes irrelevant features | Combines/transforms original features |
| Original features remain understandable | New features may be less interpretable |
| Example: selecting Age and Fare | Example: PCA |

---

# Simple Feature Engineering Workflow

```text
Raw Dataset
     ↓
Handle Missing Values
     ↓
Encode Categories
     ↓
Handle Outliers
     ↓
Scale Features
     ↓
Construct New Features
     ↓
Select / Extract Features
     ↓
Train Model
```

---

## 🧠 Quick Revision

- **Feature Engineering** converts raw data into better features for machine learning.
- **Feature Transformation** modifies existing features.
- Missing values can be handled using **mean, median, mode, or removal**.
- Categorical values need to be converted into numerical form.
- **Feature Scaling** makes features comparable.
- **Feature Construction** creates new features from existing ones.
- **Feature Selection** keeps only useful existing features.
- **Feature Extraction** creates new features, such as using **PCA**.
- Better features can have a major impact on model performance.