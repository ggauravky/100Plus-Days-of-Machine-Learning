# Tensors in Machine Learning

## What is a Tensor?

A **tensor** is a general-purpose data structure used to store **numerical data in multiple dimensions**.

It can be thought of as a **multidimensional array**.

Tensors provide a common way to represent:

* A single number
* A list of numbers
* A table of numbers
* Images
* Videos
* Time-series data
* Text representations

Tensor-based data structures are fundamental in deep-learning frameworks such as **TensorFlow** and **PyTorch**.

> [!IMPORTANT]
> **Scalar, Vector, and Matrix are all special cases of tensors.**

```text
Tensor
│
├── 0D → Scalar
├── 1D → Vector
├── 2D → Matrix
├── 3D → Collection of matrices
├── 4D → Collection of 3D tensors
└── 5D → Collection of 4D tensors
```

---

## Why Do We Use Tensors?

Machine-learning models work mainly with **numbers**.

Real-world information therefore needs to be converted into numerical structures before it can be processed.

For example:

```text
Image
   ↓
Pixel values
   ↓
Tensor
   ↓
Machine Learning Model
```

Similarly:

```text
Text → Numbers → Tensor → Model

Video → Pixel values → Tensor → Model

Tabular Data → Numerical values → Tensor → Model
```

Tensors make it possible to organize all these different types of numerical data using one general structure.

---

# Types of Tensors

The type of tensor is commonly described using its **rank**, which represents the number of axes or dimensions.

---

## 0D Tensor — Scalar

A **0D tensor** contains only **one numerical value**.

Example:

```text
5
```

or

```text
3.14
```

It has:

* **Rank:** 0
* **Axes:** 0
* **Shape:** `()`
* **Size:** 1

### Example

```python
x = 10
```

Conceptually, `10` represents a scalar value.

Examples in machine learning may include:

* Loss value
* Learning rate
* Accuracy value
* Single temperature measurement

---

## 1D Tensor — Vector

A **1D tensor** is a collection of numbers arranged along **one axis**.

Example:

```text
[10, 20, 30, 40]
```

It has:

* **Rank:** 1
* **Axes:** 1
* **Shape:** `(4,)`
* **Size:** 4

A 1D tensor is commonly called a **vector**.

### Example in Machine Learning

Suppose one student is represented using:

```text
[Age, Marks, Attendance]
```

Example:

```text
[20, 85, 92]
```

This single student's information can be represented using a **1D tensor**.

---

## 2D Tensor — Matrix

A **2D tensor** is a collection of vectors arranged in **rows and columns**.

It is commonly called a **matrix**.

Example:

```text
[
  [1, 2, 3],
  [4, 5, 6]
]
```

This tensor has:

```text
Rows    = 2
Columns = 3
```

Therefore:

* **Rank:** 2
* **Axes:** 2
* **Shape:** `(2, 3)`
* **Size:** `2 × 3 = 6`

### Example in Machine Learning

Suppose we have information about three students:

```text
Student 1 → [20, 85, 92]
Student 2 → [21, 78, 88]
Student 3 → [20, 91, 95]
```

The complete dataset becomes:

```text
[
  [20, 85, 92],
  [21, 78, 88],
  [20, 91, 95]
]
```

Here:

```text
Rows    → Students / Samples
Columns → Features
```

This is one of the most common representations of **tabular datasets**.

---

## 3D Tensor

A **3D tensor** can be understood as a **collection of matrices stacked together**.

Example:

```text
Matrix 1
[
 [1, 2],
 [3, 4]
]

Matrix 2
[
 [5, 6],
 [7, 8]
]
```

Together:

```text
[
  [
    [1, 2],
    [3, 4]
  ],

  [
    [5, 6],
    [7, 8]
  ]
]
```

Its shape is:

```text
(2, 2, 2)
```

It has:

* **Rank:** 3
* **Axes:** 3
* **Size:** `2 × 2 × 2 = 8`

### Common Uses

3D tensors are often used for:

* **Time-series data**
* **Sequences**
* **Individual color images**

A common time-series representation is:

```text
Samples × Time Steps × Features
```

Example:

```text
100 patients
30 days of measurements
5 measurements per day
```

