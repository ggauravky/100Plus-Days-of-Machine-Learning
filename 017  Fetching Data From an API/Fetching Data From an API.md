# Fetching Data from APIs

## What is an API?

* **API (Application Programming Interface)** allows two software systems to communicate.
* It works like a **bridge between an application and a data source**.
* Instead of directly accessing a database, an application requests only the required data through the API.

### Simple Example

A travel booking website may get:

```text
Flight Website
     ↓
     API
     ↓
Airline Database
```

The booking website gets updated flight information without directly accessing the airline's internal database.

---

## API Response Format

APIs commonly return data in **JSON format**.

Example:

```json
{
  "id": 101,
  "title": "Interstellar",
  "rating": 8.6
}
```

JSON looks similar to a Python **dictionary**.

Nested JSON can be understood more easily using a **JSON viewer**.

---

# Using an API in Python

## 1. Install / Import Libraries

```python
import requests
import pandas as pd
```

* `requests` → fetch data from API
* `pandas` → convert data into a DataFrame

---

## 2. API Key

Some APIs require an **API key**.

Example flow:

```text
Create Account
     ↓
Generate API Key
     ↓
Add Key to API Request
     ↓
Receive Data
```

> [!IMPORTANT]
> Never expose your private API key publicly or commit it directly to GitHub.

---

## 3. Fetch Data from API

Example using a movie API:

```python
url = "API_URL"

response = requests.get(url)

data = response.json()
```

Here:

* `requests.get()` → sends request
* `response.json()` → converts JSON response into Python data

---

## 4. Extract Required Fields

Suppose the API returns many fields, but we only need:

* Movie ID
* Title
* Release date
* Rating

Example:

```python
movies = pd.DataFrame(data["results"])[
    ["id", "title", "release_date", "vote_average"]
]
```

Now the data becomes structured like:

|  id | title   | release_date | vote_average |
| --: | ------- | ------------ | -----------: |
| 101 | Movie A | 2026-01-10   |          8.2 |
| 102 | Movie B | 2026-02-15   |          7.5 |

---

# Fetching Multiple Pages

Most APIs return limited records per request.

Example:

```text
Page 1 → 20 Movies
Page 2 → 20 Movies
Page 3 → 20 Movies
```

To collect more data, loop through pages.

```python
all_movies = []

for page in range(1, 6):
    url = f"API_URL&page={page}"

    response = requests.get(url)
    data = response.json()

    df = pd.DataFrame(data["results"])

    all_movies.append(df)
```

Combine all pages:

```python
final_df = pd.concat(all_movies, ignore_index=True)
```

`ignore_index=True` creates a fresh continuous index.

---

# Export Data to CSV

After collecting and cleaning the data:

```python
final_df.to_csv("movies.csv", index=False)
```

This creates:

```text
movies.csv
```

`index=False` prevents Pandas from saving the DataFrame index as an extra column.

---

# Complete Flow

```text
API
 ↓
Send Request
 ↓
Receive JSON
 ↓
Extract Required Fields
 ↓
Convert to DataFrame
 ↓
Loop Through Pages
 ↓
Combine Data
 ↓
Export to CSV
```

---

## Useful Platforms

* **TMDB** → Movie-related API data
* **RapidAPI** → Collection of many APIs
* **Kaggle** → Share and explore datasets

---

## 🧠 Quick Revision

* **API** allows applications to communicate and exchange data.
* API responses commonly come in **JSON format**.
* Use `requests.get()` to fetch API data.
* Use `.json()` to convert the response into Python data.
* Use **Pandas** to convert JSON records into a DataFrame.
* Loop through API pages to collect large datasets.
* Use `pd.concat()` to combine multiple DataFrames.
* Use `.to_csv()` to export the final dataset.
* Keep your **API key private**.
