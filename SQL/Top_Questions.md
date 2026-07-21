# 🔝 Top SQL Interview Questions (40–60 Core Questions)

Curated questions repeatedly tested in technical interview loops at top software and product companies.

---

## 📌 Table of Contents

1. [Question 1: Nth Highest Salary](#question-1-nth-highest-salary)
2. [Question 2: Delete Duplicate Rows Keeping One](#question-2-delete-duplicate-rows-keeping-one)
3. [Question 3: Gaps and Islands (Consecutive Logins)](#question-3-gaps-and-islands-consecutive-logins)
4. [Question 4: Customers Who Never Ordered (Anti-Join)](#question-4-customers-who-never-ordered-anti-join)
5. [Question 5: Running Total / Cumulative Sum](#question-5-running-total--cumulative-sum)
6. [Question 6: Top N Salaries Per Department](#question-6-top-n-salaries-per-department)
7. [Question 7: First and Last Value in Group](#question-7-first-and-last-value-in-group)
8. [Question 8: Recursive CTE for Org Chart / Hierarchy](#question-8-recursive-cte-for-org-chart--hierarchy)
9. [Question 9: Pivot Rows to Columns Dynamic / Static](#question-9-pivot-rows-to-columns-dynamic--static)
10. [Question 10: 7-Day Moving Average](#question-10-7-day-moving-average)
11. [Question 11: Median Calculation without Built-in Percentiles](#question-11-median-calculation-without-built-in-percentiles)
12. [Question 12: ACID Properties vs BASE Architecture](#question-12-acid-properties-vs-base-architecture)
13. [Question 13: Transaction Isolation Levels & Anomalies](#question-13-transaction-isolation-levels--anomalies)
14. [Question 14: Index Types: B-Tree vs Hash vs Columnar](#question-14-index-types-b-tree-vs-hash-vs-columnar)
15. [Question 15: Optimizing Slow Queries (EXPLAIN Plan Analysis)](#question-15-optimizing-slow-queries-explain-plan-analysis)
16. [Question 16: WHERE vs HAVING Execution & Use Cases](#question-16-where-vs-having-execution--use-cases)
17. [Question 17: Inner Join vs Outer Joins vs Cross Join](#question-17-inner-join-vs-outer-joins-vs-cross-join)
18. [Question 18: Deadlocks Detection & Prevention](#question-18-deadlocks-detection--prevention)
19. [Question 19: Design Indexes for Mixed OLTP/OLAP Workloads](#question-19-design-indexes-for-mixed-oltpolap-workloads)
20. [Question 20: Slowly Changing Dimensions (SCD Type 2) Implementation](#question-20-slowly-changing-dimensions-scd-type-2-implementation)
21. [Question 21: Sharding & Database Partitioning Strategies](#question-21-sharding--database-partitioning-strategies)
22. [Question 22: UNION vs UNION ALL Performance Differences](#question-22-union-vs-union-all-performance-differences)
23. [Question 23: Database Normalization (1NF to 3NF & BCNF)](#question-23-database-normalization-1nf-to-3nf--bcnf)
24. [Question 24: Movie Ticket Booking System Schema Design](#question-24-movie-ticket-booking-system-schema-design)
25. [Question 25: Clustered vs Non-Clustered Indexes](#question-25-clustered-vs-non-clustered-indexes)
26. [Question 26: SARGable Queries vs Non-SARGable Queries](#question-26-sargable-queries-vs-non-sargable-queries)
27. [Question 27: Keyset Pagination vs OFFSET Pagination](#question-27-keyset-pagination-vs-offset-pagination)
28. [Question 28: Correlated Subquery vs JOIN Performance](#question-28-correlated-subquery-vs-join-performance)
29. [Question 29: Optimistic vs Pessimistic Concurrency Locking](#question-29-optimistic-vs-pessimistic-concurrency-locking)
30. [Question 30: Materialized View vs Regular View](#question-30-materialized-view-vs-regular-view)
31. [Question 31: Detect Cycles in Directed Graph (Recursive CTE)](#question-31-detect-cycles-in-directed-graph-recursive-cte)
32. [Question 32: Unpivoting Columns into Rows](#question-32-unpivoting-columns-into-rows)
33. [Question 33: Find Monthly Active Users (MAU) & Churn Rate](#question-33-find-monthly-active-users-mau--churn-rate)
34. [Question 34: Write Skew Anomaly in Snapshot Isolation](#question-34-write-skew-anomaly-in-snapshot-isolation)
35. [Question 35: Handling Late-Arriving Data in SQL Pipelines](#question-35-handling-late-arriving-data-in-sql-pipelines)
36. [Question 36: Querying JSON / JSONB Fields in Postgres](#question-36-querying-json--jsonb-fields-in-postgres)
37. [Question 37: FOR UPDATE SKIP LOCKED for Job Queues](#question-37-for-update-skip-locked-for-job-queues)
38. [Question 38: Soft Delete Design vs Hard Delete Patterns](#question-38-soft-delete-design-vs-hard-delete-patterns)
39. [Question 39: SQL Injection Prevention in Prepared Statements](#question-39-sql-injection-prevention-in-prepared-statements)
40. [Question 40: Multi-Tenant Schema Design (Shared vs Separate DB)](#question-40-multi-tenant-schema-design-shared-vs-separate-db)

---

### Question 1: Nth Highest Salary
- **Difficulty**: Medium
- **Why Interviewers Ask It**: Evaluates subqueries, window functions, handling `NULL` values when $N$ exceeds row count, and handling tie salaries.

#### Detailed Answer
To find the $N$-th highest distinct salary, `DENSE_RANK()` is the preferred modern solution because it accounts for ties and does not create gaps in rank sequences.

```sql
WITH RankedSalaries AS (
    SELECT salary, 
           DENSE_RANK() OVER (ORDER BY salary DESC) as rnk
    FROM Employee
)
SELECT DISTINCT salary AS NthHighestSalary
FROM RankedSalaries
WHERE rnk = 2; -- Change 2 to N
```

Alternative standard approach using Subquery for $N=2$:
```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

#### Simple Explanation
- `DENSE_RANK()` assigns rank 1 to the highest salary. If two employees share the top salary, both get rank 1, and the next highest salary gets rank 2.
- Selecting `WHERE rnk = N` gets the exact $N$-th distinct salary level.

#### Follow-up Questions
- *What if the table has fewer than N rows?* Wrap query in `COALESCE` or scalar subquery to return `NULL`.
- *How would you do this in MySQL without window functions?* Use `SELECT DISTINCT salary FROM Employee ORDER BY salary DESC LIMIT 1 OFFSET N-1`.

#### Common Mistakes & Tips
- ❌ Using `LIMIT 1 OFFSET N-1` without `DISTINCT` (returns duplicate salaries if ties exist).
- 💡 Clarify with the interviewer: "Do you want the $N$-th distinct salary or the $N$-th employee row?"

---

### Question 2: Delete Duplicate Rows Keeping One
- **Difficulty**: Medium
- **Why Interviewers Ask It**: Tests data manipulation safety, row identification without primary keys, and CTE usages with `ROW_NUMBER()`.

#### Detailed Answer
```sql
WITH CTE_Duplicates AS (
    SELECT id, email,
           ROW_NUMBER() OVER (
               PARTITION BY email 
               ORDER BY id ASC
           ) as row_num
    FROM Users
)
DELETE FROM CTE_Duplicates
WHERE row_num > 1;
```

#### Simple Explanation
- `PARTITION BY email` groups identical emails together.
- `ORDER BY id ASC` orders rows so the lowest `id` gets `row_num = 1`.
- Any row with `row_num > 1` is a duplicate and is deleted safely through the CTE.

#### Follow-up Questions
- *How do you perform this operation if your database engine does not support deleting from CTEs (e.g. older MySQL)?*
  Use a self-join: `DELETE u1 FROM Users u1 JOIN Users u2 ON u1.email = u2.email WHERE u1.id > u2.id;`

#### Common Mistakes & Tips
- ❌ Using `DELETE FROM Users WHERE id NOT IN (SELECT MIN(id) FROM Users GROUP BY email)` without realizing MySQL fails when modifying the target table in a subquery.

---

### Question 3: Gaps and Islands (Consecutive Logins)
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Classic algorithmic SQL problem testing advanced window function manipulation (`ROW_NUMBER()` sequence subtraction trick).

#### Detailed Answer
Find users who logged in for at least 3 consecutive days:

```sql
WITH DistinctLogins AS (
    SELECT DISTINCT user_id, CAST(login_time AS DATE) AS login_date
    FROM UserLogins
),
GroupedLogins AS (
    SELECT user_id, login_date,
           DATEADD(day, -ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date), login_date) AS island_group
    FROM DistinctLogins
)
SELECT user_id,
       MIN(login_date) AS streak_start,
       MAX(login_date) AS streak_end,
       COUNT(*) AS consecutive_days
FROM GroupedLogins
GROUP BY user_id, island_group
HAVING COUNT(*) >= 3;
```

#### Simple Explanation
- Subtracting a sequential row number (`1, 2, 3...`) from a sequence of consecutive dates (`2026-03-01, 2026-03-02, 2026-03-03...`) yields the **same starting base date** (the "island").
- When a gap occurs in login dates, the computed base date shifts, creating a new group.

#### Common Mistakes & Tips
- ❌ Forgetting to run `SELECT DISTINCT user_id, DATE` first (multiple logins on the same day corrupt the consecutive count).

---

### Question 4: Customers Who Never Ordered (Anti-Join)
- **Difficulty**: Easy
- **Why Interviewers Ask It**: Evaluates relational set operations, `LEFT JOIN` filtering vs `NOT EXISTS` vs `NOT IN`.

#### Detailed Answer
```sql
-- Preferred Production Approach (NOT EXISTS)
SELECT c.customer_id, c.name
FROM Customers c
WHERE NOT EXISTS (
    SELECT 1 
    FROM Orders o 
    WHERE o.customer_id = c.customer_id
);

-- Alternative Approach (LEFT JOIN / Anti-Join)
SELECT c.customer_id, c.name
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
WHERE o.customer_id IS NULL;
```

#### Simple Explanation
- `LEFT JOIN` preserves all rows from `Customers`. If a customer has no match in `Orders`, the join populates `o.customer_id` with `NULL`. Filtering for `WHERE o.customer_id IS NULL` returns customers with zero orders.

#### Common Mistakes & Tips
- ❌ Writing `WHERE customer_id NOT IN (SELECT customer_id FROM Orders)`. If `Orders.customer_id` contains a single `NULL` value, `NOT IN` returns zero rows across the entire query due to 3-valued logic (`UNKNOWN`).

---

### Question 5: Running Total / Cumulative Sum
- **Difficulty**: Medium
- **Why Interviewers Ask It**: Tests knowledge of window frame specifications (`ROWS UNBOUNDED PRECEDING`).

#### Detailed Answer
```sql
SELECT transaction_id, user_id, transaction_date, amount,
       SUM(amount) OVER (
           PARTITION BY user_id 
           ORDER BY transaction_date 
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_total
FROM Transactions;
```

#### Simple Explanation
- `PARTITION BY user_id` calculates cumulative sums independently for each user.
- `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` instructs the database to sum all previous transaction amounts up to the current row.

#### Follow-up Questions
- *What is the difference between ROWS and RANGE here?*
  If multiple transactions share the exact same `transaction_date`, `RANGE` sums all duplicate dates together into the frame, whereas `ROWS` evaluates line by line physically.

---

### Question 6: Top N Salaries Per Department
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Evaluates window partition logic and filtering ranks via CTEs.

#### Detailed Answer
```sql
WITH DepartmentRanks AS (
    SELECT e.name AS employee_name, 
           e.salary, 
           d.name AS department_name,
           DENSE_RANK() OVER (
               PARTITION BY e.department_id 
               ORDER BY e.salary DESC
           ) as rnk
    FROM Employee e
    JOIN Department d ON e.department_id = d.id
)
SELECT department_name, employee_name, salary
FROM DepartmentRanks
WHERE rnk <= 3;
```

#### Simple Explanation
- Employees are ranked independently within their department (`PARTITION BY department_id`).
- Selecting `WHERE rnk <= 3` retrieves the top 3 distinct salaries per department.

---

### Question 7: First and Last Value in Group
- **Difficulty**: Medium
- **Why Interviewers Ask It**: Tests understanding of frame defaults with `LAST_VALUE()`.

#### Detailed Answer
```sql
SELECT DISTINCT department_id,
       FIRST_VALUE(salary) OVER (
           PARTITION BY department_id 
           ORDER BY hire_date ASC
       ) AS first_hired_salary,
       LAST_VALUE(salary) OVER (
           PARTITION BY department_id 
           ORDER BY hire_date ASC
           RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
       ) AS latest_hired_salary
FROM Employee;
```

#### Common Mistakes & Tips
- ❌ Omitting `RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` when calling `LAST_VALUE()`. By default, the frame ends at `CURRENT ROW`, causing `LAST_VALUE()` to return the current row's value rather than the true last value of the partition.

---

### Question 8: Recursive CTE for Org Chart / Hierarchy
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Evaluates handling tree and graph structures stored in relational databases.

#### Detailed Answer
```sql
WITH RECURSIVE OrgHierarchy AS (
    -- Anchor Member: Top-level executives (manager_id IS NULL)
    SELECT emp_id, name, manager_id, 1 AS level, CAST(name AS VARCHAR(1000)) AS path
    FROM Employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive Member: Join employees to their managers
    SELECT e.emp_id, e.name, e.manager_id, h.level + 1, CAST(h.path || ' -> ' || e.name AS VARCHAR(1000))
    FROM Employees e
    JOIN OrgHierarchy h ON e.manager_id = h.emp_id
)
SELECT * FROM OrgHierarchy ORDER BY path;
```

#### Follow-up Questions
- *How do you prevent infinite recursion if a cyclic relationship exists (e.g. A manages B, B manages A)?*
  In PostgreSQL, tracking path array `CYCLE emp_id SET is_cycle USING path` or limiting recursion depth (`WHERE level < 20`).

---

### Question 9: Pivot Rows to Columns Dynamic / Static
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Tests reporting transformations and cross-tabulation.

#### Detailed Answer (Static Pivot via Conditional Aggregation)
```sql
SELECT year,
       SUM(CASE WHEN quarter = 'Q1' THEN sales_amount ELSE 0 END) AS Q1_Sales,
       SUM(CASE WHEN quarter = 'Q2' THEN sales_amount ELSE 0 END) AS Q2_Sales,
       SUM(CASE WHEN quarter = 'Q3' THEN sales_amount ELSE 0 END) AS Q3_Sales,
       SUM(CASE WHEN quarter = 'Q4' THEN sales_amount ELSE 0 END) AS Q4_Sales
FROM QuarterlySales
GROUP BY year;
```

---

### Question 10: 7-Day Moving Average
- **Difficulty**: Medium
- **Why Interviewers Ask It**: Essential for financial and time-series analytics.

#### Detailed Answer
```sql
SELECT sales_date, daily_amount,
       AVG(daily_amount) OVER (
           ORDER BY sales_date 
           ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
       ) AS moving_avg_7d
FROM DailySales;
```

---

### Question 11: Median Calculation without Built-in Percentiles
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Tests mathematical translation into set-based SQL logic for even/odd row counts.

#### Detailed Answer
```sql
WITH OrderedData AS (
    SELECT val,
           ROW_NUMBER() OVER (ORDER BY val ASC) AS row_asc,
           ROW_NUMBER() OVER (ORDER BY val DESC) AS row_desc
    FROM Dataset
)
SELECT AVG(val) AS median
FROM OrderedData
WHERE row_asc IN (row_desc, row_desc - 1, row_desc + 1);
```

#### Simple Explanation
- For odd number of rows, the middle element has `row_asc = row_desc`.
- For even number of rows, the two middle elements satisfy `row_asc = row_desc - 1` or `row_asc = row_desc + 1`. Taking the `AVG()` yields the correct statistical median.

---

### Question 12: ACID Properties vs BASE Architecture
- **Difficulty**: Medium
- **Why Interviewers Ask It**: Fundamentals of database systems and transaction safety.

#### Detailed Answer
- **ACID** (RDBMS Guarantee):
  - **Atomicity**: All operations in a transaction succeed or all roll back.
  - **Consistency**: Database transitions from one valid state to another, enforcing constraints.
  - **Isolation**: Concurrent transactions execute without interfering with each other.
  - **Durability**: Committed data survives system crashes and power failures.
- **BASE** (NoSQL / Distributed Systems):
  - **Basically Available**: System guarantees availability under failures.
  - **Soft State**: State may change over time without explicit input (due to eventual consistency).
  - **Eventually Consistent**: Data converges to consistent state across nodes over time.

---

### Question 13: Transaction Isolation Levels & Anomalies
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Critical for backend concurrency, payments, and inventory management.

#### Detailed Answer
1. **Read Uncommitted**: Suffers from **Dirty Reads** (reading uncommitted changes of another transaction).
2. **Read Committed**: Prevents Dirty Reads. Suffers from **Non-Repeatable Reads** (re-reading same row yields different data due to another committed UPDATE).
3. **Repeatable Read**: Prevents Non-Repeatable Reads. Suffers from **Phantom Reads** in standard ANSI (re-reading a range yields new inserted rows).
4. **Serializable**: Strict execution order; prevents all anomalies including **Write Skew**.

---

### Question 14: Index Types: B-Tree vs Hash vs Columnar
- **Difficulty**: Medium
- **Why Interviewers Ask It**: Tests database storage engine knowledge.

#### Detailed Answer
- **B-Tree Index**: Self-balancing search tree. Supports range scans (`<`, `>`, `BETWEEN`), equality (`=`), and prefix searches. O(log N) lookup.
- **Hash Index**: Hash table implementation. O(1) exact equality lookup (`=`). Does not support range queries or sorting.
- **Columnar Index** (ClickHouse, Snowflake, Redshift): Stores data by column rather than row. Ideal for OLAP aggregate queries (`SUM`, `AVG` across millions of rows) due to vector compression and selective I/O.

---

### Question 15: Optimizing Slow Queries (EXPLAIN Plan Analysis)
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Evaluates real-world performance engineering skills.

#### Systematic Optimization Workflow
1. Run `EXPLAIN (ANALYZE, BUFFERS)` in Postgres or `EXPLAIN FORMAT=JSON` in MySQL.
2. Check for **Sequential Scans / Full Table Scans** on large tables.
3. Inspect **Estimated Rows vs Actual Rows**: Large discrepancies indicate outdated table statistics (`ANALYZE table_name` needed).
4. Look for **Nested Loop Joins** operating on large datasets without indexes (should be Hash Join or indexed scan).
5. Identify **Sort Nodes spilling to disk** (`WorkMem` exhaustion). Increase sort buffer memory or add index with desired sort order.
6. Verify **SARGability**: Ensure indexed columns are not wrapped in functions.

---

### Question 16: WHERE vs HAVING Execution & Use Cases
- **Difficulty**: Easy
- **Why Interviewers Ask It**: Basic query execution logic.

#### Key Differences
- `WHERE` executes *before* `GROUP BY`. Filters individual rows from source tables. Cannot contain aggregate functions.
- `HAVING` executes *after* `GROUP BY`. Filters aggregated groups based on summary functions (e.g. `HAVING COUNT(*) > 5`).

---

### Question 17: Inner Join vs Outer Joins vs Cross Join
- **Difficulty**: Easy
- **Why Interviewers Ask It**: Relational join fundamentals.

#### Detailed Answer
- **INNER JOIN**: Returns only matching rows present in both tables.
- **LEFT (OUTER) JOIN**: Returns all rows from left table, plus matched rows from right table (NULL if no match).
- **RIGHT (OUTER) JOIN**: Returns all rows from right table, plus matched rows from left table.
- **FULL OUTER JOIN**: Returns all rows when there is a match in either table (NULL for non-matches).
- **CROSS JOIN**: Produces Cartesian product ($M \times N$ rows).

---

### Question 18: Deadlocks Detection & Prevention
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Crucial for high-throughput production databases.

#### Detailed Answer
- **Cause**: Transaction A locks Resource 1 and waits for Resource 2. Transaction B locks Resource 2 and waits for Resource 1.
- **Detection**: Database engine periodically runs a Wait-For Graph (WFG) cycle detection algorithm and kills the least expensive transaction (deadlock victim).
- **Prevention Strategies**:
  1. Always acquire locks on tables/rows in identical order across all application services.
  2. Keep transactions short and avoid user interaction inside open transactions.
  3. Use explicit lock timeouts (`SET lock_timeout = '5s'`).
  4. Use lower isolation levels or optimistic locking where feasible.

---

### Question 19: Design Indexes for Mixed OLTP/OLAP Workloads
- **Difficulty**: Hard
- **Why Interviewers Ask It**: Evaluates practical DBA / System Architecture trade-offs.

#### Strategy
- OLTP requires fast point lookups (`WHERE user_id = ?`) with minimal write amplification. Keep index count low. Use B-Trees on Primary Keys and Foreign Keys.
- OLAP requires heavy aggregation across wide tables. Use columnar store / covering indexes or replicate data asynchronously to a dedicated analytical data warehouse (ClickHouse / Snowflake) using Change Data Capture (CDC / Debezium).

---

### Question 20: Slowly Changing Dimensions (SCD Type 2) Implementation
- **Difficulty**: Very Hard
- **Why Interviewers Ask It**: Essential data engineering pattern for historical tracking.

#### Schema & Strategy
Maintain historical record changes by adding `start_date`, `end_date`, and `is_current` columns.

```sql
-- Expire existing current record
UPDATE DimCustomer
SET end_date = CURRENT_DATE, is_current = FALSE
WHERE customer_id = 101 AND is_current = TRUE;

-- Insert new version record
INSERT INTO DimCustomer (customer_id, address, start_date, end_date, is_current)
VALUES (101, '456 New Street', CURRENT_DATE, '9999-12-31', TRUE);
```

---

### Questions 21–40 Short Summary Matrix

| # | Topic | Key Answer Highlight |
|---|-------|----------------------|
| 21 | **Sharding** | Horizontal partitioning across database nodes using a shard key (Consistent Hashing). |
| 22 | **UNION vs UNION ALL** | `UNION` runs duplicate elimination (sort/hash cost); `UNION ALL` directly appends datasets (faster). |
| 23 | **Normalization** | 1NF (atomic values), 2NF (no partial dependencies on composite key), 3NF (no transitive dependencies). |
| 24 | **Movie Booking System** | Schema: Movies, Shows, Seats, Bookings, BookingSeats. Use optimistic locking or `SELECT FOR UPDATE` on seats. |
| 25 | **Clustered Index** | Physical storage order of table rows matches index key (only 1 per table). Non-clustered is a separate structure with pointers to table rows. |
| 26 | **SARGable Queries** | Search Argument Able: Write predicates like `col >= '2026-01-01'` instead of `YEAR(col) = 2026` to enable B-Tree scans. |
| 27 | **Keyset Pagination** | Use `WHERE id > last_seen_id ORDER BY id LIMIT 20` instead of `OFFSET 100000` (O(1) vs O(N) performance). |
| 28 | **Correlated Subquery** | Evaluates once per outer row (O(N^2) potential cost). Rewrite to JOIN or Window function for O(N log N). |
| 29 | **Optimistic Locking** | Uses `version` column: `UPDATE t SET val=?, version=version+1 WHERE id=? AND version=?`. Fails if version changed. |
| 30 | **Materialized View** | Physically stores precomputed query results on disk; requires explicit refresh (`REFRESH MATERIALIZED VIEW`). |
| 31 | **Cycle Detection** | Track visited nodes in array inside Recursive CTE: `WHERE id = ANY(path)` to break loops. |
| 32 | **Unpivot** | Converts columns into rows using `CROSS JOIN LATERAL` or `UNPIVOT` clause. |
| 33 | **MAU & Churn** | Calculate active users in current month vs prior month using `FULL OUTER JOIN` or `LAG()`. |
| 34 | **Write Skew** | Occurs under Snapshot Isolation when two transactions read overlapping data and update distinct rows violating a cross-row constraint. |
| 35 | **Late-Arriving Data** | Handle late events using upserts (`ON CONFLICT DO UPDATE`) or watermark-based ingestion windows. |
| 36 | **JSON / JSONB** | Postgres `JSONB` stores decomposed binary JSON enabling GIN indexing (`WHERE data @> '{"status": "active"}'`). |
| 37 | **Job Queues** | Use `SELECT * FROM queue_table WHERE status = 'pending' ORDER BY id LIMIT 1 FOR UPDATE SKIP LOCKED;` for safe multi-worker queues. |
| 38 | **Soft Delete** | Add `deleted_at TIMESTAMP` column with partial index (`WHERE deleted_at IS NULL`). |
| 39 | **SQL Injection** | Use Parameterized Queries / Prepared Statements (`?` or `$1`) so driver separates SQL logic from data string. |
| 40 | **Multi-Tenant Schema** | Options: Single DB + `tenant_id` column (row security), Schema per tenant, or Database per tenant (highest isolation). |
