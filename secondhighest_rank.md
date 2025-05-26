
# 🧠 SQL: Find ID with Second Highest Salary — Understanding `RANK()` vs `DENSE_RANK()`

## ❓ Problem Statement

You are given an `Employee` table:

```sql
Employee
+-------------+--------+
| id          | salary |
+-------------+--------+
```

Write a SQL query to return the `id` of the employee who has the **second highest distinct salary**.  
If multiple employees are tied for the second-highest salary, return **any one** of their `id`s in a **deterministic way** (e.g., lowest ID).  
If no second-highest salary exists, return `NULL`.

---

## ✅ Sample Input

```text
| id | salary |
|----|--------|
| 1  | 300    |
| 2  | 200    |
| 3  | 200    |
| 4  | 100    |
```

### Expected Output:
```text
| SecondHighestId |
|-----------------|
| 2               |
```

---

## 🧠 Concept: `RANK()` vs `DENSE_RANK()`

### 🔹 `RANK()`

- Assigns the same rank to tied values.
- **Skips ranks** for tied rows.

```sql
RANK() OVER (ORDER BY salary DESC)
```

| salary | RANK |
|--------|------|
| 300    | 1    |
| 200    | 2    |
| 200    | 2    |
| 100    | 4 ❗  |

> Notice: Rank 3 is skipped.

---

### 🔹 `DENSE_RANK()`

- Also assigns same rank to ties.
- **Does not skip** the next rank.

```sql
DENSE_RANK() OVER (ORDER BY salary DESC)
```

| salary | DENSE_RANK |
|--------|------------|
| 300    | 1          |
| 200    | 2 ✅        |
| 200    | 2 ✅        |
| 100    | 3          |

> Perfect for getting **second highest distinct salary**.

---

## ✅ Final SQL Solution (Return one ID deterministically)

```sql
WITH ranked_emp AS (
    SELECT id, salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rk
    FROM Employee
)
SELECT (
    SELECT id
    FROM ranked_emp
    WHERE rk = 2
    ORDER BY id  -- ensures consistent result
    LIMIT 1
) AS SecondHighestId;
```

- Uses `DENSE_RANK()` to find the second-highest **distinct** salary.
- If multiple rows are tied, returns the **lowest id**.

---

## ✅ Alternate: Return all tied IDs (if allowed)

```sql
WITH ranked_emp AS (
    SELECT id, salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rk
    FROM Employee
)
SELECT id
FROM ranked_emp
WHERE rk = 2;
```

---

## ✅ Summary

| Need                               | Approach                            |
|------------------------------------|--------------------------------------|
| Second highest distinct salary     | Use `DENSE_RANK()`                   |
| One `id` returned deterministically | Add `ORDER BY id LIMIT 1`           |
| Avoid skipping ranks               | Prefer `DENSE_RANK()` over `RANK()` |

Let me know if you'd like to extend this to return name, join date, or to return all second-highest earners.
