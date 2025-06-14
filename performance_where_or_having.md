
# SQL: WHERE vs HAVING in GROUP BY Queries

## 📄 Problem
You want to show the number of patients for each allergy (excluding null values), ordered by the most common ones.

### Two Query Options:

#### ✅ Option 1: Using `WHERE`
```sql
SELECT allergies, COUNT(*) AS total_diagnosis
FROM patients
WHERE allergies IS NOT NULL
GROUP BY allergies
ORDER BY total_diagnosis DESC;
```

#### ✅ Option 2: Using `HAVING`
```sql
SELECT allergies, COUNT(*) AS total_diagnosis
FROM patients
GROUP BY allergies
HAVING allergies IS NOT NULL
ORDER BY total_diagnosis DESC;
```

---

## 🔧 Execution Order Recap
1. `FROM`
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `ORDER BY`

---

## 🤔 Key Differences
| Feature                | `WHERE`                         | `HAVING`                                |
|------------------------|----------------------------------|------------------------------------------|
| Runs **before** grouping | ✅ Yes                        | ❌ No (runs after grouping)             |
| Filters **row-level** data | ✅ Yes                      | ❌ No (used for aggregated results)    |
| Performance            | ✅ Better                    | ❌ Worse, groups unnecessary rows     |
| Best used for          | Simple filters on raw data       | Filters on aggregated values             |

---

## ⚡️ Performance Insight
- `WHERE` is more **efficient** because it **filters early**, reducing the number of rows that `GROUP BY` needs to process.
- `HAVING` applies the filter **after aggregation**, so it works harder for the same result.

---

## 🔹 When to Use What:
| Situation                          | Recommended Clause |
|-----------------------------------|---------------------|
| Filtering raw values               | `WHERE`             |
| Filtering based on aggregates      | `HAVING`            |

---

## 🖊️ Final Tips:
> "Always use `WHERE` to filter non-aggregated data before `GROUP BY`. Use `HAVING` only when filtering on aggregated results. `WHERE` is faster and more efficient."
