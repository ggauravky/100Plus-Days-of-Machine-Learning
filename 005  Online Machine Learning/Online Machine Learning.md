# 🌐 Online Machine Learning

## 1. What is Online Learning?

**Online Machine Learning** is a technique where the model learns **incrementally** from new data.

Instead of training again on the entire dataset, the model learns from **small chunks of incoming data**.

```text
New Data → Model → Update
              ↑
         continuously
```

> 📌 **Batch Learning:** Train → Deploy → Retrain later
> 📌 **Online Learning:** Train → Deploy → Keep updating with new data

---

## 2. How Does Online Learning Work?

The model is deployed in production and updated as new data becomes available.

```text
Initial Data
     ↓
Train Model
     ↓
Deploy
     ↓
New Data Arrives
     ↓
Model Learns
     ↓
Updated Model
     ↓
More New Data...
```

### 🛒 Real-Life Example — E-commerce

Suppose an online shopping website recommends products.

A user:

* Searches for laptops
* Clicks gaming laptops
* Buys gaming accessories

New behavior can be incorporated into future model updates.

```text
User Activity
     ↓
New Data
     ↓
Model Update
     ↓
Better Recommendations
```

This helps the system adapt to **changing user interests**.

---

# 3. When is Online Learning Useful?

## 📈 A. Volatile / Changing Data

Online learning is useful when patterns change frequently.

Examples:

* 📈 Financial-market data
* 🛒 E-commerce behavior
* 🎬 Recommendation systems
* 💬 User interactions

The model can adapt as newer data becomes available.

---

## 💾 B. Very Large Datasets

Sometimes the complete dataset is **too large to fit into RAM**.

Instead of:

```text
Huge Dataset → RAM ❌
```

we can process it like:

```text
Huge Dataset
     ↓
Batch 1 → Learn
Batch 2 → Learn
Batch 3 → Learn
Batch 4 → Learn
...
```

This idea is known as **Out-of-Core Learning**.

---

# 4. Common Examples

### 🎬 YouTube Recommendations

User behavior keeps changing.

```text
Watch Video
    ↓
New Interaction
    ↓
Recommendation System
    ↓
Future Recommendations
```

### ⌨️ Predictive Text

A keyboard can adapt based on typing patterns.

Example:

```text
"I am going ___"

        ↓

home
there
today
```

### 💬 Chatbots

Systems can use continuously arriving interaction data as part of an online-learning pipeline when their models are designed to support incremental updates.

---

# 5. Tools for Online Learning

## 🐍 Scikit-Learn

Some Scikit-Learn models support incremental training using:

```python
model.partial_fit(X, y)
```

`partial_fit()` allows supported models to learn from **small batches of data** instead of retraining from scratch every time.

---

## 🌊 River

**River** is a Python library designed for **online/streaming machine learning**.

```text
Streaming Data → River Model → Incremental Updates
```

---

## ⚡ Vowpal Wabbit

**Vowpal Wabbit** is another tool designed for efficient machine learning on large and streaming datasets.

---

# 6. Learning Rate

The **Learning Rate** controls how strongly model parameters change during learning.

In an online setting, this affects how quickly a model adapts to new observations.

### Too High 📈

```text
New Data
   ↓
Very Large Update
   ↓
Model can become unstable
```

### Too Low 📉

```text
New Data
   ↓
Very Small Update
   ↓
Model adapts slowly
```

So choosing an appropriate learning rate is important.

---

# 7. Out-of-Core Learning

**Out-of-Core Learning** is useful when the dataset is too large to fit into memory.

### Example

Suppose:

```text
Dataset = 100 GB
Available RAM = 8 GB
```

Loading the entire dataset:

```text
100 GB → 8 GB RAM ❌
```

Instead:

```text
100 GB Dataset
      ↓
Small Chunk
      ↓
Train / Update
      ↓
Next Chunk
      ↓
Train / Update
      ↓
Continue...
```

This allows us to work with datasets larger than available RAM.

---

# 8. ⚠️ Risks of Online Learning

Because the model learns from incoming data, **bad data can negatively affect it**.

Examples:

* Corrupted data
* Incorrect inputs
* Malicious/manipulated data
* Sudden unusual patterns

```text
Bad Incoming Data
       ↓
Model Learns From It
       ↓
Performance May Decrease
```

This can contribute to problems such as **catastrophic interference/forgetting** or unwanted bias.

---

## 🛡️ Monitoring is Important

Online models should be continuously monitored.

```text
Online Model
     ↓
Monitor Performance
     ↓
Performance Good? ── Yes → Continue
     │
     No
     ↓
Investigate / Stop Updates
     ↓
Rollback if necessary
```

Keeping previous stable model versions can help if an update causes performance degradation.

---

# 9. 📊 Batch Learning vs Online Learning

| Feature                     | 📦 Batch Learning       | 🌐 Online Learning              |
| --------------------------- | ----------------------- | ------------------------------- |
| Learning                    | Entire batch            | Incrementally                   |
| New Data                    | Retrain periodically    | Update incrementally            |
| Model Updates               | Less frequent           | More frequent/continuous        |
| Changing Data               | Less responsive         | Better suited                   |
| Huge Datasets               | Can be expensive        | Can process small chunks        |
| Implementation              | Usually simpler         | More complex                    |
| Monitoring                  | Important               | **Very important**              |
| Risk from bad incoming data | Lower during deployment | Higher if automatically learned |

---

# 🧠 Quick Revision

```text
ONLINE MACHINE LEARNING
        ↓
Incremental Learning
        ↓
Small / Sequential Data Chunks
        ↓
Model Keeps Updating
        ↓
Useful For:
├── Changing Data
├── Streaming Data
└── Huge Datasets

Important Concepts:
├── partial_fit()
├── Learning Rate
├── Out-of-Core Learning
└── Model Monitoring
```

## 📌 Key Points

* **Online Learning = Incremental Learning**
* Model learns from small, sequential chunks of data.
* Useful when data patterns change frequently.
* Can help process datasets larger than RAM.
* `partial_fit()` is supported by some Scikit-Learn models.
* **River** and **Vowpal Wabbit** support streaming/online ML.
* Learning rate affects how quickly the model adapts.
* Bad incoming data can damage model performance.
* Strong monitoring and rollback strategies are important.

---

## ⚡ One-Line Definition

> **Online Machine Learning is an approach where a model learns incrementally from incoming data instead of repeatedly retraining from scratch on the entire dataset.**
