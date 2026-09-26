# Pandas Profiling for Automated EDA

## What is Pandas Profiling?

**Pandas Profiling** is a library that automates many repetitive **Exploratory Data Analysis (EDA)** tasks.

It can automatically generate:

- Dataset statistics
- Missing value analysis
- Correlations
- Distributions
- Duplicate information
- Variable-level summaries

Instead of checking every column manually, it creates a detailed **HTML report**.

---

## Installation

```bash
pip install pandas-profiling
```

---

## Basic Usage

```python
import pandas as pd
from pandas_profiling import ProfileReport

df = pd.read_csv("data.csv")

profile = ProfileReport(df)

profile.to_file("report.html")
```

This creates:

```text
report.html
```

Open it in a browser to explore the dataset.

---

# Main Sections of the Report

## 1. Overview

Shows general information about the dataset:

- Number of rows
- Number of columns
- Missing values
- Duplicate rows
- Memory usage
- Data types
- Automatic warnings

Example warning:

```text
Column "Cabin" has many missing values.
```

---

## 2. Variables

Provides detailed information for each column.

For numerical columns:

- Mean
- Median
- Standard deviation
- Minimum / Maximum
- Percentiles
- Histogram

For categorical columns:

- Unique values
- Most frequent values
- Frequency distribution

---

## 3. Interactions

Shows relationships between numerical variables using graphs such as **scatter plots**.

Example:

```text
Age vs Fare
Fare vs Passenger Class
```

Useful for **bivariate analysis**.

---

## 4. Correlations

Shows correlation between numerical variables.

Common correlation method:

- **Pearson Correlation**

Helps identify variables that move together.

> [!IMPORTANT]
> High correlation shows a relationship, but it does not automatically mean one variable causes the other.

---

## 5. Missing Values

Provides:

- Missing value counts
- Missing percentages
- Visualizations of missing-data patterns

This helps identify columns that may require:

- Imputation
- Cleaning
- Removal

---

## 6. Sample

Shows sample rows from the dataset, such as:

- First rows
- Last rows

Useful for quickly checking whether the data looks correct.

---

# Simple Workflow

```text
Load Dataset
     ↓
Create ProfileReport
     ↓
Generate HTML Report
     ↓
Inspect Warnings
     ↓
Study Variables
     ↓
Check Correlations
     ↓
Check Missing Values
     ↓
Write Observations
```

---

## Why Use Pandas Profiling?

### Advantages

- Saves time
- Automates repetitive EDA
- Gives a quick dataset overview
- Finds missing values and duplicates
- Provides useful visualizations
- Helps discover possible data-quality problems

### Limitation

Do not depend only on automated reports.

You should still:

- Inspect important columns manually.
- Ask meaningful questions.
- Verify unusual patterns.
- Write your own observations.

---

## 🧠 Quick Revision

- **Pandas Profiling** automates many EDA tasks.
- `ProfileReport()` generates the analysis.
- `.to_file()` exports the report as HTML.
- **Overview** → dataset summary.
- **Variables** → individual column analysis.
- **Interactions** → relationships between variables.
- **Correlations** → numerical relationships.
- **Missing Values** → null-data patterns.
- **Sample** → quick look at rows.
- Automated EDA saves time, but **manual interpretation is still important**.