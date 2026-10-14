# Handling Mixed Variables in Machine Learning

> **Topic:** Feature Engineering · Mixed Numerical and Categorical Variables  
> **Example Dataset:** Titanic (`Ticket` and `Cabin`)  
> **Tools:** Python · Pandas · Regular Expressions (Regex) · Scikit-learn

## 1. What Are Mixed Variables?

- A **mixed variable** is a feature whose values combine **numerical and categorical information**, often in the same string.
- It commonly appears as an `object` or `string` column in Pandas.
- **Example:** `Ticket = "PC 17599"` contains a **category/prefix** (`PC`) and a **number** (`17599`).
- The goal is to extract meaningful information into **separate, model-ready features**.

| Original feature | Example value | Categorical part | Numerical part |
|---|---|---|---|
| `Ticket` | `PC 17599` | `PC` | `17599` |
| `Ticket` | `A/5 21171` | `A/5` | `21171` |
| `Ticket` | `113803` | `NUMERIC_ONLY` | `113803` |
| `Cabin` | `C85` | `C` (deck) | `85` |
| `Cabin` | `B28` | `B` (deck) | `28` |

> [!IMPORTANT]
> A mixed variable is **not simply a column with missing values or a mixture of Python data types**. Here, the key issue is that **one feature value contains more than one kind of information**.

---

## 2. Why Do We Need to Handle Mixed Variables?

- Most standard ML estimators cannot directly use raw strings such as `"C85"` or `"A/5 21171"`.
- Treating each complete string as a category can create **very high cardinality** (many unique categories).
- Converting the entire string to a number fails when letters or symbols are present.
- Separating components lets us apply the appropriate preprocessing:
  - **Numerical part:** imputation, scaling, or other numerical transformations.
  - **Categorical part:** one-hot encoding or another suitable categorical encoding.

### Core Idea

$$
\boxed{\text{Mixed value } x \longrightarrow (x_{\text{category}},\;x_{\text{number}})}
$$

**Example:**

```text
"PC 17599"  ──►  Ticket_Prefix = "PC"
              └─►  Ticket_Number = 17599

"C85"       ──►  Cabin_Deck   = "C"
              └─►  Cabin_Number = 85
```

The **extraction rule depends on the format and meaning of the feature**; there is no universal rule for every mixed column.

---

## 3. Main Strategies for Handling Mixed Variables

### A. Extract the Numerical Component

- Identify the meaningful number using **string methods** or **regular expressions**.
- Convert the extracted text into a numeric data type with `pd.to_numeric()`.
- If no suitable number exists, keep it as `NaN` until a missing-value strategy is chosen.

### B. Extract the Categorical Component

- Extract a **prefix, label, letter, or other meaningful category**.
- Clean whitespace and inconsistent letter case.
- Encode the resulting categories after inspection.

### C. Handle Missing or Unusable Components

- **Categorical missing value:** A label like `MISSING` is often useful.
- **Numerical missing value:** Keep `NaN`, then impute using a training-set statistic when appropriate.
- Do **not automatically replace missing numbers with `0`**: zero may have a real meaning and can mislead the model.

---

## 4. Regular Expressions (Regex)

**Regex** is a pattern-matching method used to find specific characters or groups inside strings.

| Pattern | Meaning | Example match |
|---|---|---|
| `\d` | Any digit (`0–9`) | `7` |
| `\d+` | One or more digits | `17599` |
| `[A-Za-z]` | An English letter | `C` |
| `^` | Start of string | `^C` in `C85` |
| `$` | End of string | `99$` in `17599` |
| `\s*` | Zero or more whitespace characters | Space before a number |
| `( ... )` | Capture a matching group | `(\d+)` captures `17599` |

### Useful Extraction Patterns

```python
r"(\d+)\s*$"   # Trailing digits: "A/5 21171" -> "21171"
r"^([A-Z])"   # First capital letter: "C85" -> "C"
r"(\d+)"     # First run of digits: "C23 C25 C27" -> "23"
```

> [!TIP]
> For Titanic ticket values such as `A/5 21171`, extracting **every digit** would incorrectly mix the `5` in the prefix with the actual trailing ticket number. Use a pattern that matches the **intended position** of the number.

---

## 5. Practical Example: Titanic Dataset

### Step 1 — Inspect the Original Columns

```python
# In an actual Titanic DataFrame named df:
print(df[["Ticket", "Cabin"]].head())
print(df[["Ticket", "Cabin"]].isna().sum())
```

### Step 2 — Split Ticket and Cabin Features

```python
import pandas as pd

# Ticket: e.g., "PC 17599", "A/5 21171", "113803", "LINE"
ticket = df["Ticket"].astype("string").str.strip()

df["Ticket_Number"] = pd.to_numeric(
    ticket.str.extract(r"(\d+)\s*$", expand=False),
    errors="coerce"
)

df["Ticket_Prefix"] = (
    ticket.str.replace(r"\s*\d+\s*$", "", regex=True)
          .str.strip()
          .replace("", "NUMERIC_ONLY")
          .fillna("MISSING")
)

# Cabin: e.g., "C85", "B28", "F G73", or missing
cabin = df["Cabin"].astype("string").str.strip().str.upper()

df["Cabin_Deck"] = (
    cabin.str.extract(r"^([A-Z])", expand=False).fillna("MISSING")
)

df["Cabin_Number"] = pd.to_numeric(
    cabin.str.extract(r"(\d+)", expand=False),
    errors="coerce"
)

print(df[["Ticket_Prefix", "Ticket_Number", "Cabin_Deck", "Cabin_Number"]].head())
```

