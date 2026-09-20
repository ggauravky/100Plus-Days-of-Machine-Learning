# Loading JSON and SQL Data with Pandas

## 1. Working with JSON Data

### What is JSON?

* **JSON (JavaScript Object Notation)** is a lightweight format used to store and exchange data.
* It is commonly used by:

  * APIs
  * Web applications
  * Configuration files

Example JSON:

```json
[
  {
    "name": "Gaurav",
    "age": 20,
    "city": "Lucknow"
  },
  {
    "name": "Rahul",
    "age": 21,
    "city": "Delhi"
  }
]
```

---

## Load a Local JSON File

Use:

```python
import pandas as pd

df = pd.read_json("data.json")

print(df.head())
```

Output:

| name   | age | city    |
| ------ | --: | ------- |
| Gaurav |  20 | Lucknow |
| Rahul  |  21 | Delhi   |

---

## Load JSON Directly from an API

If an API returns JSON data, Pandas can sometimes read it directly.

```python
import pandas as pd

url = "https://api.example.com/data"

df = pd.read_json(url)
```

Flow:

```text
API
 ↓
JSON Data
 ↓
pd.read_json()
 ↓
DataFrame
```

> [!TIP]
> API JSON structures can differ, so sometimes you may need to fetch and normalize the data separately.

---

# 2. Working with SQL Data

SQL databases store data in **tables**.

Example:

| id | country | population |
| -: | ------- | ---------: |
|  1 | India   | 1400000000 |
|  2 | Japan   |  125000000 |
|  3 | Germany |   84000000 |

---

## Basic Setup

For local MySQL practice:

1. Install **XAMPP**.
2. Start the **MySQL** server.
3. Open **phpMyAdmin**.
4. Create a database.
5. Import a `.sql` file if required.

---

## Install MySQL Connector

```bash
pip install mysql-connector-python
```

---

## Connect Python to MySQL

```python
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="",
    database="world"
)
```

Here:

* `host` → database server
* `user` → MySQL username
* `password` → MySQL password
* `database` → database name

---

## Load SQL Data into Pandas

```python
import pandas as pd

query = "SELECT * FROM city"

df = pd.read_sql_query(query, conn)

print(df.head())
```

Flow:

```text
MySQL Database
      ↓
SQL Query
      ↓
pd.read_sql_query()
      ↓
Pandas DataFrame
```

---

## Filtering Data Using SQL

### Example 1: Specific Country

```python
query = """
SELECT *
FROM city
WHERE CountryCode = 'IND'
"""

df = pd.read_sql_query(query, conn)
```

This loads only cities from **India**.

---

### Example 2: Numerical Condition

```python
query = """
SELECT *
FROM country
WHERE LifeExpectancy > 75
"""

df = pd.read_sql_query(query, conn)
```

This returns countries where:

```text
Life Expectancy > 75 years
```

---

## JSON vs SQL

| JSON                  | SQL                         |
| --------------------- | --------------------------- |
| Common with APIs      | Common with databases       |
| File / web-based data | Table-based structured data |
| `pd.read_json()`      | `pd.read_sql_query()`       |
| Lightweight format    | Powerful querying system    |

---

## 🧠 Quick Revision

* **JSON** is commonly used for APIs and data exchange.
* Use `pd.read_json()` to load JSON into Pandas.
* **SQL** stores structured data in tables.
* Use **XAMPP + phpMyAdmin** for local MySQL practice.
* Use `mysql-connector-python` to connect Python with MySQL.
* Use `pd.read_sql_query()` to load query results into a DataFrame.
* SQL `WHERE` conditions can filter data before loading it into Pandas.
