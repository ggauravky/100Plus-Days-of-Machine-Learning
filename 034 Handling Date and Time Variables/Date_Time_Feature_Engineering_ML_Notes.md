# Handling Date and Time Variables in Machine Learning

> **Topic:** Feature Engineering — Date & Time Features  
> **Tools:** Python, Pandas  
> **Goal:** Convert raw timestamps into useful features for machine learning.

## 1. What Are Date and Time Variables?

- A **date variable** represents a calendar date, such as `2026-10-15`.
- A **time variable** represents a time of day, such as `14:30:45`.
- A **datetime variable** combines both, such as `2026-10-15 14:30:45`.
- Most common ML models cannot directly learn from raw date strings. We usually extract useful **numerical or categorical features** first.

**Basic idea:**

```text
Raw Date / Timestamp
        ↓
Convert to datetime
        ↓
Extract date and time components
        ↓
Calculate durations (when useful)
        ↓
Use the resulting features in an ML model
```

**Example:** An e-commerce order timestamp can tell us the **order month**, **day of the week**, **weekend status**, and **time of purchase**.

---

## 2. Convert Strings to Datetime

Pandas provides **`pd.to_datetime()`** to convert strings or object columns into datetime values.

```python
import pandas as pd

df = pd.DataFrame({
    "order_date": ["2026-10-15 14:30:45", "2026-10-18 09:15:10"]
})

# Convert string values into pandas datetime values
df["order_date"] = pd.to_datetime(df["order_date"])

print(df["order_date"].dtype)  # datetime64[ns]
```

For invalid or missing dates:

```python
df["order_date"] = pd.to_datetime(
    df["order_date"], errors="coerce"
)
```

- `errors="coerce"` converts invalid dates into **`NaT`** (*Not a Time*).
- If dates follow a known format, specify it using `format=...`, for example `format="%d-%m-%Y"`.
- Once converted, use the **`.dt` accessor** to extract date/time information.

> [!IMPORTANT]
> Use `.dt` on a Pandas Series containing datetime-like values, **not on raw string columns**.

---

## 3. Extract Features from a Date

### A. Year, Month, and Day

```python
df["year"] = df["order_date"].dt.year
df["month"] = df["order_date"].dt.month
df["day"] = df["order_date"].dt.day
```

| Input date | Year | Month | Day |
|---|---:|---:|---:|
| `2026-10-15` | 2026 | 10 | 15 |

### B. Month Name and Day Name

```python
df["month_name"] = df["order_date"].dt.month_name()
df["day_name"] = df["order_date"].dt.day_name()
```

- `month_name()` → `October`
- `day_name()` → `Thursday`
- These are **categorical features**; they may need encoding before model training.

### C. Day of Week and Weekend

```python
df["day_of_week"] = df["order_date"].dt.dayofweek

# Monday = 0, ..., Saturday = 5, Sunday = 6
df["is_weekend"] = df["day_of_week"] >= 5
```

**Weekend rule:**

$$
\operatorname{is\_weekend}(d)=
\begin{cases}
1 & \text{if weekday}(d) \in \{5,6\} \\
0 & \text{otherwise}
\end{cases}
$$

- `True` / `1` → Saturday or Sunday.
- `False` / `0` → Monday through Friday.
- Check missing dates first: comparisons involving a missing weekday can result in `False`, which should **not** be interpreted as a known weekday.

### D. Week Number, Quarter, and Semester

```python
# ISO week number (typically 1 to 53)
df["week_number"] = df["order_date"].dt.isocalendar().week

# Calendar quarter (1 to 4)
df["quarter"] = df["order_date"].dt.quarter

# Calendar half-year / semester (1 or 2)
df["semester"] = ((df["order_date"].dt.month - 1) // 6) + 1
```

| Feature | Meaning | Example |
|---|---|---|
| **Week number** | ISO week of the year | Week 42 |
| **Quarter** | A 3-month calendar period | October → Q4 |
| **Semester** | A 6-month calendar period | October → Semester 2 |

**Calendar quarter ranges:**

| Quarter | Months |
|---|---|
| Q1 | January–March |
| Q2 | April–June |
| Q3 | July–September |
| Q4 | October–December |

**Calendar semester ranges:**

- **Semester 1:** January–June
- **Semester 2:** July–December

> [!NOTE]
> **Fiscal quarters** depend on the organization's financial year and may differ from calendar quarters. For a financial year beginning in April, `df["order_date"].dt.to_period("Q-MAR").dt.quarter` gives the fiscal quarter. ISO week-year can also differ from calendar year near New Year.

---

## 4. Calculate the Difference Between Dates