**How the code works:**

- `.str.extract(...)` returns the part matching a **capturing group** in the regex.
- `.str.replace(...)` removes the trailing ticket number, leaving its prefix.
- `pd.to_numeric(..., errors="coerce")` converts valid numbers and assigns `NaN` when conversion fails.
- `.fillna("MISSING")` gives unavailable categorical values a readable label.
- `NUMERIC_ONLY` means the ticket contains a number but **no separate prefix**. It is not the same as a missing ticket.

### Expected Examples

| Original `Ticket` | `Ticket_Prefix` | `Ticket_Number` |
|---|---|---:|
| `PC 17599` | `PC` | 17599 |
| `A/5 21171` | `A/5` | 21171 |
| `113803` | `NUMERIC_ONLY` | 113803 |
| `LINE` | `LINE` | `NaN` |
| Missing | `MISSING` | `NaN` |

| Original `Cabin` | `Cabin_Deck` | `Cabin_Number` |
|---|---|---:|
| `C85` | `C` | 85 |
| `B28` | `B` | 28 |
| `F G73` | `F` | 73 |
| Missing | `MISSING` | `NaN` |

> [!WARNING]
> Some cabin strings contain **multiple cabins** (for example, `C23 C25 C27`). The simple code above takes only the **first deck letter and first number**. More complex formats need specific parsing rules if the extra information matters.

---

## 6. Analyze the Newly Created Features

### Categorical: Check Categories and Frequencies

```python
print(df["Ticket_Prefix"].value_counts(dropna=False).head(10))
print(df["Cabin_Deck"].value_counts(dropna=False))
```

- Identify **frequent categories**, **rare categories**, and **missing labels**.
- For high-cardinality features, consider grouping rare labels into `OTHER`.
- Such category groupings should be learned from **training data only** when they depend on frequencies.

### Numerical: Check Missing Values and Distribution

```python
print(df[["Ticket_Number", "Cabin_Number"]].isna().sum())
print(df[["Ticket_Number", "Cabin_Number"]].describe())
```

- Check whether extracted numbers have meaningful ranges and relationships.
- A ticket number may be an **identifier**, not a meaningful continuous measurement. Its numerical magnitude may offer little predictive value.
- Keep, transform, or discard engineered features based on **domain knowledge and validation results**.

---

## 7. Prepare the Features for a Machine Learning Model

Use separate preprocessing for the two resulting feature types.

```text
Original mixed columns
         ↓
Extract categorical + numerical components
         ↓
Categorical ──► Encode categories ───────┐
                                         ├──► Train ML model
Numerical   ──► Impute missing numbers ──┘
```

### Scikit-learn Preprocessing Pipeline

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder
from sklearn.pipeline import Pipeline

numeric_cols = ["Ticket_Number", "Cabin_Number"]
category_cols = ["Ticket_Prefix", "Cabin_Deck"]

preprocessor = ColumnTransformer([
    ("num", SimpleImputer(strategy="median"), numeric_cols),
    ("cat", OneHotEncoder(handle_unknown="ignore"), category_cols),
])

# With a classifier and Titanic target "Survived":
from sklearn.linear_model import LogisticRegression

model = Pipeline([
    ("preprocess", preprocessor),
    ("classifier", LogisticRegression(max_iter=1000))
])

# Fit/evaluate using a train/test split or cross-validation.
# Learned preprocessing (such as median) is fitted on training data only.
```

**Why a pipeline?** It keeps preprocessing and model training together, helping avoid **data leakage** when fitting on training folds. Simple deterministic string extraction can be done before splitting, but any learned statistics must come from training data.

---

## 8. Common Mistakes to Avoid

| Mistake | Better approach |
|---|---|
| Encoding the entire ticket string as one category | Extract meaningful components first |
| Extracting every digit from a complex ticket prefix | Match the correct **trailing number** |
| Replacing every missing numerical value with `0` | Use `NaN`, then an appropriate imputer |
| Treating every extracted number as a useful measurement | Check whether it is just an **ID** |
| Assuming every cabin value has one simple format | Inspect multi-cabin and irregular values |
| Learning imputations or frequency grouping on all data | Fit learned preprocessing using training data only |

> [!IMPORTANT]
> **Feature engineering does not guarantee higher accuracy.** Compare a baseline model with an engineered-feature model using appropriate evaluation metrics and cross-validation.

---

## 🧠 Quick Revision

- **Mixed variables** contain numerical and categorical information in a single feature value.
- **Main technique:** Split the original column into **separate numerical and categorical features**.
- **Regex** helps extract letters, prefixes, and numbers from mixed strings.
- **`pd.to_numeric(errors="coerce")`** converts extracted numbers and returns `NaN` for invalid values.
- **Missing category →** a label such as `MISSING`; **missing number →** typically `NaN` followed by suitable imputation.
- **High cardinality** can result from encoding complete mixed strings; analyze extracted categories first.
- **Ticket numbers may act like identifiers**, so numerical extraction is not automatically useful.
- **Validate engineered features** with cross-validation and avoid data leakage.
