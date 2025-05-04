
# 📊 SQL Concept: Grouping by Month and Country with Conditional Aggregation

## ✅ Problem Statement

Write an SQL query to find **for each month and country**:
- the number of transactions
- their total amount
- the number of approved transactions
- the total amount of approved transactions

From this table:
```
Transactions(id, country, state, amount, trans_date)
```

---

## 💥 Final Query

```sql
SELECT 
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(*) AS trans_count,
    SUM(state = 'approved') AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM Transactions
GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country;
```

---

## 🔍 Key Concepts

| Concept                                  | Explanation                                                     |
|------------------------------------------|-----------------------------------------------------------------|
| `DATE_FORMAT(trans_date, '%Y-%m')`      | Extracts year-month like `2018-12` from date                   |
| `COUNT(*)`                              | Counts total number of transactions                           |
| `SUM(state = 'approved')`               | Counts approved transactions (because TRUE = 1, FALSE = 0)    |
| `SUM(amount)`                           | Sums all transaction amounts                                  |
| `SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END)` | Sums amounts only for approved transactions                  |
| `GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country` | Groups results by month and country                          |

---

## ✅ Example Output

| month    | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
|----------|---------|-------------|---------------|---------------------|-----------------------|
| 2018-12 | US      | 2           | 1             | 3000               | 1000                 |
| 2019-01 | US      | 1           | 1             | 2000               | 2000                 |
| 2019-01 | DE      | 1           | 1             | 2000               | 2000                 |

---

💡 **Tip:** Conditional sums (`SUM(condition)`) and `CASE WHEN` patterns are powerful for multi-metric reports!
