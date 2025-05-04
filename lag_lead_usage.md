
# 📊 SQL Cheat Sheet: LAG() and LEAD()

## ✅ What are LAG() and LEAD()?

- **LAG()** → Looks backward in the result set.
- **LEAD()** → Looks forward in the result set.

They are **window functions** that help compare current row values with previous or next rows.

---

## ✅ Syntax

```sql
LAG(column, offset, default_value) OVER (PARTITION BY col ORDER BY col)
LEAD(column, offset, default_value) OVER (PARTITION BY col ORDER BY col)
```

| Part             | Meaning                                      |
|------------------|---------------------------------------------|
| `column`        | Column to look back/forward on             |
| `offset`        | How many rows back/forward (default = 1)  |
| `default_value` | Value if no row exists (default = NULL)   |
| `OVER` clause   | Defines partition and order of the window |

---

## 💥 Example

```sql
SELECT 
    id,
    num,
    LAG(num, 1) OVER (ORDER BY id) AS prev1,
    LAG(num, 2) OVER (ORDER BY id) AS prev2,
    LEAD(num, 1) OVER (ORDER BY id) AS next1
FROM Logs;
```

| id | num | prev1 | prev2 | next1 |
|----|-----|-------|-------|-------|
| 1  | 1   | NULL  | NULL  | 1     |
| 2  | 1   | 1     | NULL  | 1     |
| 3  | 1   | 1     | 1     | 2     |
| 4  | 2   | 1     | 1     | 1     |
| 5  | 1   | 2     | 1     | 2     |
| 6  | 2   | 1     | 2     | 2     |
| 7  | 2   | 2     | 1     | NULL  |

---

## ✅ Example with PARTITION BY

```sql
LAG(num) OVER (PARTITION BY user_id ORDER BY id)
```

→ Resets the window for each `user_id` group.

---

## ✅ Common Use Cases

- Detect consecutive values (`num = prev1 = prev2`)
- Compare with previous or next row
- Calculate differences or changes between rows
- Find trends or transitions in ordered data

---

💡 **Tip:** Always use `ORDER BY` in the `OVER()` clause to control row order!
