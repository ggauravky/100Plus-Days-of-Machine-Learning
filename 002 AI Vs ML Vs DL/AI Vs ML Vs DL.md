# AI, Machine Learning & Deep Learning

## 1. AI, ML aur DL ka Structure

* **AI (Artificial Intelligence)** sabse broad field hai.
* **ML (Machine Learning)**, AI ka subset hai.
* **DL (Deep Learning)**, ML ka subset hai.

```text
Artificial Intelligence (AI)
        ↓
Machine Learning (ML)
        ↓
Deep Learning (DL)
```

**Simple meaning:**

* **AI** → Machine ko intelligent banana.
* **ML** → Machine ko data se sikhana.
* **DL** → Neural Networks ki help se complex patterns sikhana.

---

## 2. Artificial Intelligence (AI) Kya Hai?

* AI ka goal machines ko **human-like intelligent tasks** perform karne layak banana hai.
* AI systems decision making, problem solving, language understanding, planning etc. kar sakte hain.
* Har AI system Machine Learning use kare, ye zaroori nahi hai.

**Examples:**

* Chatbots
* Google Maps route suggestions
* Voice assistants
* Chess-playing programs
* Recommendation systems

> **AI = Machines ko intelligently tasks perform karwana.**

---

## 3. Expert Systems aur Symbolic AI

### Symbolic AI

* Symbolic AI mein intelligence **rules aur logic** ke through banayi jaati hai.
* Rules humans/programmers define karte hain.
* System automatically data se learn nahi karta.

**Example:**

```text
IF temperature > 38°C
AND cough = Yes
THEN possible fever
```

### Expert System

* Expert System ek AI program hai jo kisi **human expert ke knowledge** ko rules ke form mein use karta hai.
* Iske mainly do parts hote hain:

```text
Knowledge Base + Inference Engine → Decision
```

* **Knowledge Base** → Facts aur rules store karta hai.
* **Inference Engine** → Rules use karke decision nikalta hai.

---

## 4. Machine Learning (ML) ka Concept

* ML, **Artificial Intelligence ka subset** hai.
* Isme machine ko har rule manually nahi bataya jata.
* Machine **data se patterns learn** karti hai.
* Learned patterns ko new data par prediction ke liye use kiya jata hai.

```text
Training Data
     ↓
ML Algorithm
     ↓
Trained Model
     ↓
New Data → Prediction
```

**Example — Spam Detection:**

Machine ko bahut saare:

* Spam emails
* Normal emails

diye jaate hain.

Machine patterns learn karke new email ko:

```text
Spam / Not Spam
```

classify kar sakti hai.

**Other Examples:**

* House price prediction
* Fraud detection
* Recommendation systems
* Customer churn prediction

---

## 5. Deep Learning (DL) Kya Hai?

* Deep Learning, **Machine Learning ka subset** hai.
* DL mein **Neural Networks** use hote hain.
* Multiple hidden layers hone ki wajah se ise **Deep Learning** kaha jata hai.
* Ye large aur complex data ke liye useful hai.

```text
Input
  ↓
Neural Network Layers
  ↓
Learn Patterns
  ↓
Output
```

**Example:**

Cat aur Dog ki thousands of images dene par Deep Learning model automatically important visual features learn kar sakta hai.

**Common Uses:**

* Image Recognition
* Speech Recognition
* NLP
* Computer Vision
* Generative AI

---

## 6. Machine Learning vs Deep Learning

| Machine Learning                   | Deep Learning                                         |
| ---------------------------------- | ----------------------------------------------------- |
| AI ka subset                       | ML ka subset                                          |
| Small/medium data par bhi useful   | Usually large data se benefit karta hai               |
| Less computing power               | More computing power                                  |
| Feature engineering often required | Features automatically learn kar sakta hai            |
| Structured data ke liye strong     | Images, audio, text jaise complex data ke liye strong |
| Training generally faster          | Training generally slower                             |

### ML kab use karein?

* Data small/medium ho.
* Structured/tabular data ho.
* Computing resources limited hon.
* Explainable model chahiye.

**Example:** House price prediction.

### DL kab use karein?

* Large amount of data ho.
* Images, audio ya text process karna ho.
* Problem mein complex patterns hon.
* Enough computing power available ho.

**Example:** Image recognition.

---

## Quick Revision

```text
AI
│
├── Symbolic / Rule-Based AI
│
└── Machine Learning
       │
       └── Deep Learning
```

* **AI** → Machines ko intelligent banana.
* **Symbolic AI** → Rules + Logic.
* **Expert System** → Expert knowledge + rules se decisions.
* **ML** → Data se patterns learn karna.
* **DL** → Neural Networks se complex patterns learn karna.
* **ML** → Structured/smaller data ke liye useful.
* **DL** → Images, audio, text aur large/complex data ke liye useful.
