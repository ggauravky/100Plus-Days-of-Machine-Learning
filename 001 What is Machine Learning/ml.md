# What is Machine Learning?

**Machine Learning (ML)** is a branch of Artificial Intelligence (AI) where computers **learn patterns from data** and use those patterns to make predictions or decisions without being explicitly programmed for every situation.

In simple words:

> **Machine Learning = Learning from data instead of writing every rule manually.**

For example, instead of writing hundreds of rules to identify whether an email is spam, we can give a Machine Learning model many examples of **spam and non-spam emails**. The model learns the patterns and can then classify new emails.

---

## Classical Programming vs Machine Learning

The main difference is **how we get the rules**.

### Classical Programming

In classical programming, **we create the rules ourselves**.

```text
        Classical Programming

     Data  +  Rules
            │
            ▼
      ┌─────────────┐
      │   Program   │
      └─────────────┘
            │
            ▼
          Output
```

**Example:**

To determine whether someone can vote:

```python
age = 20

if age >= 18:
    print("Eligible to vote")
else:
    print("Not eligible to vote")
```

Here, we explicitly created the rule:

```text
age >= 18
```

The computer simply follows it.

---

### Machine Learning

In Machine Learning, instead of manually providing all the rules, we provide **data and examples of correct outputs**.

```text
          Machine Learning

      Data  +  Expected Output
               │
               ▼
        ┌───────────────┐
        │ ML Algorithm  │
        └───────────────┘
               │
               ▼
             Model
               │
               ▼
        New Data → Prediction
```

The ML algorithm finds patterns in the training data and creates a **model**.

That model can then make predictions on new, unseen data.

---

## Simple Difference

```text
CLASSICAL PROGRAMMING

Data + Rules
     ↓
  Program
     ↓
   Output


MACHINE LEARNING

Data + Correct Outputs
        ↓
   ML Algorithm
        ↓
      Model
        ↓
New Data → Prediction
```

---

# Practical Examples of Machine Learning

## 1. Email Spam Detection 📧

Suppose we want to identify whether an email is:

* Spam
* Not Spam

We provide thousands of previously classified emails to an ML model.

```text
Previous Emails
      +
Spam / Not Spam Labels
        │
        ▼
   ML Algorithm
        │
        ▼
      Model
        │
        ▼
    New Email
        │
        ▼
 Spam / Not Spam
```

The model may learn patterns related to:

* words used in the email
* suspicious links
* sender information
* email structure
* previous spam patterns

Then, when a new email arrives, the model predicts whether it is spam.

---

## 2. House Price Prediction 🏠

Suppose we have information about thousands of previously sold houses.

The data might contain:

| Size       | Bedrooms | Location |     Age |    Price |
| ---------- | -------: | -------- | ------: | -------: |
| 1200 sq.ft |        2 | Lucknow  | 5 years | ₹45 Lakh |
| 1800 sq.ft |        3 | Lucknow  | 2 years | ₹70 Lakh |
| 2500 sq.ft |        4 | Lucknow  |  1 year | ₹1 Crore |

The ML model learns relationships between these features and the house price.

```text
Historical House Data
        │
        ▼
   ML Algorithm
        │
        ▼
      Model
        │
        ▼
New House Information
        │
        ▼
 Predicted Price
```

For example:

```text
Size      → 2000 sq.ft
Bedrooms  → 3
Location  → Lucknow
Age       → 3 years

             ↓

        ML Model

             ↓

Predicted Price → ₹75 Lakh
```

---

## 3. Netflix / YouTube Recommendations 🎬

Recommendation systems use your activity and the activity of other users to predict what you might be interested in.

For example, the system may consider things such as:

```text
Videos you watched
Videos you liked
Search history
Watch time
Similar users' behavior
        │
        ▼
 Machine Learning Model
        │
        ▼
Recommended Content
```

If you frequently watch:

```text
Python
Machine Learning
Data Science
AI
```

the system may learn that you're interested in technical content and recommend similar videos.

The goal of ML here is to **predict which content you are most likely to find relevant or engaging.**

---

# Key Takeaways

* **Machine Learning is a part of Artificial Intelligence.**
* ML allows computers to learn patterns from **data**.
* In classical programming, humans usually define the rules.
* In Machine Learning, algorithms learn useful patterns from examples.
* The learned result is called a **model**.
* The model can make **predictions or decisions on new data**.
* Real-world ML applications include spam detection, price prediction, recommendation systems, fraud detection, image recognition, and many more.