Tensor shape:

```text
(100, 30, 5)
```

---

# 4D Tensor

A **4D tensor** is a collection of multiple **3D tensors**.

A common use is storing a **batch of images**.

For example, one color image may have:

```text
Height × Width × Channels
```

Suppose an image has:

```text
Height   = 224
Width    = 224
Channels = 3
```

Its shape may be:

```text
(224, 224, 3)
```

Here:

```text
3 Channels
│
├── Red
├── Green
└── Blue
```

If we have **32 images**, they can be stored together in a 4D tensor:

```text
Batch × Height × Width × Channels
```

Shape:

```text
(32, 224, 224, 3)
```

Meaning:

* `32` → number of images
* `224` → image height
* `224` → image width
* `3` → RGB color channels

> [!NOTE]
> Some deep-learning libraries may use a different axis order, such as:
>
> ```text
> Batch × Channels × Height × Width
> ```
>
> The important idea is that the tensor contains the same dimensions, even if their order changes.

---

# 5D Tensor

A **5D tensor** is commonly used to represent **batches of videos**.

A video contains:

```text
Multiple Frames
      ↓
Each Frame is an Image
```

Therefore, video data needs an additional **time/frame dimension**.

A common representation is:

```text
Batch × Frames × Height × Width × Channels
```

Example:

```text
(8, 30, 224, 224, 3)
```

Meaning:

* `8` → videos
* `30` → frames per video
* `224` → frame height
* `224` → frame width
* `3` → RGB channels

So:

```text
Image Batch
Batch × Height × Width × Channels
                 ↓
              4D Tensor
```

while:

```text
Video Batch
Batch × Frames × Height × Width × Channels
                    ↓
                 5D Tensor
```

---

# Important Tensor Terminology

## 1. Rank

The **rank** of a tensor is the **number of axes or dimensions** it has.

Examples:

| Tensor    | Example Shape          | Rank |
| --------- | ---------------------- | ---: |
| Scalar    | `()`                   |    0 |
| Vector    | `(5,)`                 |    1 |
| Matrix    | `(3, 4)`               |    2 |
| 3D Tensor | `(10, 5, 3)`           |    3 |
| 4D Tensor | `(32, 224, 224, 3)`    |    4 |
| 5D Tensor | `(8, 30, 224, 224, 3)` |    5 |

> [!TIP]
> **Rank = How many numbers are required to describe the shape.**

For example:

```text
Shape = (10, 20, 30)
```

There are **3 dimensions**, so:

```text
Rank = 3
```

---

## 2. Axes

An **axis** represents one particular direction or dimension of a tensor.

For a matrix:

```text
[
 [1, 2, 3],
 [4, 5, 6]
]
```

There are two axes:

```text
Axis 0 → Rows
Axis 1 → Columns
```

So this tensor has:

```text
Rank = 2
```

---

## 3. Shape

The **shape** tells us how many elements exist along each axis.

Example:

```text
[
 [1, 2, 3],
 [4, 5, 6]
]
```

There are:

```text
2 rows
3 columns
```

Therefore:

```text
Shape = (2, 3)
```

Another example:

```text
Shape = (32, 224, 224, 3)
```

could represent:

```text
32 images
224 height
224 width
3 color channels
```

---

## 4. Size

The **size** of a tensor is the total number of elements stored inside it.

It is calculated by multiplying all dimensions of its shape.

### Example

If:

```text
Shape = (2, 3)
```

Then:

```text
Size = 2 × 3
     = 6
```

For:

```text
Shape = (2, 3, 4)
```

Size:

```text
2 × 3 × 4 = 24
```

Therefore:

$$
\text{Size} = d_1 \times d_2 \times d_3 \times \dots \times d_n
$$

where each \(d\) represents the length of one tensor dimension.

---

# Rank vs Shape vs Size

These three terms are easy to confuse.

Suppose:

```text
Shape = (3, 4, 5)
```

Then:

```text
Rank = 3
```

because there are **3 axes**.

```text
Shape = (3, 4, 5)
```

because the axes contain **3, 4, and 5 elements** respectively.

```text
Size = 3 × 4 × 5
     = 60
```

### Quick Comparison

