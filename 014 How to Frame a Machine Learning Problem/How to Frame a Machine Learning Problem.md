# Framing a Machine Learning Problem

Before building a machine learning model, first convert the **business problem** into a clear **ML problem**.

## Example: Netflix Churn

Netflix wants to:

```text
Increase Revenue
      ↓
Retain More Users
      ↓
Reduce Customer Churn
```

**Churn** means a customer stops using or cancels the service.

---

## 1. Define the Business Goal

Start with the actual business objective.

Example:

* Goal → Increase revenue
* Possible solution → Retain existing customers
* ML objective → Identify users likely to leave Netflix

> The problem should be **specific and measurable**.

---

## 2. Decide the ML Problem Type

Choose the type of prediction required.

### Classification

Predict:

```text
Will user leave?

Yes / No
```

### Regression / Risk Score

Predict something like:

```text
User A → 90% churn risk
User B → 60% churn risk
User C → 15% churn risk
```

This allows Netflix to take different actions for different users.

Example:

```text
High Risk   → Special discount
Medium Risk → Better recommendations
Low Risk    → No action
```

---

## 3. Check Existing Solutions

Before creating a new model:

* Check whether the company already has a churn model.
* Study previous approaches.
* Understand which features worked well.
* Identify problems in the existing system.

This can save **time, money, and effort**.

---

## 4. Identify Required Data

Decide what information may help predict churn.

Example Netflix features:

* Watch time
* Login frequency
* Search history
* Number of movies/series watched
* Recommendation clicks
* Subscription plan
* Days since last activity

Example dataset:

| Watch Time | Logins | Last Active | Churn |
| ---------: | -----: | ----------: | ----- |
|     50 hrs |     25 |       1 day | No    |
|      3 hrs |      2 |     20 days | Yes   |

The **data engineering team** may help collect and prepare this data.

---

## 5. Define Success Metrics

Decide how you will know whether the project is successful.

Technical metrics may include:

* Accuracy
* Precision
* Recall
* F1 Score
* MAE / RMSE

But the **business metric** is also important.

Example:

```text
Before Model → Churn = 10%
After Model  → Churn = 7%
```

The real goal is not just high model accuracy.

The goal is:

> **Reduce actual customer churn.**

---

## 6. Choose Learning Strategy

### Batch Learning

Model is retrained periodically.

```text
Collect Data
    ↓
Train Model
    ↓
Deploy
    ↓
Retrain Later
```

Useful when data does not change very quickly.

### Online Learning

Model learns or updates more frequently as new data arrives.

Useful when user behaviour changes rapidly.

Example:

* Festivals
* Holidays
* New Netflix releases
* Major events

may suddenly change viewing behaviour.

---

## 7. Validate Assumptions

Before and during development, check assumptions.

Questions to ask:

* Is the required data actually available?
* Is the data reliable?
* Are the selected features useful?
* Does the model work for different countries?
* Is user behaviour changing over time?
* Does the model still solve the original business problem?

---

## Simple ML Problem Framing Flow

```text
Business Problem
      ↓
Define Goal
      ↓
Choose ML Problem
      ↓
Check Existing Solution
      ↓
Identify Data
      ↓
Build Model
      ↓
Evaluate Model
      ↓
Measure Business Impact
      ↓
Improve & Retrain
```

---

## 🧠 Quick Revision

* Start with the **business goal**, not with coding.
* Convert the business goal into a clear **ML problem**.
* Decide between **classification, regression, or another ML approach**.
* Check whether an existing solution already exists.
* Identify the required **features and data sources**.
* Define both **technical metrics and business KPIs**.
* Choose between **batch learning and online learning**.
* Continuously verify your assumptions.
* A good data scientist should understand both **business + machine learning**.

> [!IMPORTANT]
> **Do not start building a model until you clearly understand what business problem the model is supposed to solve.**
