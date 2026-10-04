# Machine Learning Pipelines in Scikit-Learn

## What is a Pipeline?

A **Pipeline** chains multiple machine learning steps into one workflow.

```text
Raw Data
   ↓
Missing Value Handling
   ↓
Encoding
   ↓
Feature Selection
   ↓
Model
   ↓
Prediction
```

The output of one step automatically becomes the input of the next step.

---

## Why Use Pipelines?

Without pipelines, we manually repeat preprocessing for:

- Training data
- Testing data
- New production data

This can cause:

- Repeated code
- Inconsistent preprocessing
- Data leakage
- Deployment problems

With a pipeline, all preprocessing and modeling logic stays together.

---

# Example Workflow

Using the **Titanic dataset**:

```text
Age → Missing value imputation
Sex / Embarked → One-Hot Encoding
Features → Feature Selection
Final Data → Model
```

---

## ColumnTransformer Inside Pipeline

Different columns may need different preprocessing.

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder

preprocessor = ColumnTransformer([
    ("age_imputer",
     SimpleImputer(strategy="mean"),
     [2]),

    ("categorical",
     OneHotEncoder(handle_unknown="ignore"),
     [1, 6])
])
```

The video uses **column indices** so the transformations continue working when intermediate data becomes NumPy arrays.

---

# Creating a Pipeline

```python
from sklearn.pipeline import Pipeline
from sklearn.tree import DecisionTreeClassifier

pipe = Pipeline([
    ("preprocessing", preprocessor),
    ("model", DecisionTreeClassifier())
])
```

Now the complete workflow is contained inside `pipe`.

---

## Train the Pipeline

```python
pipe.fit(X_train, y_train)
```

The pipeline automatically:

```text
X_train
   ↓
Preprocessing
   ↓
Transformation
   ↓
Model Training
```

---

## Make Predictions

```python
y_pred = pipe.predict(X_test)
```

You do **not** need to manually preprocess `X_test`.

The pipeline automatically applies the same transformations.

---

# Feature Selection in Pipeline

Feature selection can also be added before the model.

```text
Preprocessing
     ↓
Feature Selection
     ↓
Model
```

Example structure:

```python
pipe = Pipeline([
    ("preprocessing", preprocessor),
    ("feature_selection", selector),
    ("model", model)
])
```

This keeps the whole ML workflow organized.

---

# Access Pipeline Steps

Pipeline components can be inspected using:

```python
pipe.named_steps
```

Example:

```python
pipe.named_steps["model"]
```

This allows us to inspect:

- Model parameters
- Transformers
- Individual pipeline components

---

# Saving the Entire Pipeline

One major advantage is that the complete pipeline can be saved using **Pickle**.

```python
import pickle

pickle.dump(
    pipe,
    open("pipeline.pkl", "wb")
)
```

Load it later:

```python
pipe = pickle.load(
    open("pipeline.pkl", "rb")
)
```

Now one file contains:

```text
Preprocessing
+
Encoding
+
Feature Selection
+
Model
```

This makes deployment much easier.

---

# Cross-Validation

Cross-validation can be performed directly on the pipeline.

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(
    pipe,
    X,
    y,
    cv=5
)
```

The complete preprocessing + model workflow runs inside every fold.

---

# Hyperparameter Tuning

Pipeline parameters can also be tuned using `GridSearchCV`.

Example:

```python
from sklearn.model_selection import GridSearchCV

params = {
    "model__max_depth": [3, 5, 10]
}

grid = GridSearchCV(
    pipe,
    params,
    cv=5
)

grid.fit(X_train, y_train)
```

Notice:

```text
model__max_depth
```

Format:

```text
step_name__parameter_name
```

---

# Pipeline vs make_pipeline

## Pipeline

You manually give names to steps.

```python
Pipeline([
    ("preprocessing", preprocessor),
    ("model", model)
])
```

## make_pipeline

Step names are generated automatically.

```python
from sklearn.pipeline import make_pipeline

pipe = make_pipeline(
    preprocessor,
    model
)
```

Both perform similar jobs.

---

# Main Advantage

Without Pipeline:

```text
Preprocess Train
Preprocess Test
Encode Train
Encode Test
Select Features
Train Model
Repeat Everything During Deployment
```

With Pipeline:

```text
Raw Data
   ↓
Pipeline
   ↓
Prediction
```

---

## 🧠 Quick Revision

- **Pipeline** combines preprocessing and model training into one workflow.
- Each step automatically passes its output to the next step.
- It reduces repeated preprocessing code.
- `ColumnTransformer` can handle different columns differently.
- Feature selection can also be included inside a pipeline.
- `pipe.fit()` trains preprocessing steps and the model.
- `pipe.predict()` automatically preprocesses new data before prediction.
- `named_steps` helps inspect individual components.
- The complete pipeline can be saved using **Pickle**.
- Pipelines work directly with **Cross-Validation** and **GridSearchCV**.
- `Pipeline` uses custom step names, while `make_pipeline()` creates names automatically.