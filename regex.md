
# 📌 SQL + REGEXP Cheat Sheet

## ✅ What is REGEXP in SQL?

- **REGEXP** (or `RLIKE` in MySQL) is used to match a column value against a **regular expression pattern**.
- Common in MySQL, PostgreSQL (`~`), and Oracle.
  
Example:
```sql
SELECT *
FROM Users
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_\.\-]*@leetcode\.com$';
```

---

## ✅ Example explained

Pattern:
```
^[A-Za-z][A-Za-z0-9_\.\-]*@leetcode\.com$
```

Breakdown:
| Part                         | Meaning                                      |
|-------------------------------|---------------------------------------------|
| `^`                          | Start of string                             |
| `[A-Za-z]`                  | First character is a letter                 |
| `[A-Za-z0-9_\.\-]*`       | Zero or more letters, numbers, `_`, `.`, `-` |
| `@leetcode`                  | Literal `@leetcode`                         |
| `\.`                        | Literal dot `.`                             |
| `com`                        | Literal `com`                               |
| `$`                          | End of string                               |

---

## ✅ Valid vs Invalid examples

**Valid:**
- `a@leetcode.com`
- `abc123@leetcode.com`

**Invalid:**
- `1abc@leetcode.com` → starts with number
- `abc@leetcode.net` → wrong domain
- `abc@leetcode.comx` → extra after `.com`

---

## ✅ Quick REGEXP symbol cheat sheet

| Symbol  | Meaning                                  |
|---------|------------------------------------------|
| `^`    | Start of string                         |
| `$`    | End of string                           |
| `.`    | Any character (except newline)         |
| `*`    | Zero or more of previous element       |
| `+`    | One or more of previous element        |
| `?`    | Zero or one of previous element       |
| `[]`   | Character set                          |
| `[^]`  | Negated character set                |
| `()`   | Grouping                              |
| `|`    | OR                                     |

---

## ✅ Example queries

1. Match emails ending with `.com`:
```sql
SELECT * FROM Users WHERE email REGEXP '\.com$';
```

2. Match usernames starting with letter, allow numbers:
```sql
SELECT * FROM Users WHERE username REGEXP '^[A-Za-z][A-Za-z0-9]*$';
```

3. Match phone numbers with optional country code:
```sql
SELECT * FROM Users WHERE phone REGEXP '^(\+\d{1,3}\s)?\d{10}$';
```

---

💡 **Tip:** Always double-escape (`\.`) special regex characters inside SQL strings.
