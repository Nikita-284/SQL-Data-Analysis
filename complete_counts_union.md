# 📊 Ensuring Complete Output in SQL Aggregations with LEFT JOIN and COALESCE

## 🎯 Problem Context: Salary Categories

You are given a table of bank accounts with `income`, and you need to group accounts into salary categories:

- **Low Salary**: income < 20000  
- **Average Salary**: income between 20000 and 50000 (inclusive)  
- **High Salary**: income > 50000  

The twist: **your output must include all three categories**, even if **some have 0 accounts**.

---

## ❗ The Challenge

A regular `GROUP BY` query will **exclude categories** that have no data.

### 🔍 Example:

If the `Accounts` table only has:
```text
account_id | income
-----------|--------
1          | 18000
2          | 30000
```

A `GROUP BY` query on categorized income will return:
```text
category        | accounts_count
----------------|----------------
Low Salary      | 1
Average Salary  | 1
```

But **"High Salary" is missing** ❌

---

## ✅ The Strategy

### 1. **Create all possible categories manually**
```sql
all_categories AS (
    SELECT 'Low Salary' AS category
    UNION ALL
    SELECT 'Average Salary'
    UNION ALL
    SELECT 'High Salary'
)
```

This guarantees a row for each category exists, like a scaffold.

---

### 2. **Build your main category count using CASE and GROUP BY**
```sql
WITH categorized_accounts AS (
    SELECT 
        CASE 
            WHEN income < 20000 THEN 'Low Salary'
            WHEN income BETWEEN 20000 AND 50000 THEN 'Average Salary'
            ELSE 'High Salary'
        END AS category
    FROM Accounts
),
category_counts AS (
    SELECT 
        category, 
        COUNT(*) AS accounts_count
    FROM categorized_accounts
    GROUP BY category
)
```

---

### 3. **LEFT JOIN with COALESCE**
```sql
SELECT 
    a.category, 
    COALESCE(c.accounts_count, 0) AS accounts_count
FROM all_categories a
LEFT JOIN category_counts c
ON a.category = c.category;
```

- `LEFT JOIN`: Preserves all rows from `all_categories`.
- `COALESCE`: Converts `NULL` to `0` for missing categories.

---

## 🧾 Final Result
| category       | accounts_count |
|----------------|----------------|
| Low Salary     | 1              |
| Average Salary | 1              |
| High Salary    | 0              | ✅

---

## 💡 Why This is Important in Interviews

- Shows knowledge of **set completeness** in SQL reporting.
- Demonstrates **handling NULLs** with `COALESCE`.
- Combines logic across **window functions, joins, and aggregation**.

---

## 📌 Summary Keywords
- `CASE`: For classification
- `LEFT JOIN`: To preserve all base categories
- `COALESCE`: To convert missing values to 0
- `UNION ALL`: To manually construct complete dimension list

