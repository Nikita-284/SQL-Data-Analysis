The `ROWS BETWEEN` clause in SQL window functions defines the window frame for the calculations. The window frame determines which rows are included in the calculation for the current row. There are several ways to specify the frame depending on your use case.

Here’s the general syntax for the `ROWS BETWEEN` clause:

```sql
ROWS BETWEEN <start> AND <end>
```

### Possible Values for `<start>` and `<end>`:

1. **CURRENT ROW**: Refers to the current row.
2. **n PRECEDING**: Refers to `n` rows before the current row.
3. **n FOLLOWING**: Refers to `n` rows after the current row.
4. **UNBOUNDED PRECEDING**: Refers to all rows before the current row (starting from the very first row in the partition).
5. **UNBOUNDED FOLLOWING**: Refers to all rows after the current row (including all rows to the end of the partition).
6. **<start> PRECEDING AND <end> FOLLOWING**: Allows you to specify a range from `n` rows before the current row to `m` rows after the current row.

### Common Use Cases for `ROWS BETWEEN`:

#### 1. **Default Window: Entire Partition (No `ROWS BETWEEN`)**

If you don’t specify `ROWS BETWEEN`, SQL will calculate the aggregate over the entire partition.

```sql
AVG(amount) OVER (PARTITION BY customer_id)
```

#### 2. **Current Row and Preceding Rows (`ROWS BETWEEN n PRECEDING AND CURRENT ROW`)**

Useful for cumulative sums or moving averages, where you include the current row and a set number of previous rows.

```sql
SUM(amount) OVER (ORDER BY visited_on ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
```

* This includes the current row and the 6 rows preceding it (7-day window).

#### 3. **Current Row and Following Rows (`ROWS BETWEEN CURRENT ROW AND n FOLLOWING`)**

If you want to include the current row and a set number of following rows in the calculation.

```sql
SUM(amount) OVER (ORDER BY visited_on ROWS BETWEEN CURRENT ROW AND 3 FOLLOWING)
```

* This includes the current row and the next 3 rows after it.

#### 4. **Unbounded Preceding (`ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`)**

Includes all rows from the beginning of the partition up to and including the current row.

```sql
SUM(amount) OVER (PARTITION BY customer_id ORDER BY visited_on ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
```

* This computes a cumulative sum for each customer starting from the very first row in the partition up to the current row.

#### 5. **Unbounded Following (`ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING`)**

Includes the current row and all rows after it, up to the last row in the partition.

```sql
SUM(amount) OVER (PARTITION BY customer_id ORDER BY visited_on ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
```

* This could be useful when calculating aggregates that consider the remaining data from the current row to the last row.

#### 6. **Fixed Window Size (`ROWS BETWEEN n PRECEDING AND m FOLLOWING`)**

You can specify a range that includes a specific number of rows before and after the current row.

```sql
AVG(amount) OVER (ORDER BY visited_on ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING)
```

* This includes the 2 rows before the current row and the 2 rows after the current row.

#### 7. **Window with No Rows before or after (`ROWS BETWEEN 0 PRECEDING AND 0 FOLLOWING`)**

This would give you the value for the current row alone. It's essentially equivalent to a `ROW` function.

```sql
SUM(amount) OVER (ORDER BY visited_on ROWS BETWEEN 0 PRECEDING AND 0 FOLLOWING)
```

* This will just return the `amount` for the current row.

### Summary of Possible `ROWS BETWEEN` Values:

* `ROWS BETWEEN CURRENT ROW AND CURRENT ROW`: The window contains only the current row.
* `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`: The window starts from the first row in the partition and ends at the current row.
* `ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING`: The window starts at the current row and includes all rows after it.
* `ROWS BETWEEN n PRECEDING AND CURRENT ROW`: The window includes the current row and `n` rows before it.
* `ROWS BETWEEN CURRENT ROW AND n FOLLOWING`: The window includes the current row and `n` rows after it.
* `ROWS BETWEEN n PRECEDING AND m FOLLOWING`: The window includes `n` rows before and `m` rows after the current row.
* `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`: The window spans the entire partition, including all rows.

### Examples:

1. **Cumulative Sum for Current and Previous 6 Days**:

   ```sql
   SUM(amount) OVER (ORDER BY visited_on ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
   ```

2. **Moving Average for Previous 7 Days**:

   ```sql
   AVG(amount) OVER (ORDER BY visited_on ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
   ```

3. **Rolling Window with 3 Rows Before and 2 Rows After**:

   ```sql
   SUM(amount) OVER (ORDER BY visited_on ROWS BETWEEN 3 PRECEDING AND 2 FOLLOWING)
   ```

### Conclusion:

The `ROWS BETWEEN` clause offers flexibility to define the range of rows to include in the window function calculation, and you can adapt this based on your specific use case (e.g., moving averages, cumulative sums, etc.).