Date subtraction produces a **`timedelta`**, which represents elapsed time.

### A. Days Between Two Dates

```python
df["start_date"] = pd.to_datetime(df["start_date"])
df["end_date"] = pd.to_datetime(df["end_date"])

df["duration"] = df["end_date"] - df["start_date"]

# Whole completed days (rounded down)
df["days"] = df["duration"].dt.days

# Elapsed days including fractions
df["days_exact"] = df["duration"].dt.total_seconds() / 86400
```

**Formula — elapsed days:**

$$
\boxed{\text{Elapsed Days} = \frac{\text{End Time} - \text{Start Time}}{86{,}400\ \text{seconds per day}}}
$$

- **`86,400`** = `24 × 60 × 60` seconds in one day.
- `.dt.days` returns the **whole-day component**, while `.dt.total_seconds() / 86400` preserves fractional days.

**Example:** A duration of **1 day and 12 hours** equals `1` whole day or `1.5` elapsed days.

### B. Difference in Calendar Months

To find how many **calendar-month boundaries** separate two dates:

$$
\boxed{\text{Month Difference} = 12(Y_2-Y_1)+(M_2-M_1)}
$$

Where:

- $Y_1, M_1$ = starting year and month.
- $Y_2, M_2$ = ending year and month.

```python
df["months_between"] = (
    (df["end_date"].dt.year - df["start_date"].dt.year) * 12
    + (df["end_date"].dt.month - df["start_date"].dt.month)
)
```

**Example:** January 2026 → April 2026 gives **3 calendar months**.

> [!WARNING]
> This calculates **calendar-month difference**, not exact elapsed months. January 31 → February 1 returns `1`, even though only one day passed. Months have different lengths, so there is no single universally exact conversion from days to months.

---

## 5. Extract Features from Time

For a combined datetime column, use `.dt` to retrieve time components.

```python
df["hour"] = df["order_date"].dt.hour
df["minute"] = df["order_date"].dt.minute
df["second"] = df["order_date"].dt.second

# Time of day as a Python time object
df["time_only"] = df["order_date"].dt.time
```

| Timestamp | Hour | Minute | Second |
|---|---:|---:|---:|
| `2026-10-15 14:30:45` | 14 | 30 | 45 |

- **Hour:** `0–23`
- **Minute:** `0–59`
- **Second:** `0–59`
- `.dt.time` is useful for inspection, but its Python time objects are **not directly numeric ML features**.

**If a column contains only time strings:**

```python
df["login_time"] = pd.to_datetime(
    df["login_time"], format="%H:%M:%S", errors="coerce"
)

df["login_hour"] = df["login_time"].dt.hour
```

This gives the time a placeholder date internally; keep the original **full timestamps** when calculating durations across dates.

---

## 6. Calculate Time Differences

Suppose a message was **sent at 11:55 PM** and **read at 12:05 AM the next day**.

| Sent | Read | Elapsed Time |
|---|---|---|
| 23:55 (Day 1) | 00:05 (Day 2) | 10 minutes |

**Formula — elapsed seconds:**

$$
\boxed{\Delta t_{\text{seconds}} = (t_{\text{end}}-t_{\text{start}})_{\text{in seconds}}}
$$

**Conversions:**

$$
\text{Minutes} = \frac{\text{Seconds}}{60}
\qquad
\text{Hours} = \frac{\text{Seconds}}{3600}
$$

```python
df["sent_at"] = pd.to_datetime(df["sent_at"])
df["read_at"] = pd.to_datetime(df["read_at"])

elapsed = df["read_at"] - df["sent_at"]

df["seconds_to_read"] = elapsed.dt.total_seconds()
df["minutes_to_read"] = elapsed.dt.total_seconds() / 60
df["hours_to_read"] = elapsed.dt.total_seconds() / 3600
```

**Remember:** Use `.dt.total_seconds()` for complete durations; `.dt.seconds` alone excludes whole days and can give misleading results.

---

## 7. Practical Examples

### Example 1 — E-Commerce Orders Dataset

**Goal:** Extract when an order was placed and how long delivery took.

```python
import pandas as pd

orders = pd.DataFrame({
    "order_id": [101, 102, 103],
    "ordered_at": [
        "2026-02-01 10:30:00",
        "2026-02-02 21:05:00",
        "2026-02-03 08:00:00"
    ],
    "delivered_at": [
        "2026-02-03 12:00:00",
        "2026-02-04 09:00:00",
        None
    ]
})

for col in ["ordered_at", "delivered_at"]:
    orders[col] = pd.to_datetime(orders[col], errors="coerce")

orders["order_month"] = orders["ordered_at"].dt.month
orders["order_hour"] = orders["ordered_at"].dt.hour
orders["order_day"] = orders["ordered_at"].dt.day_name()
orders["is_weekend"] = orders["ordered_at"].dt.dayofweek >= 5

orders["delivery_hours"] = (
    orders["delivered_at"] - orders["ordered_at"]
).dt.total_seconds() / 3600

print(orders)
```

