
# Understanding `EXISTS` in SQL – With Seat Swapping Example

## 🔍 What is `EXISTS`?

The `EXISTS` keyword in SQL is used to test for the **existence of any record** in a subquery.  
It returns:
- `TRUE` if the subquery **returns at least one row**
- `FALSE` if the subquery **returns no rows**

It is commonly used in `WHERE` or `CASE` statements to apply logic based on whether related data exists.

---

## ✅ Syntax

```sql
EXISTS (SELECT 1 FROM table_name WHERE condition)
```

- `SELECT 1`: A lightweight placeholder. We only care about **existence**, not the actual data.
- `condition`: Filters the inner table (or alias) to match against the outer row.

---

## 📘 Use Case: Seat Swapping Problem

### Table: `Seat`

| id | student |
|----|---------|
| 1  | Alice   |
| 2  | Bob     |
| 3  | Charlie |
| 4  | Dave    |
| 5  | Eva     |

We want to swap every **two consecutive students**, keeping the last one as-is if there's an odd number of rows.

---

### 🧠 Logic

| Condition                                    | New ID      |
|---------------------------------------------|-------------|
| Odd ID and next ID exists                   | `id + 1`    |
| Even ID                                      | `id - 1`    |
| Odd ID and next ID does **not** exist       | `id`        |

---

### 💡 SQL Query Using `EXISTS`

```sql
SELECT
    CASE 
        WHEN id % 2 = 1 AND EXISTS (
            SELECT 1 FROM Seat s2 WHERE s2.id = Seat.id + 1
        ) THEN id + 1
        WHEN id % 2 = 0 THEN id - 1
        ELSE id
    END AS id,
    student
FROM Seat
ORDER BY id;
```

---

### 🧪 Example Output

| id | student |
|----|---------|
| 1  | Bob     |
| 2  | Alice   |
| 3  | Dave    |
| 4  | Charlie |
| 5  | Eva     |

---

## 🧠 Key Takeaways

- `EXISTS` is used to **check if a row exists**, without fetching it.
- Use `SELECT 1` with `EXISTS` to keep queries lightweight.
- Very useful in scenarios where **conditional logic** depends on **related data availability**.
