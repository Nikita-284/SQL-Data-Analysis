# SQL + Regex Notes

## 📌 SQL + REGEXP Overview

- In SQL, the `REGEXP` operator is used to **filter rows where a column matches a regular expression**.
- Example:
  ```sql
  SELECT *
  FROM Users
  WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_\\.\\-]*@leetcode\\.com$';
