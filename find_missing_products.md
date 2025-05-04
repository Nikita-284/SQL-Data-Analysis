
# 📊 SQL Concept: Finding Customers Who Did Not Buy All Products

## ✅ Problem

We want to identify **which customers have not purchased all available products**.

### Example Tables

**Customer table**

| customer_id | product_key |
|-------------|-------------|
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |

**Product table**

| product_key |
|-------------|
| 5          |
| 6          |

---

## 💥 Solution Query

```sql
SELECT c.customer_id
FROM Customer c
GROUP BY c.customer_id
HAVING COUNT(DISTINCT c.product_key) < (SELECT COUNT(*) FROM Product);
```

---

## 🔍 Explanation

| Part                                       | Meaning                                                       |
|--------------------------------------------|-------------------------------------------------------------|
| `GROUP BY c.customer_id`                  | Group rows by customer ID                                   |
| `COUNT(DISTINCT c.product_key)`           | Count how many unique products each customer has bought    |
| `(SELECT COUNT(*) FROM Product)`          | Get total number of products available                    |
| `HAVING ... < total`                      | Keep only customers who bought fewer than all products     |

---

## ✅ Example Breakdown

| customer_id | bought products  | product count |
|-------------|------------------|---------------|
| 1          | 5, 6             | 2            |
| 2          | 6               | 1            |
| 3          | 5, 6           | 2            |

- Total products = 2  
- Only **customer 2** didn’t buy all products → result = `2`

---

## ✅ Key Takeaway

- Use `GROUP BY` to analyze per customer.
- Use `COUNT(DISTINCT)` to count unique products per customer.
- Use a subquery to get the total products.
- Use `HAVING` to filter groups.

💡 **Tip:** This pattern works for many “who’s missing what” questions across users, products, courses, events, etc.

