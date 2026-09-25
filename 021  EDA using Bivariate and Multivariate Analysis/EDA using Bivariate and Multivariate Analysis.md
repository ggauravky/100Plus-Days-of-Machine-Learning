# Bivariate and Multivariate Analysis in EDA

## What is Bivariate Analysis?

**Bivariate Analysis** studies the relationship between **two variables**.

Examples:

```text
Age vs Fare
Gender vs Survived
Class vs Fare
```

## What is Multivariate Analysis?

**Multivariate Analysis** studies **more than two variables together**.

Example:

```text
Fare + Age + Gender + Survival Status
```

The correct visualization depends on whether the variables are:

* **Numerical**
* **Categorical**

---

# 1. Numerical vs Numerical

## Scatter Plot

Used to study the relationship between two continuous numerical variables.

```python
import seaborn as sns

sns.scatterplot(
    x="Age",
    y="Fare",
    data=df
)
```

Useful for identifying:

* Positive relationship
* Negative relationship
* Clusters
* Outliers
* Possible linear trends

Example:

```text
Age ↑
Fare ↑
```

may indicate a positive relationship.

---

## Multivariate Scatter Plot

Extra variables can be added using:

* `hue`
* `style`
* `size`

```python
sns.scatterplot(
    x="total_bill",
    y="tip",
    hue="sex",
    style="smoker",
    size="size",
    data=df
)
```

This allows several variables to be analyzed in one graph.

---

# 2. Numerical vs Categorical

## Bar Plot

Used to compare the average numerical value across categories.

```python
sns.barplot(
    x="Pclass",
    y="Fare",
    data=df
)
```

Example:

```text
Passenger Class
      ↓
Average Fare
```

---

## Box Plot

Shows the distribution of a numerical variable for different categories.

```python
sns.boxplot(
    x="Pclass",
    y="Age",
    data=df
)
```

Helps compare:

* Median
* Spread
* Quartiles
* Outliers

---

## Distribution / KDE Plot

Used to compare numerical distributions between categories.

Example:

```python
sns.kdeplot(
    data=df,
    x="Age",
    hue="Survived"
)
```

This can help answer questions like:

> How does the age distribution differ between passengers who survived and those who did not?

---

# 3. Categorical vs Categorical

## Crosstab

A **crosstab** creates a frequency table between two categorical variables.

```python
import pandas as pd

pd.crosstab(
    df["Pclass"],
    df["Survived"]
)
```

Example:

| Pclass | Not Survived | Survived |
| ------ | -----------: | -------: |
| 1      |           80 |      136 |
| 2      |           97 |       87 |
| 3      |          372 |      119 |

---

## Heatmap

A heatmap visually represents a crosstab or matrix.

```python
table = pd.crosstab(
    df["Pclass"],
    df["Survived"]
)

sns.heatmap(
    table,
    annot=True
)
```

Useful for quickly spotting high and low values.

---

## Clustermap

A **Clustermap** groups similar rows and columns together.

```python
sns.clustermap(table)
```

It also shows a **dendrogram**, which represents hierarchical similarity between groups.

---

# 4. Pairplot

A **Pairplot** automatically compares multiple numerical columns.

```python
sns.pairplot(df)
```

With categories:

```python
sns.pairplot(
    df,
    hue="Survived"
)
```

It is useful for quickly observing:

* Relationships
* Clusters
* Trends
* Distributions

---

# 5. Line Plot

A **Line Plot** is useful when data has an order, especially **time-series data**.

```python
sns.lineplot(
    x="Date",
    y="Sales",
    data=df
)
```

Example:

```text
Time
 ↓
Jan → Feb → Mar → Apr
 ↓
Sales Trend
```

It helps identify:

* Growth
* Decline
* Seasonal patterns
* Sudden changes

---

# 6. Pivot Table

A **Pivot Table** summarizes data by converting categories into rows and columns.

```python
table = df.pivot_table(
    values="Fare",
    index="Pclass",
    columns="Survived"
)
```

The result can then be visualized using:

```python
sns.heatmap(
    table,
    annot=True
)
```

---

# Choosing the Right Plot

| Variable Combination         | Common Visualization                |
| ---------------------------- | ----------------------------------- |
| Numerical + Numerical        | Scatter Plot                        |
| Numerical + Categorical      | Bar Plot, Box Plot, KDE Plot        |
| Categorical + Categorical    | Crosstab, Heatmap, Clustermap       |
| Multiple Numerical Variables | Pairplot                            |
| Time / Sequential Data       | Line Plot                           |
| Multiple Variables           | `hue`, `style`, `size`, Pivot Table |

---

# Simple EDA Approach

```text
Ask a Question
      ↓
Identify Variable Types
      ↓
Choose Suitable Plot
      ↓
Visualize Data
      ↓
Find Patterns / Relationships
      ↓
Generate Insights
```

> [!IMPORTANT]
> **EDA is not just about creating graphs.**
>
> The main goal is to ask meaningful questions and use visualizations to discover the story hidden inside the data.

---

## 🧠 Quick Revision

* **Bivariate Analysis** studies two variables.
* **Multivariate Analysis** studies more than two variables.
* **Scatter Plot** → numerical vs numerical.
* **Bar Plot / Box Plot / KDE** → numerical vs categorical.
* **Crosstab / Heatmap** → categorical vs categorical.
* **Clustermap** → finds hierarchical similarities.
* **Pairplot** → compares multiple numerical features.
* **Line Plot** → useful for time-series or ordered data.
* **Pivot Table** → creates summarized matrices.
* Good EDA starts with **asking the right question**, not just plotting graphs.
