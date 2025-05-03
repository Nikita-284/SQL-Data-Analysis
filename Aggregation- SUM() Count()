
# 📊 SQL Concept: COUNT() vs SUM() with Example

## ✅ Scenario: Confirmation Rate per User

We want to calculate the **confirmation rate** per user from two tables:

- `Signups`: user registrations
- `Confirmations`: user actions like 'confirmed' or 'timeout'

---

## 💥 Example Query

```sql
SELECT 
    a.user_id,
    SUM(b.action = 'confirmed') / COUNT(b.action) AS confirmation_rate
FROM Signups a
LEFT JOIN Confirmations b ON a.user_id = b.user_id
GROUP BY a.user_id;
```

---

## 🔍 How It Works

| Part                            | Meaning                                     |
|----------------------------------|-------------------------------------------|
| `GROUP BY a.user_id`           | Group rows by user ID                    |
| `b.action = 'confirmed'`       | Returns TRUE (1) or FALSE (0) per row    |
| `SUM(b.action = 'confirmed')` | Adds up the number of confirmed actions  |
| `COUNT(b.action)`             | Counts total actions (non-NULL only)    |
| Division `SUM / COUNT`         | Gives confirmation rate per user         |

---

## ⚠ Why Not `COUNT()` Instead of `SUM()`?

- `COUNT(b.action = 'confirmed')` → counts ALL rows, because `b.action = 'confirmed'` is never NULL (always TRUE or FALSE).
- `SUM(b.action = 'confirmed')` → adds 1 for confirmed, 0 for others, which gives the correct count.

To filter inside COUNT safely:
```sql
COUNT(CASE WHEN b.action = 'confirmed' THEN 1 END)
```

---

## ✅ Summary

- Use `SUM(condition)` when you want to count **how many times a condition is true**.
- Use `COUNT(column)` to count **non-NULL values**.
- Use `COUNT(*)` to count **all rows**.

