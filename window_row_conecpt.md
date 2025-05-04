
# 📊 SQL Concept: Combining Window Functions and CASE Without GROUP BY

## ✅ Problem Example

We are working with the following table:

```
Delivery
---------------------------------------------
| delivery_id | customer_id | order_date  | customer_pref_delivery_date |
|-------------|-------------|-------------|-----------------------------|
| int        | int         | date       | date                        |
---------------------------------------------
```

### Task:
- Find the percentage of **immediate orders** in the first orders of all customers.
- An order is **immediate** if `order_date = customer_pref_delivery_date`.

---

## 💥 Solution Query

```sql
WITH sub AS (
    SELECT
        delivery_id,
        customer_id,
        CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0 END AS immed,
        ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date ASC) AS rownum
    FROM Delivery
)
SELECT ROUND(SUM(immed) / COUNT(*) * 100, 2) AS immediate_percentage
FROM sub
WHERE rownum = 1;
```

---

## 🔍 Explanation of Query

| Part                                    | Explanation                                                       |
|-----------------------------------------|------------------------------------------------------------------|
| `CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0` | Tags each row as immediate (`1`) or scheduled (`0`), **row by row** |
| `ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date ASC)` | Assigns a row number to each order, starting from the earliest per customer |
| `WHERE rownum = 1`                      | Filters only the **first order** for each customer                |
| `SUM(immed)`                            | Sums the `1`s → counts how many first orders were immediate       |
| `COUNT(*)`                              | Counts how many first orders in total                            |
| `ROUND(SUM(immed) / COUNT(*) * 100, 2)` | Calculates the percentage, rounded to 2 decimal places           |

---

## ⚠ Why No GROUP BY Needed

- The `CASE` works **row by row**, tagging each record.
- The `ROW_NUMBER()` partitions per customer and marks the first row.
- The outer query **aggregates only the first row per customer**, so no `GROUP BY` is required.

Example flow:

| customer_id | delivery_id | order_date | immed | rownum |
|-------------|-------------|------------|-------|--------|
| 1          | 10         | 2021-01-01 | 1     | 1     |
| 1          | 11         | 2021-01-02 | 0     | 2     |
| 2          | 20         | 2021-01-03 | 1     | 1     |
| 2          | 21         | 2021-01-04 | 1     | 2     |

After `WHERE rownum = 1`:

| customer_id | delivery_id | order_date | immed | rownum |
|-------------|-------------|------------|-------|--------|
| 1          | 10         | 2021-01-01 | 1     | 1     |
| 2          | 20         | 2021-01-03 | 1     | 1     |

Then we aggregate → `SUM(immed) / COUNT(*)`.

---

## ✅ Key Takeaway

- Use `CASE` to tag rows.
- Use `ROW_NUMBER()` to pick top rows per group.
- No need for `GROUP BY` if you aggregate **after filtering** to one row per group.

💡 **Tip:** This pattern is powerful when combined with window functions and lets you avoid complex grouping.
