# Data Science & ML Environment Setup

## 1. Anaconda

- **Anaconda** helps manage Python, packages, and virtual environments.
- Download and install Anaconda, then use **Anaconda Prompt** or terminal.

### Useful Conda Commands

```bash
conda --version
conda create -n ml_env python=3.11
conda activate ml_env
conda deactivate
conda env list
conda install numpy pandas matplotlib scikit-learn
```

---

## 2. Jupyter Notebook & Spyder

- **Jupyter Notebook** is widely used in data science because code, output, graphs, and notes can be kept together.
- **Spyder** provides an IDE-style environment for Python.

```bash
jupyter notebook
```

Install if required:

```bash
conda install jupyter
conda install spyder
```

---

## 3. Virtual Environments

Virtual environments keep dependencies of different projects separate.

```bash
conda create -n project1 python=3.11
conda activate project1
pip install pandas numpy scikit-learn
```

> [!TIP]
> Use a separate environment for important projects to avoid package-version conflicts.

---

## 4. Kaggle Datasets

Kaggle provides datasets and cloud notebooks for ML projects.

### Kaggle API Setup

Install Kaggle:

```bash
pip install kaggle
```

On Kaggle:

```text
Profile → Settings → API → Create New Token
```

This downloads:

```text
kaggle.json
```

### Linux / Google Colab Setup

```bash
mkdir -p ~/.kaggle
cp kaggle.json ~/.kaggle/
chmod 600 ~/.kaggle/kaggle.json
```

Download a dataset:

```bash
kaggle datasets download -d username/dataset-name
```

Unzip it:

```bash
unzip dataset-name.zip
```

List files:

```bash
ls
```

---

## 5. Google Colab

- Runs Python directly in the browser.
- Useful for ML because **GPU/TPU acceleration** may be available.
- Can connect directly with **Google Drive**.

### Mount Google Drive

```python
from google.colab import drive
drive.mount('/content/drive')
```

### Enable GPU

```text
Runtime → Change runtime type → GPU
```

Check GPU:

```bash
!nvidia-smi
```

---

## 6. Kaggle API in Google Colab

Upload `kaggle.json`:

```python
from google.colab import files
files.upload()
```

Then:

```bash
!mkdir -p ~/.kaggle
!cp kaggle.json ~/.kaggle/
!chmod 600 ~/.kaggle/kaggle.json
```

Download dataset:

```bash
!kaggle datasets download -d username/dataset-name
```

Extract:

```bash
!unzip dataset-name.zip
```

---

## 7. Basic Data Loading

```python
import pandas as pd

df = pd.read_csv("data.csv")
df.head()
```

Check dataset:

```python
df.shape
df.info()
df.describe()
```

---

## 🧠 Quick Revision

- **Anaconda** simplifies Python package and environment management.
- **Jupyter Notebook** is useful for interactive ML experiments.
- Use **virtual environments** to avoid dependency conflicts.
- **Kaggle** provides datasets, notebooks, and an API for downloading data.
- `kaggle.json` contains Kaggle API credentials and should be kept private.
- **Google Colab** provides browser-based notebooks and optional GPU/TPU support.
- Google Drive can be mounted directly inside Colab.
