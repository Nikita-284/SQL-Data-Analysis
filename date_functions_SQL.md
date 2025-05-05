
# 📅 SQL Date Functions Cheat Sheet

This guide covers the most useful SQL date functions, examples, and when to use them in typical SQL problems.

---

## ✅ Basic Date Functions

| Function         | Description                  | Example                          |
|------------------|-----------------------------|----------------------------------|
| `CURRENT_DATE()` | Returns today’s date        | `'2024-05-05'`                  |
| `NOW()`         | Returns current date + time | `'2024-05-05 12:34:56'`         |

---

## 🔧 Date Arithmetic

| Function                         | Description                            | Example                                      |
|----------------------------------|----------------------------------------|--------------------------------------------|
| `DATE_ADD(date, INTERVAL N unit)` | Add N units to date                   | `DATE_ADD('2024-01-01', INTERVAL 5 DAY)` → `'2024-01-06'` |
| `DATE_SUB(date, INTERVAL N unit)` | Subtract N units from date            | `DATE_SUB('2024-01-01', INTERVAL 1 DAY)` → `'2023-12-31'` |
| `DATEDIFF(date1, date2)`         | Days between two dates                | `DATEDIFF('2024-01-03', '2024-01-01')` → `2` |

---

## 📆 Extract Date Parts

| Function            | Description                         | Example                                  |
|---------------------|------------------------------------|----------------------------------------|
| `YEAR(date)`      | Extract year                      | `YEAR('2024-05-05')` → `2024`        |
| `MONTH(date)`     | Extract month number (1–12)      | `MONTH('2024-05-05')` → `5`          |
| `DAY(date)` or `DAYOFMONTH(date)` | Extract day of month  | `DAY('2024-05-05')` → `5`           |
| `DAYOFWEEK(date)` | Weekday index (1 = Sunday)       | `DAYOFWEEK('2024-05-05')` → `1`      |
| `WEEK(date)`      | Week number of the year          | `WEEK('2024-05-05')` → `18`          |
| `QUARTER(date)`   | Quarter (1–4)                   | `QUARTER('2024-05-05')` → `2`        |

---

## 🎨 Format and Parse Dates

| Function                | Description                         | Example                                      |
|-------------------------|-------------------------------------|--------------------------------------------|
| `DATE_FORMAT(date, format_string)` | Format date as string         | `DATE_FORMAT('2024-05-05', '%Y-%m')` → `'2024-05'` |
| `STR_TO_DATE(string, format)`      | Convert string to date       | `STR_TO_DATE('2024-05-05', '%Y-%m-%d')` → date |

---

## ✂️ Truncate and Round Dates

| Function         | Description                          | Example                                    |
|------------------|-------------------------------------|------------------------------------------|
| `DATE_TRUNC('unit', date)` (Postgres) | Truncate to unit (year, month, etc.) | `DATE_TRUNC('month', '2024-05-05')` → `'2024-05-01'` |
| `LAST_DAY(date)` (MySQL, Oracle) | Last day of month            | `LAST_DAY('2024-05-05')` → `'2024-05-31'` |

---

## ⚡ Use Cases for SQL Problems

| Problem Type                               | Recommended Functions                   |
|-------------------------------------------|----------------------------------------|
| Check if next-day event exists           | `DATE_ADD()`, `DATEDIFF()`            |
| Calculate days between dates             | `DATEDIFF()`                         |
| Group or filter by month/year           | `YEAR()`, `MONTH()`, `DATE_FORMAT()` |
| Find earliest/latest event per group    | `MIN(date)`, `MAX(date)`             |
| Truncate to month or year               | `DATE_FORMAT()`, `DATE_TRUNC()`      |
| Handle time ranges (7-day, month window) | `DATE_ADD()`, `DATE_SUB()`           |

---

## 💥 Example: Find users who logged in the month after signup

```sql
WHERE login_date >= DATE_ADD(signup_date, INTERVAL 1 MONTH)
```

## 💥 Example: Group revenue by year-month

```sql
GROUP BY DATE_FORMAT(order_date, '%Y-%m')
```

---

💡 **Tip:** Mastering date functions makes you unbeatable in SQL data cleaning, reporting, and time-based analysis!

