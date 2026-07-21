# 🧪 SQL Practice Questions & Challenges

Organized into structured practical categories to test and solidify SQL interview readiness.

---

## 📌 Categories Overview

1. [Concept Questions](#1-concept-questions)
2. [Coding Questions](#2-coding-questions)
3. [Debugging Questions](#3-debugging-questions)
4. [Output Prediction Questions](#4-output-prediction-questions)
5. [Scenario-Based Questions](#5-scenario-based-questions)
6. [Tricky Questions](#6-tricky-questions)
7. [Practical Problems](#7-practical-problems)
8. [Interview Challenges](#8-interview-challenges)

---

## 💡 1. Concept Questions

### Q1.1: Difference between Clustered and Non-Clustered Index
- **Difficulty**: Medium
- **Hint**: Think about physical data page ordering on disk vs secondary index lookup pointers.
- **Expected Approach**: Explain that a clustered index determines the physical order of data inside table data pages (hence only 1 allowed per table). Non-clustered index is a separate structure containing index key values and row locators (pointers to the clustered index key or RID heap).
- **Common Mistakes**: Claiming you can create multiple clustered indexes on a single table.

### Q1.2: Phantom Read vs Non-Repeatable Read
- **Difficulty**: Hard
- **Hint**: One affects updates to existing rows; the other affects insertion of new matching rows in a range scan.
- **Expected Approach**:
  - Non-repeatable read: Transaction A reads row 1. Transaction B updates row 1 and commits. Transaction A re-reads row 1 and sees updated values.
  - Phantom read: Transaction A reads all rows where `age > 30`. Transaction B inserts a new row with `age = 35` and commits. Transaction A re-executes range scan and sees new "phantom" row.
- **Common Mistakes**: Confusing row-level modification with range-level insertion.

---

## 💻 2. Coding Questions

### Q2.1: Find Consecutive Available Seats in a Theater
- **Difficulty**: Medium
- **Hint**: Use `LEAD()` or `ROW_NUMBER()` difference to identify adjacent free seat IDs.
- **Problem Statement**: Given a `Cinema` table with columns `seat_id` (INT) and `free` (BOOL: 1 for empty, 0 for occupied), return all consecutive available seat IDs.

#### Expected Approach
```sql
WITH ConsecutiveSeats AS (
    SELECT seat_id, free,
           LAG(free) OVER (ORDER BY seat_id) AS prev_free,
           LEAD(free) OVER (ORDER BY seat_id) AS next_free
    FROM Cinema
)
SELECT seat_id
FROM ConsecutiveSeats
WHERE free = 1 AND (prev_free = 1 OR next_free = 1);
```
- **Common Mistakes**: Only checking `prev_free = 1` which misses the trailing seat in a pair.

---

## 🐛 3. Debugging Questions

### Q3.1: Fix the Invalid Query in Department Highest Salary
- **Difficulty**: Medium
- **Problem Statement**: Identify and fix all bugs in the following candidate submission:

```sql
-- FAULTY QUERY
SELECT d.name AS Department, e.name AS Employee, MAX(e.salary) AS Salary
FROM Employee e
JOIN Department d ON e.department_id = d.id
WHERE e.salary = MAX(e.salary);
```

#### Bugs Identified & Expected Approach
1. Cannot use `MAX(e.salary)` inside a `WHERE` clause.
2. `SELECT d.name, e.name` without `GROUP BY e.name` causes non-aggregate projection error.
3. Multiple employees might share the max salary.

#### Corrected Query
```sql
WITH DepartmentMaxSalary AS (
    SELECT e.name AS Employee, 
           e.salary AS Salary, 
           d.name AS Department,
           DENSE_RANK() OVER (PARTITION BY e.department_id ORDER BY e.salary DESC) as rnk
    FROM Employee e
    JOIN Department d ON e.department_id = d.id
)
SELECT Department, Employee, Salary
FROM DepartmentMaxSalary
WHERE rnk = 1;
```

---

## 🔮 4. Output Prediction Questions

### Q4.1: NULL Aggregate Predictions
Given table `SampleData` containing a single column `val` with values: `{10, 20, NULL, 30, NULL}`.

Predict the output of the following queries:

```sql
Query A: SELECT COUNT(*) FROM SampleData;
Query B: SELECT COUNT(val) FROM SampleData;
Query C: SELECT AVG(val) FROM SampleData;
Query D: SELECT SUM(val) FROM SampleData;
```

#### Expected Output
- **Query A**: `5` (`COUNT(*)` counts all rows including NULLs).
- **Query B**: `3` (`COUNT(col)` ignores NULLs).
- **Query C**: `20.0` (`(10 + 20 + 30) / 3 = 60 / 3 = 20.0`. Note denominator is 3, not 5!).
- **Query D**: `60` (Ignores NULLs).

---

## 🏢 5. Scenario-Based Questions

### Q5.1: Sudden High CPU & Lock Wait Outage
- **Difficulty**: Hard
- **Scenario**: A checkout microservice experiences severe latency. Database CPU jumps to 100%, and transaction connection pools are exhausted with `Lock Wait Timeout Exceeded`.
- **Hint**: Look for missing indexes on Foreign Keys during `DELETE`/`UPDATE` or unindexed bulk updates.
- **Expected Diagnosis Steps**:
  1. Inspect active processes via `SHOW FULL PROCESSLIST` or `pg_stat_activity`.
  2. Identify blocking query holding table/row locks.
  3. Check execution plan for full table scans.
  4. Fix: Add index on foreign key / filter predicate; terminate blocking transaction safely.

---

## ⚠️ 6. Tricky Questions

### Q6.1: String vs Numeric Comparison Coercion
- **Difficulty**: Medium
- **Problem Statement**: Predict result in MySQL: `SELECT '10' > '9'` vs `SELECT 10 > '9'`.
- **Explanation**:
  - `'10' > '9'` evaluates lexicographically string-by-string: `'1' < '9'`, so `'10' > '9'` is **FALSE (0)**.
  - `10 > '9'` implicitly converts string `'9'` to integer `9`: `10 > 9` is **TRUE (1)**.

---

## 🛠️ 7. Practical Problems

### Q7.1: Deduplicating Customer Records without ID
- **Difficulty**: Hard
- **Problem Statement**: A legacy table `Customers` has exact duplicate rows (`first_name`, `last_name`, `email`) with no primary key column. Deduplicate the table.

#### Expected Solution (PostgreSQL `ctid` / Oracle `rowid`)
```sql
DELETE FROM Customers c1
USING Customers c2
WHERE c1.email = c2.email
  AND c1.first_name = c2.first_name
  AND c1.last_name = c2.last_name
  AND c1.ctid > c2.ctid;
```

---

## 🏆 8. Interview Challenges

### Q8.1: Real-time Fraud Windowing Challenge
- **Difficulty**: Very Hard
- **Problem Statement**: Detect any user who makes 3 or more transactions within any rolling 10-minute period.

```sql
WITH FlaggedTransactions AS (
    SELECT user_id, transaction_id, transaction_time,
           COUNT(*) OVER (
               PARTITION BY user_id 
               ORDER BY transaction_time 
               RANGE BETWEEN INTERVAL '10 minutes' PRECEDING AND CURRENT ROW
           ) as txn_count_10m
    FROM UserTransactions
)
SELECT DISTINCT user_id
FROM FlaggedTransactions
WHERE txn_count_10m >= 3;
```
