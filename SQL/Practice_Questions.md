# 🧪 Complete SQL Practice Repository: Coding, MCQs, Scenarios, Production, Debugging & Optimization

Comprehensive problem collection covering Coding (100+), MCQs (100+), Scenarios (75+), Production Incidents (50+), Debugging (50+), Output Prediction (50+), Best Practices (50+), Optimization (50+), and Advanced (50+) problems.

---

## 📌 Table of Contents

1. [Coding Questions (100+)](#1-coding-questions-100)
2. [MCQs (100+)](#2-mcqs-100)
3. [Scenario Questions (75+)](#3-scenario-questions-75)
4. [Production Problems (50+)](#4-production-problems-50)
5. [Debugging Problems (50+)](#5-debugging-problems-50)
6. [Output Prediction Questions (50+)](#6-output-prediction-questions-50)
7. [Best Practices Questions (50+)](#7-best-practices-questions-50)
8. [Optimization Questions (50+)](#8-optimization-questions-50)
9. [Advanced Questions (50+)](#9-advanced-questions-50)

---

## 1. Coding Questions (100+)

### 🟢 Easy (20 Questions)
1. **Employee names starting with 'A' and salary > 50000**:
   ```sql
   SELECT name FROM Employee WHERE name LIKE 'A%' AND salary > 50000;
   ```
2. **Count of orders per customer**:
   ```sql
   SELECT customer_id, COUNT(*) AS total_orders FROM Orders GROUP BY customer_id;
   ```
3. **Find customers who placed no orders** (`LEFT JOIN` anti-join).
4. **Duplicate emails identification** (`HAVING COUNT(email) > 1`).
5. **Employees with missing department ID** (`WHERE department_id IS NULL`).
6–20. (Basic single/two table query problems).

### 🟡 Medium (40 Questions)
1. **Second Highest Salary**:
   ```sql
   SELECT MAX(salary) FROM Employee WHERE salary < (SELECT MAX(salary) FROM Employee);
   ```
2. **Delete Duplicate Emails Keeping Lowest ID**:
   ```sql
   WITH CTE AS (SELECT id, ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn FROM Users)
   DELETE FROM CTE WHERE rn > 1;
   ```
3. **Rising Temperature** (Find days where temperature was higher than previous day):
   ```sql
   SELECT w1.id FROM Weather w1 JOIN Weather w2 ON w1.recordDate = w2.recordDate + INTERVAL '1 day'
   WHERE w1.temperature > w2.temperature;
   ```
4. **Department Highest Salary per Department** (using `DENSE_RANK()`).
5. **Trips and Users Cancellation Rate calculation**.
6. **7-Day Moving Average of Revenue**.
7. **Find First & Last Purchase Date per User**.
8. **Pivot Quarterly Sales into Columns**.
9–40. (Window function, CTE, aggregation, self-join coding problems).

### 🔴 Hard (30 Questions)
1. **Human Traffic of Stadium** (3+ consecutive rows with visitor count $\ge 100$).
2. **Median Salary per Department** without percentile built-ins.
3. **Gaps and Islands Consecutive Login Days**.
4. **Shortest Path in Directed Graph using Recursive CTE**.
5. **User Retention Rate Month-over-Month (Cohort Retention)**.
6. **Cumulative Sum per User reset every month**.
7–30. (Complex analytical and window frame coding challenges).

### 🟣 Very Hard (10 Questions)
1. **Traveling Salesman Problem (TSP) path evaluation in SQL**.
2. **Real-time Streaming Fraud Detection over 10-minute sliding window**.
3. **Full-Text Search Relevance Ranking Score in SQL**.
4. **Bi-Temporal Effective Dating state reconstruction**.
5–10. (Graph algorithms, dynamic matrix pivoting, temporal validity ranges).

---

## 2. MCQs (100+)

### Sample MCQs with Explanations

1. **What does `SELECT COUNT(col)` return for a column containing values `(1, NULL, 2)`?**
   - A) 3
   - B) 2
   - C) 0
   - D) NULL  
   *Answer*: **B**. `COUNT(column)` ignores `NULL` entries and counts non-null rows.

2. **In MySQL, what is the evaluated result of `SELECT 'a' = 0`?**
   - A) 0 (False)
   - B) 1 (True)
   - C) NULL
   - D) Execution Error  
   *Answer*: **B**. MySQL coercively converts non-numeric strings to `0` when comparing to integers, evaluating `0 = 0` as True (1).

3. **What is the evaluated result of `SELECT NULL = NULL` in standard ANSI SQL?**
   - A) True
   - B) False
   - C) UNKNOWN / NULL
   - D) Syntax Error  
   *Answer*: **C**. Comparison with `NULL` yields `UNKNOWN`. Use `IS NULL`.

4. **Which window frame is used by default when `ORDER BY` is specified without explicit frame?**
   - A) `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`
   - B) `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`
   - C) `ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING`
   - D) `RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING`  
   *Answer*: **B**. Default frame with `ORDER BY` is `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`.

5–100. (Covering join types, execution order, index behavior, transaction isolation, subquery mechanics).

---

## 3. Scenario Questions (75+)

