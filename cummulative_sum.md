# 🚌 SQL Window Function: Cumulative Sum with Cut-off Logic

## 📘 Interview Concept: "Last Valid Entry Within a Cumulative Limit"

### ❓ Problem
You are given a `Queue` table:

| Column Name | Type    |
|-------------|---------|
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |

- `turn` defines the boarding order.
- The bus can carry people **in turn order**, **one at a time**, up to a **maximum total weight of 1000 kg**.
- Find the **`person_name` of the last person** who can get on **without exceeding the limit**.

---

### ✅ SQL Strategy

1. **Calculate cumulative weight** using a window function:
   ```sql
   SUM(weight) OVER (ORDER BY turn)
   ```

2. **Filter rows where cumulative sum ≤ 1000**.

3. **Get the person with the maximum turn** from the filtered list — this is the last person who can board.

---

### 🧠 Final SQL Query

```sql
WITH cte AS (
    SELECT 
        person_id,
        person_name,
        weight,
        turn,
        SUM(weight) OVER (ORDER BY turn) AS cumulative_weight
    FROM Queue
),
valid_boarders AS (
    SELECT *
    FROM cte
    WHERE cumulative_weight <= 1000
)
SELECT person_name
FROM valid_boarders
WHERE turn = (
    SELECT MAX(turn) FROM valid_boarders
);
```

---

### 💡 Why This Is Important in Interviews

- Demonstrates use of **window functions** like `SUM() OVER (...)`.
- Combines **CTEs**, **filtering**, and **aggregation**.
- Shows understanding of how to **limit records based on dynamic calculations**.

---

### 📌 Keywords to Remember
- `SUM() OVER (ORDER BY ...)`: Running total
- `CTE`: Helps manage layered logic
- `MAX(turn)`: Identifies the final valid record
- Use **subquery or CTE** when filtering on window function results

---

### 🧪 Pro Tip
You can extend this approach to:
- Budget-constrained transactions
- Capacity planning
- Rate limit evaluations
