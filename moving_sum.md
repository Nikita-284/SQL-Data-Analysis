
# 📊 SQL Window Function: 7-Day Moving Average Problem

## ❓ Problem Statement

You are the restaurant owner and want to analyze a possible expansion. Your goal is to **compute the 7-day moving average** of how much the customers paid.

- You are given a `Customer` table with the following schema:

```
Table: Customer

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| visited_on    | date    |
| amount        | int     |
+---------------+---------+
(customer_id, visited_on) is the primary key.
Each row indicates that a customer visited the restaurant and paid a certain amount.
```

- You need to compute the **total amount** collected each day.
- Then calculate the **7-day moving average**, where the window is the **current day and the 6 previous days**.
- Round the `average_amount` to **2 decimal places**.
- Return one row per day **only after the first 6 days**, and order by `visited_on` ascending.

### Example Output:

```
+--------------+--------+----------------+
| visited_on   | amount | average_amount |
+--------------+--------+----------------+
| 2019-01-07   | 860    | 122.86         |
| 2019-01-08   | 840    | 120.00         |
| 2019-01-09   | 840    | 120.00         |
| 2019-01-10   | 1000   | 142.86         |
+--------------+--------+----------------+
```

---

## ✅ Correct SQL Solution

```sql
SELECT visited_on, amount, average_amount 
FROM 
(SELECT DISTINCT visited_on, SUM(amount) OVER
 (ORDER BY visited_on RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW) AS amount,
  ROUND(SUM(amount) OVER (ORDER BY visited_on RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW)/7,2)
   AS average_amount
FROM Customer) as whole_totals
WHERE DATEDIFF(visited_on, (SELECT MIN(visited_on) FROM Customer)) >= 6
```

---

## 💡 Key Concepts in This Window Function

### ✅ `ROWS BETWEEN` vs `RANGE BETWEEN`

| Feature               | `ROWS BETWEEN`                           | `RANGE BETWEEN` |
|----------------------|-------------------------------------------|-----------------|
| Operates on          | Physical rows                             | Logical value range |
| Considers duplicates | Yes                                       | Yes              |
| Works with dates?    | ❌ Only if date is evenly spaced          | ✅ Ideal for date intervals |
| Requires unique order values? | ❌                          | ✅ Usually required |

### Example:
If `visited_on = '2019-01-10'`, then:
- `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW` → Looks back at 6 previous rows, not dates.
- `RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW` → Looks back at rows with dates between `'2019-01-04'` and `'2019-01-10'`.

> 📌 In our solution, we used `ROWS` because we pre-aggregated the data to ensure **one row per date**, making the moving average over dates consistent.

---

## 📘 Summary

- Pre-aggregate the `amount` by `visited_on` first.
- Then apply a **window function** on the aggregated daily totals.
- Use `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW` when you know the dataset has **one row per date**.
- Use `RANGE BETWEEN` when your dates might be **missing** or **duplicated**, and you need logic based on date values.

