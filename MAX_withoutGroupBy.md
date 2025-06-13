
# 🧠 SQL: Correctly Selecting Row(s) with Maximum Value

## ❓Problem

**Goal:** Show the `first_name`, `last_name`, and `height` of the patient(s) with the **greatest height** from a `patients` table.

---

## ⚠️ Incorrect Approach (Common Mistake)

```sql
SELECT first_name, last_name, MAX(height) AS height
FROM patients;
```

### ❌ Why This Doesn't Work:

- `MAX(height)` is an **aggregate function**, which returns just the **maximum height value**.
- But `first_name` and `last_name` are **not aggregated**, nor grouped.
- So SQL:
  - Returns **only one row**
  - May pick **random values** for `first_name` and `last_name` (non-deterministic)
- This is especially a problem in **MySQL without ONLY_FULL_GROUP_BY mode**.

> 🔴 You might **not get the name of the tallest person**, just their height.

---

## ✅ Correct Approach: Use a Subquery to Match Max Height

```sql
SELECT first_name, last_name, height
FROM patients
WHERE height = (
    SELECT MAX(height)
    FROM patients
);
```

### ✅ Why This Works:

- Subquery finds the **maximum height**.
- Outer query selects **rows matching that height**.
- Returns **correct name(s)** associated with the tallest patient(s).
- Also handles **ties** (e.g., multiple people with the same max height).

---

## 🧠 Summary

| Approach | Correct? | Why |
|----------|----------|-----|
| `SELECT first_name, last_name, MAX(height)` | ❌ | `first_name`/`last_name` may not match the max height |
| `WHERE height = (SELECT MAX(height))`       | ✅ | Accurately finds the row(s) with the max height |

---

## ✅ Optional Extension

To return **only one** patient in case of a tie (e.g., first alphabetically):

```sql
SELECT first_name, last_name, height
FROM patients
WHERE height = (
    SELECT MAX(height)
    FROM patients
)
ORDER BY first_name
LIMIT 1;
```

---

Happy querying! 🧬💻
