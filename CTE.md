
# 📊 SQL Cheat Sheet: Common Table Expressions (CTE)

## ✅ What is a CTE?

A **CTE (Common Table Expression)** is a temporary result set that you can reference within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` query.

It improves:
- Query readability
- Modularization of complex logic
- Reusability within a single query

---

## ✅ Syntax

```sql
WITH cte_name AS (
    SELECT column1, column2
    FROM some_table
    WHERE condition
)
SELECT *
FROM cte_name
WHERE another_condition;
```

- `WITH` → introduces the CTE
- `cte_name` → your temporary table name
- Inside `()` → the subquery that generates the CTE

---

## 💥 Example 1: Simple CTE

```sql
WITH approved_transactions AS (
    SELECT *
    FROM Transactions
    WHERE state = 'approved'
)
SELECT country, SUM(amount) AS total_approved
FROM approved_transactions
GROUP BY country;
```

---

## 💥 Example 2: Multiple CTEs

```sql
WITH cte1 AS (
    SELECT * FROM table1
),
cte2 AS (
    SELECT * FROM table2
)
SELECT *
FROM cte1
JOIN cte2 ON cte1.id = cte2.id;
```

---

## 💥 Example 3: Recursive CTE

```sql
WITH RECURSIVE cte_name AS (
    SELECT id, parent_id, name FROM categories WHERE parent_id IS NULL
    UNION ALL
    SELECT c.id, c.parent_id, c.name
    FROM categories c
    INNER JOIN cte_name p ON c.parent_id = p.id
)
SELECT * FROM cte_name;
```

→ Recursive CTEs are useful for working with hierarchical or tree-structured data.

---

## ✅ Advantages

- Improves query readability
- Avoids repeating subqueries
- Enables recursive queries (with `WITH RECURSIVE`)
- Works well with joins, window functions, and aggregates

---

💡 **Tip:** Think of CTEs as named temporary views for a single query!