**Why these features are useful:**

- `order_hour` → Identify peak shopping hours.
- `order_day` / `is_weekend` → Study weekday versus weekend activity.
- `delivery_hours` → Analyze delivery speed **after an order has been delivered**.
- Missing `delivered_at` produces a missing delivery duration (`NaN`).

### Example 2 — Messaging Platform Dataset

**Goal:** Measure how quickly users read messages.

```python
messages = pd.DataFrame({
    "sent_at": [
        "2026-03-02 23:55:00",
        "2026-03-03 10:10:00"
    ],
    "read_at": [
        "2026-03-03 00:05:00",
        "2026-03-03 10:12:30"
    ]
})

messages["sent_at"] = pd.to_datetime(messages["sent_at"])
messages["read_at"] = pd.to_datetime(messages["read_at"])

messages["sent_hour"] = messages["sent_at"].dt.hour
messages["sent_weekday"] = messages["sent_at"].dt.day_name()

messages["read_delay_minutes"] = (
    messages["read_at"] - messages["sent_at"]
).dt.total_seconds() / 60

print(messages[["sent_hour", "sent_weekday", "read_delay_minutes"]])
```

**Expected read delays:** `10.0` minutes and `2.5` minutes.

These features can support **engagement analysis, user behavior studies, and notification timing**.

---

## 8. Common Mistakes and Best Practices

| Mistake | Better approach |
|---|---|
| Applying `.dt` to a string column | Convert using `pd.to_datetime()` first |
| Ignoring invalid date strings | Check `NaT` and handle missing values |
| Using `.dt.seconds` for multi-day durations | Use `.dt.total_seconds()` |
| Assuming all months have 30 days | Distinguish calendar months from elapsed days |
| Subtracting only time-of-day values | Use complete datetimes, especially across midnight |
| Mixing time zones | Make timestamps timezone-consistent first |
| Using a future event to predict a past event | Avoid **data leakage** |

> [!IMPORTANT]
> **Data leakage example:** If predicting an order's delivery time **at checkout**, you cannot use its actual `delivered_at` timestamp as an input feature. That value is unknown when the prediction is made.
>
> For chronological data, also consider a **time-based train/test split** rather than randomly mixing past and future observations.

---

## 9. Quick Reference — Pandas Date/Time Features

| What you need | Pandas expression |
|---|---|
| Convert to datetime | `pd.to_datetime(df["date"])` |
| Year | `df["date"].dt.year` |
| Month number | `df["date"].dt.month` |
| Month name | `df["date"].dt.month_name()` |
| Day of month | `df["date"].dt.day` |
| Weekday name | `df["date"].dt.day_name()` |
| Weekday number | `df["date"].dt.dayofweek` |
| Weekend flag | `df["date"].dt.dayofweek >= 5` |
| ISO week number | `df["date"].dt.isocalendar().week` |
| Calendar quarter | `df["date"].dt.quarter` |
| Hour | `df["date"].dt.hour` |
| Minute | `df["date"].dt.minute` |
| Second | `df["date"].dt.second` |
| Time only | `df["date"].dt.time` |
| Elapsed days | `(end - start).dt.total_seconds() / 86400` |
| Elapsed hours | `(end - start).dt.total_seconds() / 3600` |
| Elapsed minutes | `(end - start).dt.total_seconds() / 60` |

---

## 🧠 Quick Revision

- **`pd.to_datetime()`** converts date strings into Pandas datetime values.
- **`.dt`** extracts year, month, day, weekday, quarter, hour, minute, and second.
- **Weekend flag:** `dayofweek >= 5` (Saturday = 5, Sunday = 6).
- **ISO week:** `.dt.isocalendar().week`; **calendar quarter:** `.dt.quarter`.
- **Date subtraction** produces a `timedelta`.
- **`.dt.total_seconds()`** is the safest starting point for duration calculations.
- **Month difference** based on year/month fields counts calendar-month boundaries, **not exact elapsed months**.
- **ML rule:** Extract only features available when the prediction is made, and validate on appropriate data splits.

> **One-line takeaway:** A single timestamp can become several meaningful ML features — **when an event happened, which calendar period it belongs to, and how long an activity took**.
