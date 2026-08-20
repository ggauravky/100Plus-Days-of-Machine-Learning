#  Types of Machine Learning

Machine Learning can be broadly divided into **four main types** based on how a model learns from data.

```text
Machine Learning
│
├── 1. Supervised Learning
│   ├── Regression
│   └── Classification
│
├── 2. Unsupervised Learning
│   ├── Clustering
│   ├── Dimensionality Reduction
│   ├── Anomaly Detection
│   └── Association Rule Learning
│
├── 3. Semi-Supervised Learning
│
└── 4. Reinforcement Learning
```

---

## 1. 🎯 Supervised Learning

In **Supervised Learning**, the model learns from **labeled data**.

This means the training data contains both:

* **Input (X)** → Features
* **Output (Y)** → Correct answer / Label

```text
Labeled Data
     ↓
[ Input + Output ]
     ↓
 ML Algorithm
     ↓
 Trained Model
     ↓
Prediction
```

### A. Regression

Regression is used when the output is a **continuous numerical value**.

**Examples:**

* 🏠 Predicting house prices
* 🌡️ Predicting temperature
* 💰 Predicting salary

```text
House Features → ML Model → ₹45,00,000
```

### B. Classification

Classification is used when the output belongs to a **category or class**.

**Examples:**

* 📧 Spam → `Spam / Not Spam`
* 🎓 Placement → `Placed / Not Placed`
* 🩺 Diagnosis → `Positive / Negative`

```text
Email → ML Model → Spam / Not Spam
```

---

## 2. 🔍 Unsupervised Learning

In **Unsupervised Learning**, the data contains **inputs but no output labels**.

The model tries to discover hidden patterns or structures in the data itself.

```text
Unlabeled Data
      ↓
 ML Algorithm
      ↓
Discover Patterns
```

### A. Clustering

Clustering groups **similar data points together**.

**Example:** Customer segmentation.

```text
Customers
    ↓
Clustering
    ↓
┌─────────┬─────────┬─────────┐
│ Group A │ Group B │ Group C │
│ Budget  │ Regular │ Premium │
└─────────┴─────────┴─────────┘
```

Businesses can use these groups to better understand different types of customers.

### B. Dimensionality Reduction

Dimensionality Reduction reduces the **number of features/variables** while trying to preserve important information.

A popular technique is **PCA (Principal Component Analysis)**.

```text
Many Features
X1 X2 X3 X4 X5 X6 X7
          ↓
         PCA
          ↓
Fewer Features
   PC1 PC2 PC3
```

This can make complex datasets easier to analyze and process.

### C. Anomaly Detection

Anomaly Detection finds **unusual or abnormal data points**.

```text
● ● ● ● ● ● ● ●        X
Normal Data          Anomaly
```

**Examples:**

* 💳 Suspicious transactions
* 🔐 Unusual login activity
* ⚙️ Abnormal machine behavior

### D. Association Rule Learning

Association Rule Learning discovers **relationships between items or variables**.

A famous example is the **Beer and Diapers** story.

```text
Customer buys A
      ↓
Often also buys B
      ↓
A → B relationship
```

It is commonly associated with **market basket analysis** and recommendation patterns.

---

## 3. 🔄 Semi-Supervised Learning

Semi-Supervised Learning combines:

> **A small amount of labeled data + a large amount of unlabeled data**

```text
Small Labeled Dataset
        +
Large Unlabeled Dataset
        ↓
Semi-Supervised Learning
        ↓
     Model
```

This is useful when collecting data is easy, but **manually labeling it is expensive or time-consuming**.

---

## 4. 🎮 Reinforcement Learning

In **Reinforcement Learning (RL)**, an **agent** learns by interacting with an **environment**.

The agent performs actions and receives **rewards or penalties**.

```text
        ┌───────────────┐
        │  Environment  │
        └───────┬───────┘
                │
           State/Reward
                ↓
             Agent
                │
              Action
                │
                └──────────→ Environment
```

The basic idea is:

```text
Agent
  ↓
Takes Action
  ↓
Environment Responds
  ↓
Reward / Penalty
  ↓
Agent Learns
  ↓
Better Future Actions
```

A famous example is **AlphaGo**, where reinforcement-learning techniques helped develop systems capable of playing the game Go at an extremely high level.

---

## 📊 Quick Comparison

| Type                | Training Data                | Main Goal                         | Example                |
| ------------------- | ---------------------------- | --------------------------------- | ---------------------- |
| **Supervised**      | Labeled                      | Predict outputs                   | House price prediction |
| **Unsupervised**    | Unlabeled                    | Discover patterns                 | Customer segmentation  |
| **Semi-Supervised** | Few labeled + many unlabeled | Learn with limited labels         | Image classification   |
| **Reinforcement**   | Rewards / penalties          | Learn actions through interaction | Game-playing agent     |

---

## 🧠 Quick Revision

```text
SUPERVISED
→ Data has answers
→ Regression + Classification

UNSUPERVISED
→ Data has no answers
→ Find hidden patterns
→ Clustering + Dimensionality Reduction
→ Anomaly Detection + Association Rules

SEMI-SUPERVISED
→ Small labeled data + Large unlabeled data

REINFORCEMENT LEARNING
→ Agent + Environment
→ Actions + Rewards/Penalties
→ Learn through interaction
```

### Easy Way to Remember

**Supervised** → Learn with a **teacher** 👨‍🏫
**Unsupervised** → Find patterns **yourself** 🔍
**Semi-Supervised** → Learn with **a little help** 🧩
**Reinforcement** → Learn through **experience and rewards** 🎮

---

> **Key Takeaway:** The type of Machine Learning approach depends largely on what kind of data and feedback are available and what problem we want the model to solve.