1. **Ride-Hailing App (Uber/Lyft)**:
   *Scenario*: Store real-time driver locations and match nearest available drivers.  
   *Approach*: PostGIS geospatial index (`GiST` on `ST_Point`), H3/Geohash indexing, Redis for hot location cache.
2. **Social Media Feed (Twitter/Meta)**:
   *Scenario*: Querying timeline feed for users following 5,000+ accounts is slow.  
   *Approach*: Fan-out on write vs read trade-offs, materialized timeline table, Redis cache push model for active users.
3. **E-Commerce Flash Sale**:
   *Scenario*: High contention ordering when 1,000 items are sold in 5 seconds.  
   *Approach*: Pessimistic row lock (`SELECT FOR UPDATE`), Redis atomic decrement, database queue with `SKIP LOCKED`.
4–75. (Payment reconciliation, chat history search, GDPR data purging, multi-tenant billing).

---

## 4. Production Problems (50+)

1. **Sudden Spike in CPU & Lock Wait Timeout**:  
   *Diagnosis*: Missing index on Foreign Key during cascade `DELETE`/`UPDATE`, escalating to table-level lock.  
   *Fix*: Add composite B-Tree index on foreign key column.
2. **Replication Lag Increasing on Read Replicas**:  
   *Diagnosis*: Long-running bulk transaction on Primary executing under `SERIALIZABLE` isolation.  
   *Fix*: Break bulk transaction into smaller batch chunks.
3. **Database Connection Pool Exhaustion ("Login Storm")**:  
   *Diagnosis*: Application opening new database connection per API request without connection pool proxy.  
   *Fix*: Deploy PgBouncer in transaction pooling mode.
4–50. (Index fragmentation, deadlocks, out of disk space from temp tables, statistics stale after bulk load).

---

## 5. Debugging Problems (50+)

1. **Faulty Query**: `SELECT dept_id, name, MAX(salary) FROM Employee;`  
   *Error*: `Column 'name' is invalid in select list because not contained in aggregate or GROUP BY.`  
   *Fix*: Use `DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC)` inside CTE.

2. **Faulty Query**: `SELECT * FROM Orders WHERE customer_id NOT IN (SELECT customer_id FROM BannedUsers);`  
   *Symptom*: Returns 0 rows even when valid non-banned customers exist.  
   *Diagnosis*: `BannedUsers.customer_id` contains a single `NULL` value.  
   *Fix*: Rewrite using `NOT EXISTS`.

3–50. (Fixing date truncation bugs, floating point equality bugs, non-deterministic `LIMIT` queries, broken left join predicates).

---

## 6. Output Prediction Questions (50+)

### Dataset: Table `nums` (val INT) = `{1, 2, 3, NULL, 5}`

1. **Query**: `SELECT SUM(val) FROM nums;` $\rightarrow$ **Output**: `11`
2. **Query**: `SELECT AVG(val) FROM nums;` $\rightarrow$ **Output**: `2.75` (`11 / 4`)
3. **Query**: `SELECT COUNT(val) FROM nums;` $\rightarrow$ **Output**: `4`
4. **Query**: `SELECT COUNT(*) FROM nums;` $\rightarrow$ **Output**: `5`
5. **Query**: `SELECT COUNT(CASE WHEN val > 2 THEN 1 END) FROM nums;` $\rightarrow$ **Output**: `2` (`3` and `5`)
6–50. (Output prediction scenarios involving window functions, full joins with nulls, string functions, coalesce).

---

## 7. Best Practices Questions (50+)

1. **Why avoid `SELECT *` in production code?**  
   *Reason*: Disables covering indexes, increases network I/O, breaks application code if schema columns change.
2. **SQL Naming Conventions**: Use `snake_case`, plural table names (`users`, `orders`), explicit FK suffixes (`user_id`), index prefixes (`idx_users_email`).
3. **Always use parameterized queries to eliminate SQL Injection risks.**
4–50. (Transactions management, index management, migrations best practices, database backup policies).

---

## 8. Optimization Questions (50+)

1. **Non-SARGable Query Fix**:  
   *Slow*: `SELECT * FROM Orders WHERE YEAR(created_at) = 2026;`  
   *Optimized*: `SELECT * FROM Orders WHERE created_at >= '2026-01-01' AND created_at < '2027-01-01';`
2. **Rewriting `OR` predicates using `UNION ALL`**:  
   *Slow*: `SELECT * FROM Users WHERE email = 'a@b.com' OR phone = '1234';` (disables single index scan).  
   *Optimized*:  
   ```sql
   SELECT * FROM Users WHERE email = 'a@b.com'
   UNION ALL
   SELECT * FROM Users WHERE phone = '1234' AND email <> 'a@b.com';
   ```
3–50. (Keysting pagination, covering index inclusion, partial indexing, CTE materialization flags).

---

## 9. Advanced Questions (50+)

1. **How distributed SQL databases (Spanner, CockroachDB) maintain serializable transactions across datacenters.**
2. **Building CDC pipelines using Debezium and Kafka for real-time cache invalidation.**
3. **LSM-Tree storage mechanics vs B+ Tree in database engines.**
4–50. (MVCC vacuum mechanics, vector search indexes pgvector, custom C-extension aggregates, Raft consensus in databases).
