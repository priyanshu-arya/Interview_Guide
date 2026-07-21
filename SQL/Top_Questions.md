# 🔝 Top SQL Interview Questions (Most Repeated, Difficult, Tricky, Must-Know & Differentiating)

This document contains the curated lists of top SQL interview questions synthesized from thousands of real interview loops across FAANG, Tier-1 product companies, and unicorns for 2026–2027 loops.

---

## 📌 Table of Contents

1. [Top 25 Most Repeated Questions](#1-top-25-most-repeated-questions)
2. [Top 50 Most Difficult Questions](#2-top-50-most-difficult-questions)
3. [Top 50 Most Tricky Questions](#3-top-50-most-tricky-questions)
4. [Top 50 Must-Know Questions](#4-top-50-must-know-questions)
5. [Top 50 Frequently Rejected Questions](#5-top-50-frequently-rejected-questions)
6. [Top 50 Questions That Differentiate Top Candidates](#6-top-50-questions-that-differentiate-top-candidates)

---

## 1. Top 25 Most Repeated Questions

These questions appear across **Big Tech, Unicorn, FinTech, and Startup** loops year after year.

| # | Question | Difficulty | Level | Company Categories |
|---|----------|------------|-------|-------------------|
| 1 | Find the second highest salary from an Employee table. | Medium | SDE1/2 | Big Tech, FinTech |
| 2 | Delete duplicate rows, keeping one. | Medium | SDE1/2 | All |
| 3 | Rank employees by salary within each department. | Medium | SDE2 | Big Tech, Unicorn |
| 4 | Find customers who placed no orders. | Easy | Fresher | All |
| 5 | Running total of sales by date. | Medium | SDE2 | FinTech, Big Tech |
| 6 | Find the top 3 salaries per department. | Hard | SDE2/Senior | Big Tech |
| 7 | Consecutive days login (gaps and islands). | Hard | Senior | All |
| 8 | Retrieve the first/last record of each group. | Medium | SDE2 | Unicorn |
| 9 | Recursive CTE for hierarchical data (org chart). | Hard | Senior | Big Tech, Unicorn |
| 10 | Pivot table – convert rows to columns dynamically. | Hard | Senior | Big Tech |
| 11 | Optimize a slow running query (given EXPLAIN plan). | Hard | Senior | Big Tech, FinTech |
| 12 | Design a schema for a movie ticket booking system. | Medium | SDE2 | All |
| 13 | Explain ACID vs BASE. | Medium | Senior | All |
| 14 | Difference between WHERE and HAVING. | Easy | Fresher | All |
| 15 | Left Join vs Inner Join vs Full Outer Join with examples. | Easy | Fresher | All |
| 16 | Find median of a column without using PERCENTILE_CONT. | Hard | Senior | FinTech |
| 17 | Deadlock scenario and resolution in SQL Server/MySQL. | Hard | Senior | All |
| 18 | Design indexes for a table with mixed read/write workloads. | Hard | Staff | Big Tech |
| 19 | Build a slowly changing dimension (SCD) type 2 pipeline in SQL. | Very Hard | Staff | FinTech, Big Tech |
| 20 | How would you shard a database and rebalance? | Hard | Staff | Big Tech, Unicorn |
| 21 | Find the Nth highest salary (generic solution). | Medium | SDE2 | Big Tech |
| 22 | Difference between UNION and UNION ALL. | Easy | Fresher | All |
| 23 | Transaction isolation levels and anomalies. | Hard | Senior | Big Tech |
| 24 | Calculate 7-day moving average. | Medium | SDE2 | FinTech |
| 25 | Normalize a given denormalized table to 3NF. | Medium | SDE2 | All |

---

### Detailed Solutions & Explanations for Top 25

#### 1. Find the second highest salary
- **Why asked**: Tests handling of NULLs, subqueries vs window functions, edge cases when only one row exists.
- **Ideal Answer**:
  ```sql
  -- Using DENSE_RANK (Handles ties gracefully)
  WITH RankedSalaries AS (
      SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
      FROM Employee
  )
  SELECT MAX(salary) AS SecondHighestSalary
  FROM RankedSalaries
  WHERE rnk = 2;

  -- Using Subquery
  SELECT MAX(salary) AS SecondHighestSalary
  FROM Employee
  WHERE salary < (SELECT MAX(salary) FROM Employee);
  ```
- **Explanation**: The subquery finds max salary, then outer query finds max of remaining lower salaries. If only 1 distinct salary exists, outer `MAX()` returns `NULL`.
- **Follow-up**: How to get $N$-th highest? Use `DENSE_RANK() = N` or `OFFSET N-1`.
- **Tips**: Always clarify: "Second distinct salary, or second row when ordered by salary?"

#### 2. Delete duplicate rows, keeping one
- **Why asked**: Tests row identity understanding and modifying data through CTEs/Window functions.
- **Ideal Answer**:
  ```sql
  WITH CTE AS (
      SELECT id, email,
             ROW_NUMBER() OVER (PARTITION BY email ORDER BY id ASC) AS rn
      FROM Users
  )
  DELETE FROM CTE WHERE rn > 1;
  ```
- **Common Wrong Answer**: `DELETE FROM Users WHERE id NOT IN (SELECT MIN(id) FROM Users GROUP BY email)` — fails in MySQL due to table locks in subqueries.

#### 3. Rank employees by salary within each department
- **Why asked**: Evaluates `RANK()` vs `DENSE_RANK()` vs `ROW_NUMBER()`.
- **Ideal Answer**:
  ```sql
  SELECT emp_id, dept_id, salary,
         DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk
  FROM Employee;
  ```

#### 4. Find customers who placed no orders
- **Why asked**: Tests anti-join concepts (`NOT EXISTS` vs `LEFT JOIN` vs `NOT IN`).
- **Ideal Answer**:
  ```sql
  SELECT c.id, c.name
  FROM Customers c
  WHERE NOT EXISTS (
      SELECT 1 FROM Orders o WHERE o.cust_id = c.id
  );
  ```
- **Pitfall**: `NOT IN` fails completely if subquery returns any `NULL`.

#### 5. Running total of sales by date
- **Why asked**: Frame specification (`ROWS UNBOUNDED PRECEDING`).
- **Ideal Answer**:
  ```sql
  SELECT sale_date, amount,
         SUM(amount) OVER (ORDER BY sale_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
  FROM Sales;
  ```

#### 6. Top 3 salaries per department
- **Ideal Answer**:
  ```sql
  WITH Ranked AS (
      SELECT dept_id, emp_id, salary,
             DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) as rnk
      FROM Employee
  )
  SELECT dept_id, emp_id, salary FROM Ranked WHERE rnk <= 3;
  ```

#### 7. Consecutive days login (gaps and islands)
- **Ideal Answer**:
  ```sql
  WITH numbered AS (
      SELECT DISTINCT user_id, CAST(login_time AS DATE) AS login_date,
             ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY CAST(login_time AS DATE)) AS rn
      FROM Logins
  ),
  grouped AS (
      SELECT user_id, login_date,
             DATEADD(day, -rn, login_date) AS grp
      FROM numbered
  )
  SELECT user_id, MIN(login_date) AS start_date, MAX(login_date) AS end_date, COUNT(*) AS consecutive_days
  FROM grouped
  GROUP BY user_id, grp
  HAVING COUNT(*) >= 3;
  ```

#### 8. First and last record of each group
- **Ideal Answer**:
  ```sql
  SELECT DISTINCT dept_id,
         FIRST_VALUE(salary) OVER (PARTITION BY dept_id ORDER BY hire_date ASC) AS first_salary,
         LAST_VALUE(salary) OVER (
             PARTITION BY dept_id ORDER BY hire_date ASC
             RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
         ) AS last_salary
  FROM Employee;
  ```

#### 9. Recursive CTE for org chart
- **Ideal Answer**:
  ```sql
  WITH RECURSIVE OrgChart AS (
      SELECT id, name, manager_id, 1 AS level
      FROM Employee WHERE manager_id IS NULL
      UNION ALL
      SELECT e.id, e.name, e.manager_id, o.level + 1
      FROM Employee e JOIN OrgChart o ON e.manager_id = o.id
  )
  SELECT * FROM OrgChart;
  ```

#### 10. Pivot table – convert rows to columns dynamically
- **Ideal Answer**:
  ```sql
  SELECT year,
         SUM(CASE WHEN product = 'Widget' THEN sales ELSE 0 END) AS Widget_Sales,
         SUM(CASE WHEN product = 'Gadget' THEN sales ELSE 0 END) AS Gadget_Sales
  FROM SalesData
  GROUP BY year;
  ```

#### 11. Optimize a slow running query (given EXPLAIN plan)
- **Ideal Answer**: Analyze full table scans, non-SARGable functions, outdated statistics, missing indexes on join keys, sort spills to disk (`work_mem`).

#### 12. Design a schema for a movie ticket booking system
- **Ideal Answer**: Tables: `Movies`, `Theaters`, `Screens`, `Seats`, `Shows`, `Bookings`, `BookingSeats`. Use `SELECT FOR UPDATE` or optimistic locking versioning for seat holds.

#### 13. ACID vs BASE
- **Ideal Answer**: ACID (Atomicity, Consistency, Isolation, Durability) for relational transactional consistency; BASE (Basically Available, Soft state, Eventual consistency) for scalable NoSQL systems.

#### 14. WHERE vs HAVING
- **Ideal Answer**: `WHERE` filters individual rows before aggregation; `HAVING` filters aggregated groups after `GROUP BY`.

#### 15. Join Types
- **Ideal Answer**: `INNER` (matching both), `LEFT` (all left + matched right), `RIGHT` (all right + matched left), `FULL OUTER` (all both), `CROSS` (Cartesian product).

#### 16. Median without PERCENTILE_CONT
- **Ideal Answer**:
  ```sql
  WITH Ranked AS (
      SELECT val,
             ROW_NUMBER() OVER (ORDER BY val ASC) AS r_asc,
             ROW_NUMBER() OVER (ORDER BY val DESC) AS r_desc
      FROM Table
  )
  SELECT AVG(val) AS median FROM Ranked WHERE r_asc IN (r_desc, r_desc - 1, r_desc + 1);
  ```

#### 17. Deadlock scenario and resolution
- **Ideal Answer**: Occurs when Tx1 locks A and waits for B, while Tx2 locks B and waits for A. Engine resolves via Wait-For Graph (WFG) cycle detection. Resolve by uniform lock acquiring order and index optimization.

#### 18. Design indexes for mixed read/write workloads
- **Ideal Answer**: Cover high-frequency queries with B-Tree composite indexes, minimize write overhead by keeping index count low, use filtered/partial indexes where possible.

#### 19. Build SCD Type 2 pipeline
- **Ideal Answer**: Track history using `start_date`, `end_date`, and `is_current` columns. Close old active record (`end_date = CURRENT_DATE, is_current = FALSE`) and insert new record version.

#### 20. Sharding and rebalancing
- **Ideal Answer**: Partition database horizontally by shard key (e.g. `user_id`) using Consistent Hashing. Rebalance using dual writes and shadow reads during online migration.

#### 21. Nth highest salary (generic)
- **Ideal Answer**: `SELECT DISTINCT salary FROM Employee ORDER BY salary DESC LIMIT 1 OFFSET N-1;` or `DENSE_RANK() OVER (...)`.

#### 22. UNION vs UNION ALL
- **Ideal Answer**: `UNION` runs duplicate elimination (sort/hash overhead); `UNION ALL` appends datasets directly without deduplication (significantly faster).

#### 23. Transaction isolation levels
- **Ideal Answer**: Read Uncommitted (Dirty Read), Read Committed (Non-Repeatable Read), Repeatable Read (Phantom Read), Serializable (Strict order / No anomalies).

#### 24. Calculate 7-day moving average
- **Ideal Answer**: `AVG(amount) OVER (ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)`.

#### 25. Normalize denormalized table to 3NF
- **Ideal Answer**: Ensure 1NF (atomic columns), 2NF (no partial functional dependency on composite PK), 3NF (no transitive functional dependency).

---

## 2. Top 50 Most Difficult Questions

| # | Question | Key Challenge |
|---|----------|---------------|
| 1 | Longest streak of consecutive days a user logged in, handling gaps and date overlaps | Gaps and islands with date boundaries |
| 2 | Reconstruct event order from a bi-temporal table (`version_id`, `effective_date`) | Complex time-travel joins |
| 3 | Detect potential fraud rings using graph-like queries in SQL | Recursive CTE and graph traversal |
| 4 | Implement custom aggregate (e.g. string concatenation) without built-in `STRING_AGG` | PL/pgSQL, XML PATH, or custom user aggregate |
| 5 | Moving average over dynamic rolling window (window size stored in another column) | Dynamic frame boundary limitation |
| 6 | Parse a JSON column to flatten nested arrays and aggregate across multiple levels | JSONB functions & LATERAL UNNEST |
| 7 | Optimize a query joining 10+ tables with billions of rows in a data warehouse | Partition pruning, join order, distribution keys |
| 8 | Implement consistent hashing logic in pure SQL for data sharding | Algorithmic modulo & hash simulation |
| 9 | Current version of each row from temporal table with valid-time ranges | Subquery with `NOT EXISTS` on overlapping ranges |
| 10 | MERGE statement handling SCD Type 2 and Type 1 simultaneously | Complex MERGE with multi-branch logic |
| 11 | Resolve deadlock without modifying application code | Isolation level tweak & lock escalation config |
| 12 | Efficient pagination query for deep pages (page 100,000) without sequential PK | Keyset (cursor) pagination |
| 13 | Identify closest previous order for each order with dynamic partitioning | LATERAL joins & correlated TOP 1 |
| 14 | Fill missing minute gaps in CPU metrics table with linear interpolation | `generate_series` + window frame interpolation |
| 15 | Migrate large live table across database shards with zero downtime | Shadow writes, CDC binlog replication |
| 16 | Multi-tenant schema with isolation & cross-tenant analytics | Row-Level Security (RLS) & security barrier views |
| 17 | Compute median grouped by category without window functions (pure ANSI SQL) | Self-join on row counts |
| 18 | Dynamic PIVOT where columns are unknown in advance without SQL injection | Parameterized dynamic SQL (`sp_executesql` / `EXECUTE`) |
| 19 | B-Tree vs B+ Tree internals and impact on range vs point lookups | Leaf node linked-list pointers vs internal data storage |
| 20 | Find island ranges of sequential IDs after deletions | `ROW_NUMBER()` sequence offset grouping |
| 21 | Optimize query with multiple `OR` conditions breaking index scans | Rewrite using `UNION ALL` |
| 22 | Cycle detection in directed graphs stored as parent-child edge pairs | Path array tracking in Recursive CTE |
| 23 | Stock prices per minute: find all 3 consecutive minute price increases | Conditional window running count |
| 24 | Write `FULL OUTER JOIN` using only `LEFT JOIN` and `UNION` | Anti-join union pattern |
| 25 | Parameterized search allowing partial matches on 10 optional columns | Dynamic SQL predicate assembly |
| 26 | Optimistic concurrency control implementation using `rowversion` / `version` | Conditional UPDATE matching version |
| 27 | Add non-null column with default value to 100GB table without locking | Online DDL (`pt-online-schema-change`) |
| 28 | Generate 10-year calendar date dimension table in pure SQL | Recursive CTE / `generate_series` |
| 29 | Diagnose execution plan where estimated rows = 1 but actual rows = 1,000,000 | Outdated statistics, parameter sniffing |
| 30 | Implement RLS ensuring managers see only direct & indirect reports | Recursive user-hierarchy view + RLS predicate |
| 31 | Month-over-month sales growth comparison using self-joins instead of `LAG` | Self-join on `DATE_ADD(month, 1)` |
| 32 | Double-entry accounting system balance per account at any point in time | Cumulative sign-adjusted running sum |
| 33 | Non-deterministic query results with `ORDER BY` + `LIMIT` | Missing unique tie-breaker column in `ORDER BY` |
| 34 | Dangers of `NOLOCK` hint (read uncommitted) in SQL Server | Reading dirty rows, page allocation crashes |
| 35 | Unpivot columns to rows then regroup with conditional aggregation in single pass | `CROSS JOIN LATERAL` / `VALUES` unpivot |
| 36 | Spatial query: find all restaurants within 5km radius using spatial index | PostGIS `ST_DWithin` with GiST index |
| 37 | Microservice data consistency across independent databases | Outbox pattern & Saga transactional choreography |
| 38 | Bucket event timestamps into 5-minute intervals and compute averages | `GROUP BY FLOOR(UNIX_TIMESTAMP(time) / 300)` |
| 39 | Earliest and latest order date per customer pivoted side-by-side | `MIN()` and `MAX()` aggregation with CTE |
| 40 | Database design for chat app supporting message search & history | Partitioning by `chat_id`, full-text index |
| 41 | Construct full tree path string for hierarchy node ("CEO / VP / Manager") | Concatenating string path in Recursive CTE |
| 42 | Index JSONB column for efficient query inside specific nested key | Expression index on `(json_col->>'key')` or GIN |
| 43 | Calculate Exponential Moving Average (EMA) in pure SQL | Recursive CTE with smoothing factor formula |
| 44 | Find and delete duplicate records lacking primary keys | Deleting via database physical pointer (`ctid` / `rowid`) |
| 45 | Eliminate key lookups (bookmark lookups) caused by non-clustered index | Adding `INCLUDE` columns to make index covering |
| 46 | Global distributed SQL database write conflict resolution | TrueTime, MVCC, Paxos/Raft commit timestamps |
| 47 | Total sales per region + percentage contribution of region in single query | `SUM(amount) / SUM(SUM(amount)) OVER ()` |
| 48 | Materialized View vs Regular View storage & refresh strategies | Disk persistence vs query macro rewrite |
| 49 | Find reciprocal friendships from directional follower table | Self-join `a.follower = b.followee AND a.followee = b.follower` |
| 50 | Truly random 1% sampling from billion-row table efficiently | `TABLESAMPLE SYSTEM` / Bernoulli sampling |

---

## 3. Top 50 Most Tricky Questions

| # | Tricky Question | Why Tricky |
|---|----------------|-------------|
| 1 | `SELECT COUNT(*)` vs `SELECT COUNT(col)` | `COUNT(*)` counts all rows; `COUNT(col)` ignores `NULL`s. |
| 2 | `WHERE col = NULL` vs `WHERE col IS NULL` | `col = NULL` evaluates to `UNKNOWN` (returns 0 rows). |
| 3 | Result of `SELECT AVG(val)` on values `(10, 20, NULL)` | Evaluates to `15.0` (divides by 2, not 3). |
| 4 | MySQL expression `'10' > '9'` vs `10 > '9'` | String comparison `'10' < '9'` is True; numeric coercion converts `'9'` to `9`. |
| 5 | `GROUP BY 1` pitfalls | Fragile; ordinal positional grouping breaks if `SELECT` column order changes. |
| 6 | `HAVING COUNT(*) > 1` without `GROUP BY` | Evaluates single aggregate group across entire table. |
| 7 | `SELECT * FROM t1, t2` without `WHERE` clause | Creates unintended $M \times N$ Cartesian product. |
| 8 | Filtering predicate in `ON` clause vs `WHERE` clause in `LEFT JOIN` | Predicate in `ON` filters before join; predicate in `WHERE` filters after join (converts `LEFT` to `INNER`). |
| 9 | `LIMIT 1` in correlated subqueries | Non-deterministic unless explicit `ORDER BY` is specified inside subquery. |
| 10 | `SELECT dept_id, emp_name, MAX(salary)` without `GROUP BY emp_name` | Fails standard ANSI SQL; returns arbitrary non-aggregated values in older MySQL. |
| 11 | `COALESCE(SUM(col), 0)` vs `COUNT(*)` on empty table | `SUM()` on empty table returns `NULL` (COALESCE converts to 0); `COUNT(*)` returns `0` directly. |
| 12 | `ROW_NUMBER()` without `ORDER BY` clause | Non-deterministic ordering across execution runs. |
| 13 | `UPDATE t1 SET col = t2.val FROM t2 WHERE t1.id = t2.id` with multiple t2 matches | Non-deterministic; arbitrary row from t2 updates t1. |
| 14 | `DELETE` using `USING` clause with duplicate join rows | Deletes target row multiple times conceptually; execution can yield unexpected lock escalation. |
| 15 | Short-circuit evaluation in `CASE WHEN` statements | Standard SQL does NOT guarantee left-to-right short-circuiting; division by zero can still trigger. |
| 16 | `NULL` behavior inside `IN (1, 2, NULL)` vs `NOT IN (1, 2, NULL)` | `5 IN (1, 2, NULL)` is `UNKNOWN`; `5 NOT IN (1, 2, NULL)` evaluates to `FALSE/UNKNOWN` (returns no rows). |
| 17 | Implicit data type precedence in `SELECT 1 + '2'` | MySQL converts `'2'` to numeric `2` (returns `3`); Postgres throws syntax type error. |
| 18 | `ORDER BY` column not present in `SELECT DISTINCT` list | Illegal in ANSI SQL because distinct projection discards non-selected columns. |
| 19 | `SELECT DISTINCT a, b` vs `GROUP BY a, b` duplicate elimination | Treat `NULL` values as equal for deduplication, but performance plan may differ. |
| 20 | `WHERE 5 > ALL (SELECT val FROM t)` when `t` contains `NULL` | Evaluates to `UNKNOWN` (returns no rows). |
| 21 | `EXCEPT` vs `NOT IN` handling of `NULL` values | `EXCEPT` treats `NULL` as equal values; `NOT IN` yields `UNKNOWN` if subquery contains `NULL`. |
| 22 | `LEFT JOIN` right table duplicates multiplying left rows | Causes unintended row duplication and inflated `SUM()` / `COUNT()` aggregates. |
| 23 | `INSERT INTO ... SELECT` auto-increment ID gaps on rollback | Rolled-back transactions leave non-sequential gaps in `AUTO_INCREMENT` sequence. |
| 24 | `TRUNCATE` vs `DELETE` transaction log behavior | `TRUNCATE` is DDL (deallocates pages, minimal log); `DELETE` is DML (logs individual row deletes). |
| 25 | `UNION` with different data types in corresponding columns | Implicit type coercion occurs or engine throws data type mismatch exception. |
| 26 | `FOR UPDATE` lock behavior when 0 rows match filter | In InnoDB, gap locking / next-key locking occurs on index range even if 0 rows returned. |
| 27 | `CASE` expression in `ORDER BY` performance | Non-SARGable; prevents index-ordered scan, causing expensive sort node. |
| 28 | `COUNT(DISTINCT col1, col2)` multi-column distinct support | Not supported natively in standard T-SQL/Oracle; requires string concatenation or subquery. |
| 29 | `WHERE id IN (1, 2, ... 10000)` performance tipping point | Large `IN` list causes query planner to switch from Index Range Scan to Full Table Scan. |
| 30 | `ORDER BY 'salary'` with quotes around column name | Sorts result by constant string literal `'salary'`, effectively resulting in no sorting. |
| 31 | `BETWEEN '2026-03-01' AND '2026-03-31'` on `DATETIME` columns | Misses timestamps on March 31st after `00:00:00` because string date evaluates to midnight. |
| 32 | `UPDATE` statement with `JOIN` ordering syntax across DBs | T-SQL uses `UPDATE t FROM t JOIN u`; Postgres uses `UPDATE t SET ... FROM u`; MySQL uses `UPDATE t JOIN u`. |
| 33 | `CHAR(10)` vs `VARCHAR(10)` trailing space padding | `CHAR` pads with spaces; equality checks may ignore trailing spaces depending on collation. |
| 34 | Subquery returning multiple rows to scalar operator (`WHERE val = (SELECT ...)`) | Throws runtime runtime execution error: *"Subquery returned more than 1 value"*. |
| 35 | `HAVING` clause without `GROUP BY` containing column names | Fails execution unless columns are wrapped in aggregate functions. |
| 36 | Correlated subquery in `SELECT` list performance | Evaluates subquery for every single outer row (O(N) subquery executions). |
| 37 | `LIKE '%term'` leading wildcard index usage | Non-SARGable; cannot use standard B-Tree index (requires Full-Text or Trigram index). |
| 38 | Division by integer truncation: `SELECT 5 / 2` | Integer division in SQL Server/Python2 yields `2` instead of `2.5`. |
| 39 | `SELF JOIN` missing join condition creating Cartesian exploded duplicate output | Missed predicate creates massive output explosion crashing memory. |
| 40 | Overlapping range locking under `REPEATABLE READ` | Next-Key locks lock gaps between index records to prevent phantom reads. |
| 41 | Floating point equality checks (`WHERE float_col = 0.1`) | IEEE 754 precision issues cause equality checks to fail unexpectedly. |
| 42 | `SELECT NULL = NULL`, `SELECT NULL IS NULL`, `SELECT NULL <=> NULL` | `=` is UNKNOWN, `IS` is True, `<=>` (MySQL spaceship operator) is True. |
| 43 | Adding `WHERE` filter on Right Table in `LEFT JOIN` | Accidentally converts `LEFT JOIN` into `INNER JOIN` if filter doesn't check `IS NULL`. |
| 44 | `GROUP BY` on aliased column vs underlying table column | Order of resolution: `GROUP BY` evaluates before `SELECT` aliases in standard ANSI. |
| 45 | Subquery in `FROM` clause missing mandatory alias | Oracle / Postgres throw syntax error if derived table is unaliased. |
| 46 | Multi-column composite index `(A, B)` used with predicate `WHERE B = 10` | Violates Leftmost Prefix Rule; B-Tree index scan cannot be used efficiently. |
| 47 | Primary Key vs Unique Key `NULL` constraint differences | Primary Key rejects `NULL`; Unique Key allows one (or multiple depending on DB) `NULL`s. |
| 48 | `EXCEPT` / `MINUS` set difference row deduplication | Removes duplicates automatically like `UNION`, unlike `EXCEPT ALL`. |
| 49 | `WINDOW` clause frame default when `ORDER BY` specified | Defaults to `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` (can cause memory issues). |
| 50 | Modifying variable values inside user-defined functions (UDF side effects) | Pure SQL functions prohibit side effects (cannot modify database state or run DDL). |

---

## 4. Top 50 Must-Know Questions

1. Basic CRUD statement syntax (`SELECT`, `INSERT`, `UPDATE`, `DELETE`).
2. Primary Key vs Foreign Key constraints.
3. Differences between `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, and `FULL OUTER JOIN`.
4. How `GROUP BY` and aggregate functions (`SUM`, `COUNT`, `AVG`, `MIN`, `MAX`) work.
5. Purpose of `HAVING` clause vs `WHERE` clause.
6. Execution order of SQL clauses (`FWGH SLO`).
7. Handling `NULL` values using `IS NULL`, `IS NOT NULL`, and `COALESCE()`.
8. Differences between `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()`.
9. Purpose of `LAG()` and `LEAD()` window functions.
10. Writing scalar and correlated subqueries.
11. Purpose of Common Table Expressions (CTEs) using `WITH`.
12. Difference between `UNION` and `UNION ALL`.
13. Indexing basics: What is a B-Tree index and why does it speed up reads?
14. Difference between Clustered Index and Non-Clustered Index.
15. What is database Normalization? (1NF, 2NF, 3NF).
16. What is Denormalization and when should it be used?
17. ACID properties of database transactions.
18. Purpose of `COMMIT` and `ROLLBACK` commands.
19. Difference between `TRUNCATE`, `DELETE`, and `DROP`.
20. Writing `CASE WHEN ... THEN ... ELSE ... END` conditional expressions.
21. Using `LIKE` wildcards (`%` and `_`) for pattern matching.
22. Finding second highest / $N$-th highest value in a table.
23. Deleting duplicate records while preserving one original.
24. Calculating running total / cumulative sum using window functions.
25. Top $N$ records per group filtering.
26. Anti-join pattern using `LEFT JOIN WHERE right.pk IS NULL`.
27. Using `EXISTS` vs `IN` for subquery checks.
28. Self-join application for parent-child / manager-employee queries.
29. Differences between `CHAR`, `VARCHAR`, and `TEXT` data types.
30. What is a Database View and why is it useful?
31. Materialized View vs Regular View.
32. What is a Stored Procedure and how does it differ from a Function?
33. Understanding SQL Injection and parameterization.
34. Database transaction isolation levels (`READ COMMITTED`, `REPEATABLE READ`, etc.).
35. What is a Deadlock and how to prevent it?
36. Reading basic `EXPLAIN` query execution plans.
37. Concept of SARGability in query optimization.
38. What is a Composite Index and the Leftmost Prefix Rule?
39. What is a Covering Index (`INCLUDE` columns)?
40. Database Partitioning strategies (Range, List, Hash).
41. What is Database Sharding?
42. How to handle spatial or JSON data in SQL (`JSONB`).
43. Using `FOR UPDATE` for row-level locking.
44. Difference between OLTP and OLAP workloads.
45. Star Schema vs Snowflake Schema in Data Warehousing.
46. Slowly Changing Dimensions (SCD Type 1 vs SCD Type 2).
47. How to write a Recursive CTE for tree hierarchies.
48. Using `CROSS JOIN` to generate combinations or unpivot data.
49. Optimizing deep pagination using Keyset / Cursor pagination.
50. Database connection pooling benefits and configuration.

---

## 5. Top 50 Frequently Rejected Questions

Candidates frequently fail interview rounds on these questions due to missing edge cases, bad performance habits, or lack of production awareness:

| # | Question / Scenario | Why Candidates Get Rejected |
|---|---------------------|-----------------------------|
| 1 | "Optimize this slow query" | Jumps straight to adding random indexes without inspecting `EXPLAIN` execution plans. |
| 2 | "Design database schema for food delivery app" | Ignores transaction boundaries and atomic state updates during order payment. |
| 3 | "Handle seat booking concurrency" | Relies only on application-level `if` checks; misses database row locking (`FOR UPDATE`). |
| 4 | "Explain database normalization" | Recites textbook definitions of 3NF but cannot normalize a practical real-world table. |
| 5 | "Find the median of a column" | Writes query that works for odd row count but fails or averages incorrectly for even row count. |
| 6 | "When to use NoSQL vs Relational SQL?" | Uses generic buzzwords ("NoSQL is faster") without articulating ACID trade-offs or access patterns. |
| 7 | "Delete duplicate rows from table" | Deletes ALL duplicate rows including original, leaving zero records. |
| 8 | "Write recursive CTE for hierarchy" | Forgets termination condition or depth safety limit, causing infinite recursion loop in production. |
| 9 | "Find top $N$ per department" | Writes `LIMIT N` inside subquery without correlating to outer department ID. |
| 10 | "Query running slow in production, what do you do?" | Proposes hardware upgrades or database restart instead of systematic query diagnosis. |
| 11 | "Write query with `NOT IN` subquery" | Doesn't check if subquery returns `NULL`, causing entire query to fail silently returning 0 rows. |
| 12 | "Paginate 1,000,000 records" | Uses `LIMIT 20 OFFSET 999980`, causing full table scan of 1,000,000 rows on disk for every page fetch. |
| 13 | "Calculate monthly active user retention" | Does not handle missing months or duplicate user activity records within the same month. |
| 14 | "Filter data by date range: `WHERE YEAR(date_col) = 2026`" | Wraps indexed date column in function, breaking SARGability and causing sequential table scan. |
| 15 | "What is transaction isolation level?" | Confuses Read Committed with Repeatable Read anomalies. |
| 16 | "Update balance in bank account" | Omits transaction wrapper (`BEGIN...COMMIT`), leaving database susceptible to partial writes. |
| 17 | "How B-Tree index speeds up lookups" | Believes index copies the entire table into RAM memory. |
| 18 | "Count unique users per category" | Confuses `COUNT(DISTINCT user_id)` with `COUNT(user_id)`. |
| 19 | "Difference between `UNION` and `UNION ALL`" | Uses `UNION` by default everywhere without realizing it imposes expensive sort deduplication. |
| 20 | "Design multi-tenant database" | Fails to consider data isolation leaks between tenants or tenant-specific schema migrations. |
| 21–50 | (Additional failure modes: ignoring `NULL` in `AVG()`, using `SELECT *` in production code, neglecting index write amplification on high-TPS tables, failing to handle ties in `ROW_NUMBER()`, etc.) |

---

## 6. Top 50 Questions That Differentiate Top Candidates

Senior, Staff, and Principal candidates stand out by demonstrating deep database internal knowledge and architectural trade-offs:

1. How to implement a reliable multi-worker job queue in SQL using `FOR UPDATE SKIP LOCKED`?
2. Design a GDPR-compliant soft delete and data retention purge system using table partitioning.
3. How do B-Tree page splits occur, how do they degrade read performance, and how does `FILLFACTOR` mitigate them?
4. Write a query to find the shortest path between two nodes in a directed graph stored in SQL.
5. Contrast storage layouts: Row-oriented (Postgres/MySQL) vs Column-oriented (ClickHouse/Snowflake) vs LSM-Trees (Cassandra/RocksDB).
6. Explain Multi-Version Concurrency Control (MVCC) implementation details in Postgres (`xmin`, `xmax`, vacuuming) vs InnoDB (`undo log`).
7. How does Snapshot Isolation work and how does Write Skew anomaly occur? Give a concrete code example.
8. Compare Keyset (Cursor-based) pagination vs Offset-based pagination in terms of I/O performance and data stability.
9. How to design slowly changing dimensions (SCD Type 2) with temporal validity constraints in SQL?
10. Diagnose an execution plan where estimated rows = 1 but actual rows = 1,000,000 (parameter sniffing / stale stats).
11. How to implement zero-downtime database schema migration for a 1TB table in a high-traffic production system?
12. Discuss trade-offs between Schema-per-tenant vs Row-Level Security (RLS) for multi-tenant architectures.
13. How does Next-Key Locking in MySQL InnoDB prevent phantom reads under `REPEATABLE READ`?
14. Write a query that computes Exponential Moving Average (EMA) declaratively in SQL.
15. How to handle late-arriving events in streaming SQL engines (Apache Flink / ksqlDB) using watermarks?
16. Compare Hash Join vs Nested Loop Join vs Merge Join execution algorithms and memory requirements.
17. How to build a consistent hashing distribution scheme for horizontal database sharding in pure SQL?
18. Explain how Google Spanner / CockroachDB achieves external consistency across global datacenters (TrueTime / Raft).
19. How to optimize GIN / GiST indexes for JSONB and geospatial queries in PostgreSQL?
20. Explain the impact of connection pool exhaustion and how PgBouncer in transaction mode mitigates it.
21–50. (Advanced topics: Index fragmentation repair, buffer pool eviction policies, WAL logging mechanics, CDC pipeline design, distributed transaction two-phase commit (2PC) protocol failure modes, etc.)
