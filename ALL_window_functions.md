
# 📊 SQL Window Functions Cheat Sheet — With Use Cases

This guide explains the most important SQL window functions, when to use them, and example problems they help solve.

---

## 📋 Sample Table for Reference

| player_id | event_date  | score |
|-----------|-------------|-------|
| 1         | 2024-01-01 | 50    |
| 1         | 2024-01-02 | 70    |
| 2         | 2024-01-01 | 60    |
| 2         | 2024-01-03 | 80    |
| 3         | 2024-01-02 | 90    |

---

## 🥇 Ranking Functions

### 1. ROW_NUMBER()
Assigns a unique number to each row within a partition.
- **Use case:** Get first or last row per group.

```sql
ROW_NUMBER() OVER (PARTITION BY player_id ORDER BY event_date)
```

### 2. RANK()
Gives rank with gaps for ties.
- **Use case:** Rank players by score; ties get same rank, next rank skips.

### 3. DENSE_RANK()
Like RANK() but no gaps.
- **Use case:** Rank players by score; ties get same rank, next rank continues without skipping.

### 4. NTILE(n)
Splits rows into n buckets.
- **Use case:** Assign quartiles or deciles.

---

## 📈 Aggregate Window Functions

### 5. SUM()
Calculates running or partitioned sum.
- **Use case:** Get cumulative score per player.

### 6. AVG()
Calculates average over window.
- **Use case:** Get average score per player.

### 7. COUNT()
Counts rows over window.
- **Use case:** Count games played per player.

### 8. MIN()
Finds minimum over window.
- **Use case:** Get first login date per player.

### 9. MAX()
Finds maximum over window.
- **Use case:** Get highest score per player.

---

## 🔁 Navigation Functions

### 10. LAG()
Fetches value from previous row.
- **Use case:** Compare current and prior score.

### 11. LEAD()
Fetches value from next row.
- **Use case:** See next planned event.

### 12. FIRST_VALUE()
Gets first value in partition.
- **Use case:** Attach player’s first login date to all rows.

### 13. LAST_VALUE()
Gets last value in partition.
- **Use case:** Show latest group value on all rows.

---

## ⚡ Summary: Which Function for Which Problem

| Problem Type                      | Function to Use  |
|-----------------------------------|------------------|
| Get first/last row                | ROW_NUMBER() + filter |
| Calculate ranks                   | RANK(), DENSE_RANK(), NTILE() |
| Cumulative or group totals        | SUM(), AVG(), COUNT() |
| Find group min/max                | MIN(), MAX() |
| Compare across rows               | LAG(), LEAD() |
| Get first or last value directly  | FIRST_VALUE(), LAST_VALUE() |

💡 **Tip:** Combine window functions with PARTITION BY and ORDER BY smartly to solve almost any analytics problem in SQL!

