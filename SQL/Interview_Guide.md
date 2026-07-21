# 🎯 SQL Interview Guide (Beginner to Advanced)

This guide provides a structured breakdown of expectations across career levels for SQL interviews.

---

## 🟢 1. Beginner Level (Freshers / SDE-1 / Junior Roles)

### Core Topics
- Basic DDL (`CREATE`, `ALTER`, `DROP`, `TRUNCATE`) and DML (`SELECT`, `INSERT`, `UPDATE`, `DELETE`)
- Single-table filtering (`WHERE`, `LIKE`, `IN`, `BETWEEN`, `AND`, `OR`, `NOT`)
- Sorting and limiting (`ORDER BY`, `LIMIT`, `OFFSET`)
- Aggregate functions (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`)
- Grouping data (`GROUP BY`, `HAVING`)
- Basic joins (`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`)

### Key Concepts
- **Execution Order of SQL Queries**:
  1. `FROM` / `JOIN`
  2. `WHERE`
  3. `GROUP BY`
  4. `HAVING`
  5. `SELECT`
  6. `DISTINCT`
  7. `ORDER BY`
  8. `LIMIT` / `OFFSET`
- **NULL Value Behavior**: NULL represents an unknown value. `NULL = NULL` evaluates to `UNKNOWN` (not True). Use `IS NULL` or `IS NOT NULL`.
- **WHERE vs HAVING**: `WHERE` filters rows *before* aggregation. `HAVING` filters groups *after* aggregation.

### Common Beginner Mistakes
- Using `WHERE` to filter on aggregate functions (e.g., `WHERE COUNT(order_id) > 5` - invalid SQL).
- Assuming `COUNT(column_name)` counts NULL values (it ignores NULLs; only `COUNT(*)` counts all rows).
- Using `NOT IN (subquery)` when the subquery can return `NULL` values (causes the entire query to evaluate to empty set).
- Confusing `UNION` (removes duplicates with sort/hash overhead) and `UNION ALL` (retains all rows, faster).

### Expected Interview Questions
1. Write a query to find the top 5 highest-paid employees in a company.
2. Explain the difference between `DELETE`, `TRUNCATE`, and `DROP`.
3. Find all customers who have placed at least 3 orders.
4. How do you select rows containing `NULL` values in a `status` column?

---

## 🟡 2. Intermediate Level (SDE-2 / Data Engineers / Mid-Level)

### Frequently Tested Concepts
- **Window Functions**: `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `LAG()`, `LEAD()`, `NTILE()`, and frame clauses (`ROWS BETWEEN ...`).
- **Common Table Expressions (CTEs)** vs Subqueries vs Temporary Tables.
- **Self Joins & Anti-Joins**: Identifying unmatched records using `LEFT JOIN WHERE right_table.pk IS NULL` or `NOT EXISTS`.
- **Advanced Grouping**: `GROUPING SETS`, `ROLLUP`, `CUBE`, conditional aggregations (`CASE WHEN` inside `SUM` or `COUNT`).
- **Subquery Optimization**: Correlated subqueries vs `LATERAL` joins / window functions.
- **Indexes Basics**: B-Tree structure, Clustered vs Non-Clustered indexes.

### Coding Expectations
- Write clean, formatted, production-ready SQL using modern ANSI standards.
- Prefer CTEs for multi-step data transformations to maintain code readability.
- Handle tie-breaking deterministically in `ORDER BY` clauses when using window functions.
- Demonstrate explicit handling of duplicate rows, edge cases, and empty sets.

### Real Interview Expectations
- Interviewers will evaluate whether you handle edge cases automatically (e.g., ties in salaries, missing dates, zero totals).
- You are expected to state your target database engine (e.g., PostgreSQL, MySQL, SQL Server) as minor syntax details differ.
- You must explain the time/space complexity of window functions and joins conceptually.

### Follow-up Questions to Prepare For
- *"How does your query handle two employees with the exact same salary?"*
- *"Why did you use `DENSE_RANK()` instead of `RANK()` here?"*
- *"Can this correlated subquery be rewritten using a Join or Window Function for better performance?"*
- *"What index would you create on this table to speed up your query?"*

---

## 🔴 3. Advanced Level (Senior / Staff / Principal / Architect)

### Production Concepts
- **Query Optimization & Execution Plans**: Reading `EXPLAIN ANALYZE` output. Identifying Seq Scans, Index Scans, Index Only Scans, Nested Loop Joins, Hash Joins, and Sort Merge Joins.
- **SARGability**: Writing Search Argument Able queries that enable index lookup rather than full table scans. (e.g., avoiding `WHERE YEAR(created_at) = 2026`).
- **Database Indexing Strategies**:
  - Composite indexes and column ordering (Leftmost Prefix Rule).
  - Covering indexes (`INCLUDE` clause in SQL Server/Postgres).
  - Partial/Filtered indexes (`CREATE INDEX ... WHERE active = true`).
  - Expression/Functional indexes (`CREATE INDEX ... ON users(LOWER(email))`).
- **Transactions & Concurrency Control**:
  - ACID properties in distributed systems.
  - Multi-Version Concurrency Control (MVCC) mechanics in PostgreSQL / MySQL InnoDB.
  - Transaction isolation levels (`READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`) and anomalies (Dirty Read, Non-Repeatable Read, Phantom Read, Serialization Anomaly, Write Skew).
  - Explicit locking strategies (`SELECT FOR UPDATE`, `SELECT FOR SHARE`, `SKIP LOCKED`, `NOWAIT`).

### Architecture & System Discussions
- **Database Partitioning vs Sharding**:
  - Range, List, Hash partitioning.
  - Horizontal sharding strategies, shard key selection, hash ring rebalancing, cross-shard queries.
- **Read/Write Splitting & Replication Lag**: Eventual consistency challenges, handling stale reads.
- **Distributed SQL & Storage Engines**: LSM Trees (Cassandra/RocksDB) vs B+ Trees (InnoDB/Postgres), Distributed consensus (Raft/Spanner).

### Performance & Edge Cases
- Eliminating Index Fragmentation, adjusting `FILLFACTOR` / Page Splits.
- Deep pagination optimization (`OFFSET` performance degradation vs Keyset / Cursor-based pagination).
- Handling large batch `DELETE` or `UPDATE` operations without locking production tables (batching in chunks).

### Senior Interview Expectations
- You must drive the architectural trade-off discussion (Storage vs Memory vs Read Latency vs Write Latency).
- You must recognize potential lock contention, deadlocks, and transaction isolation pitfalls.
- You should proactively suggest database-level vs application-level solutions for complex requirements.