| Term      | Meaning                            |
| --------- | ---------------------------------- |
| **Rank**  | Number of dimensions/axes          |
| **Axis**  | One particular dimension           |
| **Shape** | Number of elements along each axis |
| **Size**  | Total number of elements           |

> [!IMPORTANT]
>
> ```text
> Rank → How many dimensions?
>
> Shape → How large is each dimension?
>
> Size → How many total values?
> ```

---

# Tensors in Different Types of Machine Learning Data

## 1. Tabular Data

Tabular data usually looks like:

| Age | Marks | Attendance |
| --: | ----: | ---------: |
|  20 |    85 |         92 |
|  21 |    78 |         88 |
|  20 |    91 |         95 |

One row can be represented as a **1D tensor**:

```text
[20, 85, 92]
```

The complete dataset can be represented as a **2D tensor**:

```text
Samples × Features
```

Example:

```text
(1000, 10)
```

means:

```text
1000 samples
10 features per sample
```

---

## 2. Natural Language Processing

Computers cannot directly understand words like:

```text
"Machine learning is interesting"
```

Text is first converted into numerical representations.

For example:

```text
Text
 ↓
Tokenization
 ↓
Token IDs / Embeddings
 ↓
Tensor
 ↓
NLP Model
```

Depending on how the text is represented, NLP models may work with **2D, 3D, or higher-dimensional tensors**.

A common sequence representation is:

```text
Batch × Sequence Length × Features
```

---

## 3. Time-Series Data

Time-series data contains observations collected over **time**.

Examples:

* Daily temperature
* Stock prices
* Sensor readings
* Heart-rate measurements
* Electricity consumption

A common 3D representation is:

```text
Samples × Time Steps × Features
```

Example:

```text
(1000, 30, 5)
```

meaning:

```text
1000 samples
30 time steps
5 features at each time step
```

---

## 4. Image Data

An individual color image can be represented using:

```text
Height × Width × Channels
```

Example:

```text
(224, 224, 3)
```

This is a **3D tensor**.

Multiple images together form a batch:

```text
Batch × Height × Width × Channels
```

Example:

```text
(32, 224, 224, 3)
```

This becomes a **4D tensor**.

---

## 5. Video Data

A video is essentially a sequence of images or **frames**.

```text
Video
 ↓
Frame 1
Frame 2
Frame 3
...
Frame N
```

A batch of videos can commonly be represented as:

```text
Batch × Frames × Height × Width × Channels
```

This forms a **5D tensor**.

---

# Understanding Tensor Dimensions Visually

```text
0D Tensor
5

        ↓

1D Tensor
[1, 2, 3]

        ↓

2D Tensor
[
 [1, 2, 3],
 [4, 5, 6]
]

        ↓

3D Tensor
[
 Matrix 1,
 Matrix 2,
 Matrix 3
]

        ↓

4D Tensor
[
 3D Tensor 1,
 3D Tensor 2,
 ...
]

        ↓

5D Tensor
[
 4D Tensor 1,
 4D Tensor 2,
 ...
]
```

The pattern is:

```text
Higher-Dimensional Tensor
=
Collection of Lower-Dimensional Tensors
```

---

# Simple Hierarchy to Remember

```text
Single Number
     ↓
Scalar
0D Tensor

Collection of Scalars
     ↓
Vector
1D Tensor

Collection of Vectors
     ↓
Matrix
2D Tensor

Collection of Matrices
     ↓
3D Tensor

Collection of 3D Tensors
     ↓
4D Tensor

Collection of 4D Tensors
     ↓
5D Tensor
```

---

## 🧠 Quick Revision

* A **tensor** is a multidimensional numerical data structure widely used in machine learning and deep learning.
* **0D Tensor → Scalar**
* **1D Tensor → Vector**
* **2D Tensor → Matrix**
* **Rank** tells the number of **axes/dimensions**.
* **Shape** tells the number of elements along each axis.
* **Size** is the total number of elements and is found by multiplying all shape dimensions.
* **Tabular data** is commonly represented using **2D tensors**, while batches of images and videos commonly use **4D and 5D tensors**.
* Tensors allow different kinds of data such as **text, images, videos, and time series** to be represented numerically for machine-learning models.
