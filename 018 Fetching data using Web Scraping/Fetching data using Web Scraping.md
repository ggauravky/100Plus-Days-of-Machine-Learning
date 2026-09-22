# Web Scraping with Python

## What is Web Scraping?

* **Web Scraping** means extracting data from websites using code.
* It is useful when:

  * No official API is available.
  * API access is limited.
  * Data is available only on web pages.

Example:

```text
Website
   ↓
HTML Page
   ↓
Python Scraper
   ↓
Required Data
   ↓
Pandas DataFrame
```

---

## Main Libraries

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

* `requests` → downloads webpage HTML.
* `BeautifulSoup` → parses and searches HTML.
* `pandas` → stores data in table format.

---

## Fetch a Webpage

```python
url = "https://example.com"

response = requests.get(url)

print(response.text)
```

`response.text` contains the raw **HTML** of the webpage.

---

## Handling 403 Forbidden

Some websites block requests that do not look like normal browser requests.

Use headers:

```python
headers = {
    "User-Agent": "Mozilla/5.0"
}

response = requests.get(url, headers=headers)
```

> [!NOTE]
> Headers can help identify your request as coming from a browser, but you should still respect the site's rules, robots.txt, and terms of service.

---

## Parse HTML with BeautifulSoup

```python
soup = BeautifulSoup(response.text, "html.parser")
```

Now we can search HTML elements.

Example:

```python
soup.find("h1")
```

Find all headings:

```python
soup.find_all("h2")
```

---

## Extract Data

Suppose company details are inside repeating HTML containers.

```python
companies = soup.find_all("div", class_="company-card")
```

Loop through them:

```python
names = []
ratings = []

for company in companies:
    name = company.find("h2").text.strip()
    rating = company.find("span", class_="rating").text.strip()

    names.append(name)
    ratings.append(rating)
```

Possible fields:

* Company name
* Rating
* Review count
* Headquarters
* Company type
* Company age
* Number of employees

---

## Clean Extracted Text

HTML text may contain spaces and line breaks.

Use:

```python
text = text.strip()
```

Example:

```text
"\n   Google   \n"
```

becomes:

```text
"Google"
```

---

## Create a DataFrame

```python
df = pd.DataFrame({
    "Company": names,
    "Rating": ratings
})
```

Example:

| Company   | Rating |
| --------- | -----: |
| Company A |    4.2 |
| Company B |    3.9 |

---

## Scraping Multiple Pages

If URLs contain page numbers:

```text
page=1
page=2
page=3
```

we can automate scraping.

```python
all_data = []

for page in range(1, 6):

    url = f"https://example.com?page={page}"

    response = requests.get(url, headers=headers)

    soup = BeautifulSoup(response.text, "html.parser")

    companies = soup.find_all(
        "div",
        class_="company-card"
    )

    for company in companies:
        name = company.find("h2").text.strip()

        all_data.append(name)
```

Then convert the collected data into a DataFrame.

---

## Complete Workflow

```text
Inspect Website
      ↓
Find Required HTML Tags / Classes
      ↓
Send Request
      ↓
Receive HTML
      ↓
BeautifulSoup Parsing
      ↓
Extract Required Fields
      ↓
Clean Data
      ↓
Store in Lists
      ↓
Create DataFrame
      ↓
Repeat for Multiple Pages
```

---

## 🧠 Quick Revision

* **Web scraping** extracts data directly from webpages.
* Use `requests` to fetch HTML.
* Use **BeautifulSoup** to parse HTML.
* Browser **Developer Tools** help identify tags and classes.
* `find()` finds one element.
* `find_all()` finds multiple matching elements.
* `.text.strip()` helps clean extracted text.
* Store extracted data in lists and create a **Pandas DataFrame**.
* Use loops to scrape multiple pages automatically.
