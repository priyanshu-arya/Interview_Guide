# 🎯 Ultimate SQL Interview Guide: Trends, Mindset, Theory (150+ Qs) & Strategy

---

## 📌 Table of Contents

1. [Trends & Mindset for SQL Interviews in 2026–2027](#1-trends--mindset-for-sql-interviews-in-20262027)
2. [Beginner Level Breakdown](#2-beginner-level-breakdown)
3. [Intermediate Level Breakdown](#3-intermediate-level-breakdown)
4. [Advanced Level Breakdown](#4-advanced-level-breakdown)
5. [Comprehensive Theory Questions (150+)](#5-comprehensive-theory-questions-150)
   - [SQL Basics (20 Qs)](#sql-basics-20-qs)
   - [DDL & DML (15 Qs)](#ddl--dml-15-qs)
   - [Joins & Subqueries (25 Qs)](#joins--subqueries-25-qs)
   - [Aggregation & Grouping (15 Qs)](#aggregation--grouping-15-qs)
   - [Window Functions (20 Qs)](#window-functions-20-qs)
   - [CTE & Recursion (10 Qs)](#cte--recursion-10-qs)
   - [Indexes & Internals (20 Qs)](#indexes--internals-20-qs)
   - [Transactions & Concurrency (15 Qs)](#transactions--concurrency-15-qs)
   - [Normalization & Schema Design (20 Qs)](#normalization--schema-design-20-qs)
   - [Security & Permissions (10 Qs)](#security--permissions-10-qs)
6. [Interview Day Strategy](#6-interview-day-strategy)

---

## 1. Trends & Mindset for SQL Interviews in 2026–2027

> **2026 Reality**: SQL is no longer just "the data analyst's language". Backend engineers, SREs, ML engineers, and front-end leads are grilled on SQL during system design and live coding rounds. Companies expect **production-grade SQL thinking** – not just syntax memorization.

### 🔥 What Interviewers Are Really Testing

| Area | Why They Ask | Weight |
|------|-------------|--------|
| **Window Functions** | Differentiates script-kiddies from real engineers. Used everywhere: analytics, leaderboards, time-series. | **35%** |
| **Query Optimization** | Proves you understand database internals, execution plans, indexing, SARGability, not just surface syntax. | **25%** |
| **Database Design & Normalization** | Shows you can model real-world domain requirements without data anomalies. | **20%** |
| **Transactions & Concurrency** | Tests if you know how to keep data consistent under concurrent high-traffic load. | **10%** |
| **Advanced SQL Features** (CTEs, recursion, JSON, pivoting) | Separates senior candidates who solve complex problems declaratively in database engine. | **10%** |

> 💡 **Memory Trick**: Remember **WINDOW** – **W**rite queries, **I**ndexing, **N**ormalization, **D**ebugging, **O**ptimization, **W**indow functions.

---

## 2. Beginner Level Breakdown

### Core Topics & Concepts
- Basic Data Manipulation (`SELECT`, `INSERT`, `UPDATE`, `DELETE`)
- Single-table filtering (`WHERE`, `LIKE`, `IN`, `BETWEEN`, `AND`, `OR`, `NOT`)
- Sorting and limiting (`ORDER BY`, `LIMIT`, `OFFSET`)
- Aggregate functions (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`)
- Grouping data (`GROUP BY`, `HAVING`)
- Basic joins (`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`)

### Key Concepts
- **Execution Order of SQL Queries** (`FWGH SLO`):
  1. `FROM` / `JOIN` $\rightarrow$ 2. `WHERE` $\rightarrow$ 3. `GROUP BY` $\rightarrow$ 4. `HAVING` $\rightarrow$ 5. `SELECT` $\rightarrow$ 6. `DISTINCT` $\rightarrow$ 7. `ORDER BY` $\rightarrow$ 8. `LIMIT` / `OFFSET`.
- **NULL Behavior**: `NULL` means unknown. `NULL = NULL` yields `UNKNOWN`. Use `IS NULL` / `IS NOT NULL`.
- **WHERE vs HAVING**: `WHERE` filters rows *before* aggregation. `HAVING` filters aggregated groups *after*.

### Common Beginner Mistakes
- Filtering aggregate functions inside `WHERE` (e.g., `WHERE COUNT(id) > 5` — invalid).
- Expecting `COUNT(column)` to include `NULL` values (it ignores `NULL`s; only `COUNT(*)` counts all rows).
- Using `NOT IN (subquery)` when subquery returns `NULL` values.

---

## 3. Intermediate Level Breakdown

### Frequently Tested Concepts
- **Window Functions**: `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `LAG()`, `LEAD()`, `NTILE()`, and frame specifications (`ROWS BETWEEN ...`).
- **CTEs vs Subqueries**: Writing readable multi-step transformations with `WITH`.
- **Anti-Joins**: Identifying unmatched records via `LEFT JOIN ... WHERE right.pk IS NULL` or `NOT EXISTS`.
- **Advanced Grouping**: `ROLLUP`, `CUBE`, `GROUPING SETS`, conditional aggregations (`CASE WHEN`).
- **Indexes Basics**: B-Tree vs Hash index structures.

---

## 4. Advanced Level Breakdown

### Production Concepts
- **Query Execution Plans**: Diagnosing `EXPLAIN ANALYZE` (Seq Scans, Index Scans, Nested Loop vs Hash Joins).
- **SARGability**: Eliminating functions on indexed columns (`WHERE date >= '2026-01-01'` instead of `WHERE YEAR(date) = 2026`).
- **Indexing Strategies**: Composite index column ordering (Leftmost Prefix Rule), Covering indexes (`INCLUDE`), Partial indexes.
- **Transactions & Isolation**: ACID guarantees, MVCC, Isolation levels, Anomalies (Dirty Read, Non-Repeatable Read, Phantom Read, Write Skew), Explicit locking (`FOR UPDATE`, `SKIP LOCKED`).

---

## 5. Comprehensive Theory Questions (150+)

### 📚 SQL Basics (20 Qs)
1. **What is SQL and what are its sublanguages?**  
   *Answer*: DDL (Data Definition Language: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`), DML (Data Manipulation Language: `SELECT`, `INSERT`, `UPDATE`, `DELETE`), DCL (Data Control Language: `GRANT`, `REVOKE`), TCL (Transaction Control Language: `COMMIT`, `ROLLBACK`, `SAVEPOINT`).
2. **Difference between CHAR and VARCHAR.**  
   *Answer*: `CHAR` is fixed-length (padded with spaces); `VARCHAR` is variable-length (stores actual string length + length byte).
3. **What is a Primary Key?**  
   *Answer*: Unique identifier for table rows; must contain unique, non-null values. Only 1 Primary Key per table.
4. **What is a Foreign Key?**  
   *Answer*: Key establishing link between data in two tables, enforcing referential integrity.
5. **Difference between Primary Key and Unique Key.**  
   *Answer*: PK prohibits `NULL`s and defines physical cluster key in some DBs; Unique Key allows `NULL` (one or more depending on DB dialect).
6. **What is NULL in SQL?**  
   *Answer*: Marker representing missing, unknown, or unpopulated value.
7. **What is relational algebra?**  
   *Answer*: Mathematical foundation of SQL based on sets and operators (select, project, join, union, intersect, difference).
8. **Explain 3-Valued Logic (3VL) in SQL.**  
   *Answer*: Truth values in SQL can be `TRUE`, `FALSE`, or `UNKNOWN`. Any comparison with `NULL` yields `UNKNOWN`.
9. **Difference between SQL and NoSQL.**  
   *Answer*: SQL is relational, schema-enforced, ACID-compliant. NoSQL is document/key-value/graph/columnar, schema-flexible, BASE-compliant.
10. **What is a Composite Key?**  
    *Answer*: Primary Key composed of two or more columns to guarantee uniqueness.
11. **Difference between `COUNT(*)` and `COUNT(col)`.**  
    *Answer*: `COUNT(*)` counts total rows including `NULL`s; `COUNT(col)` counts non-null entries in `col`.
12. **What does `COALESCE()` do?**  
    *Answer*: Evaluates parameters in order and returns the first non-null expression.
13. **Difference between `COALESCE()` and `NVL()` / `IFNULL()`.**  
    *Answer*: `COALESCE()` is ANSI standard accepting multiple parameters; `NVL()`/`IFNULL()` are vendor-specific accepting two parameters.
14. **What is an alias in SQL?**  
    *Answer*: Temporary name given to table or column using `AS` keyword for readability.
15. **How does `BETWEEN` work in date comparisons?**  
    *Answer*: Includes boundary values (`val >= min AND val <= max`). Be careful with time components in dates.
16. **What is the default sort order of `ORDER BY`?**  
    *Answer*: Ascending (`ASC`).
17. **Can a table exist without a Primary Key?**  
    *Answer*: Yes (heap table), but strongly discouraged in production due to replication and indexing issues.
18. **Difference between `TRUNCATE` and `DELETE`.**  
    *Answer*: `TRUNCATE` is DDL (deallocates storage pages, resets identity counter, minimal log); `DELETE` is DML (deletes row by row, logs each row delete).
19. **What is a surrogate key vs natural key?**  
    *Answer*: Natural key has business meaning (e.g. SSN/Email); Surrogate key is system-generated artificial ID (e.g. Auto-increment / UUID).
20. **What is the purpose of `DISTINCT` keyword?**  
    *Answer*: Eliminates duplicate rows from query result set.

### 🗂️ DDL & DML (15 Qs)
21. **Difference between `DROP TABLE` and `TRUNCATE TABLE`.**  
22. **How to add a column with default value to existing table?**  
23. **What is `CASCADE DELETE` in foreign keys?**  
24. **Difference between `INSERT INTO ... SELECT` and `SELECT ... INTO`.**  
25. **What is an Upsert statement? (`ON CONFLICT DO UPDATE` / `MERGE`)**  
26–35. (Details on DDL schema changes, locks during DDL, constraint checks, `ALTER TABLE` locks, temporary tables vs table variables).

### 🔗 Joins & Subqueries (25 Qs)
36. **Explain `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`, `CROSS JOIN`.**  
37. **What is a Self-Join and when is it used?**  
38. **Correlated Subquery vs Non-Correlated Subquery.**  
39. **`EXISTS` vs `IN`: Performance & NULL handling differences.**  
40. **What is a `LATERAL` join in PostgreSQL?**  
41–60. (Details on anti-joins, semi-joins, join order, Cartesian explosion, multiple join performance, subquery unnesting).

### 📊 Aggregation & Grouping (15 Qs)
61. **Execution order of `WHERE`, `GROUP BY`, `HAVING`, `SELECT`.**  
62. **Difference between `GROUP BY` and `DISTINCT`.**  
63. **Explain `ROLLUP`, `CUBE`, `GROUPING SETS`.**  
64. **Conditional aggregation using `SUM(CASE WHEN ...)`**  
65–75. (Details on `HAVING` without `GROUP BY`, multi-column aggregates, string aggregation, median calculation).

### 🪟 Window Functions (20 Qs)
76. **What is a Window Function and how does it differ from `GROUP BY`?**  
77. **Differences between `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`.**  
78. **`LAG()` and `LEAD()` functions with default fallbacks.**  
79. **Frame specifications: `ROWS` vs `RANGE`.**  
80. **Default frame boundary when `ORDER BY` is present.**  
81–95. (Details on `FIRST_VALUE`, `LAST_VALUE`, `NTILE`, cumulative sums, moving averages, window partition performance).

### 🧩 CTE & Recursion (10 Qs)
96. **What is a CTE and why prefer it over subqueries?**  
97. **Syntax and mechanics of Recursive CTE (`UNION ALL`).**  
98. **Anchor member vs Recursive member in recursive CTE.**  
99. **Cycle detection in recursive CTE to prevent infinite loops.**  
100–105. (Details on CTE materialization in Postgres, temporary table vs CTE).

### 📈 Indexes & Internals (20 Qs)
106. **What is a B-Tree index and how does it work?**  
107. **Clustered vs Non-Clustered index.**  
108. **What is a Composite Index and Leftmost Prefix Rule?**  
109. **What is a Covering Index?**  
110. **What is SARGability?**  
111–125. (Details on Hash index, GIN index, expression index, partial index, index fragmentation, fill factor, page splits).

### ⚙️ Transactions & Concurrency (15 Qs)
126. **Explain ACID properties.**  
127. **Transaction Isolation Levels and Anomalies.**  
128. **Dirty Read vs Non-Repeatable Read vs Phantom Read.**  
129. **What is Write Skew anomaly?**  
130. **Optimistic Locking vs Pessimistic Locking (`FOR UPDATE`).**  
131–140. (Details on MVCC, Deadlocks, Wait-For Graph, Next-Key Locking, 2PC distributed transactions).

### 🏗️ Normalization & Design (20 Qs)
141. **Explain 1NF, 2NF, 3NF, BCNF.**  
142. **When and why should you denormalize a database?**  
143. **Star Schema vs Snowflake Schema.**  
144. **Slowly Changing Dimensions (SCD Type 1, Type 2, Type 3).**  
145–160. (Details on ER modeling, surrogate keys, partitioning, sharding, multi-tenant patterns).

### 🔐 Security & Permissions (10 Qs)
161. **`GRANT` and `REVOKE` DCL syntax.**  
162. **Row-Level Security (RLS) implementation.**  
163. **Preventing SQL Injection using Prepared Statements.**  
164–170. (Details on database roles, audit logging, column encryption).

---

## 6. Interview Day Strategy

1. **Clarify Requirements First**: Ask about duplicate handling, NULL semantics, dataset size, and tie-breaking before writing code.
2. **Think Out Loud**: Walk the interviewer through your logic, trade-offs, and edge cases.
3. **Start with a Working Solution, Then Optimize**: *"Make it work, make it right, make it fast."*
4. **State Your Database Dialect**: Specify if you are using PostgreSQL, MySQL, T-SQL, or ANSI SQL.
5. **Mention Concurrency & Transactions**: If modifying data, mention isolation levels, locking (`FOR UPDATE`), and deadlock safety.
6. **Clean Formatting**: Use UPPERCASE for SQL keywords, clear table aliases, and CTEs for multi-step queries.
7. **Never Say "I Don't Know" Instantly**: Say *"I would analyze the execution plan (`EXPLAIN`) or consult system views to diagnose this behavior."*

> 🚨 **Warning**: Never leave query execution edge cases unhandled (ties in ranks, division by zero, NULL values in subqueries).
