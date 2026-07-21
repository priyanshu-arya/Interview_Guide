# 🧪 DBMS Practice Questions, Coding, MCQs & Production Bugs

Hands-on problem sets organized strictly into 12 comprehensive practice modules.

---

## 1. Concept Questions

### Q1.1: Why does B+ Tree leaf node linking eliminate index re-traversal in range queries?
- **Difficulty**: Medium
- **Hint**: Think about tree traversal vs linear linked-list iteration.
- **Expected Approach**: Explain that in standard search trees, finding keys between $A$ and $B$ requires traversing parent-child nodes repeatedly ($O(K \log N)$). In B+ Trees, after an initial $O(\log N)$ search to key $A$'s leaf, the query engine follows leaf pointers sequentially until key $B$, performing $O(K)$ sequential reads.
- **Common Mistakes**: Stating that B-Trees also have linked leaf nodes (only B+ Trees have linked leaves).

---

## 2. Top 50 Must-Know Questions

1. **ACID Properties**: Define Atomicity, Consistency, Isolation, Durability.
2. **CAP Theorem**: Consistency, Availability, Partition Tolerance trade-offs.
3. **BASE Model**: Basically Available, Soft State, Eventual Consistency.
4. **B-Tree vs LSM-Tree**: Read-optimized in-place updates vs write-optimized append-only SSTables.
5. **Normalization**: 1NF, 2NF, 3NF, BCNF definitions and anomaly prevention.
6. **Isolation Levels**: Read Uncommitted, Read Committed, Repeatable Read, Serializable.
7. **Anomalies**: Dirty Read, Non-Repeatable Read, Phantom Read, Write Skew.
8. **Index Types**: Clustered, Non-Clustered, Composite, Covering, Partial, Expression, GIN, GiST, BRIN, Bitmap.
9. **SQL Joins**: Inner, Left, Right, Full Outer, Cross, Self-join mechanics.
10. **SQL Aggregations & Window Functions**: `GROUP BY`, `HAVING`, `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `LAG()`, `LEAD()`.
11. **WAL (Write-Ahead Logging)**: Redo logs, append-only disk flushing before data page writes.
12. **Checkpointing**: Fuzzy checkpointing, flushing dirty pages, reducing recovery time.
13. **ARIES Recovery**: Analysis, Redo, Undo phases.
14. **Locks**: Shared (S), Exclusive (X), Intent (IS, IX), Gap Locks, Next-Key Locks.
15. **Deadlocks**: Wait-For Graphs, detection algorithms, timeout, lock ordering.
16. **MVCC**: Tuple versioning, `xmin`/`xmax`, snapshot isolation, non-blocking reads.
17. **VACUUM**: Postgres tuple reclamation, freezing transaction IDs, preventing wraparound.
18. **Query Optimizer (CBO)**: Cost estimation, statistics, histograms, selectivity.
19. **Join Algorithms**: Nested Loop Join, Hash Join, Merge Join.
20. **SARGability**: Writing queries that leverage B+ Tree range scans (`WHERE col >= val`).
21. **Partitioning**: Range, List, Hash, Composite partitioning.
22. **Sharding**: Shard key selection, consistent hashing, horizontal scaling.
23. **Replication**: Primary-Replica, Multi-Master, Synchronous vs Asynchronous.
24. **Consensus Protocols**: Raft, Paxos, leader election, log replication.
25. **2-Phase Commit (2PC)**: Prepare phase, Commit phase, coordinator failure risks.
26. **Saga Pattern**: Orchestrated vs Choreographed saga, compensating transactions.
27. **Change Data Capture (CDC)**: Binlog/WAL streaming via Debezium and Kafka.
28. **Connection Pooling**: Managing DB connections, PgBouncer, HikariCP.
29. **SQL Injection Prevention**: Parameterized queries, prepared statements.
30. **Encryption**: Encryption at rest (TDE) and encryption in transit (TLS/SSL).
31. **Backup Strategies**: Full, Differential, Incremental, Point-In-Time Recovery (PITR).
32. **OLTP vs OLAP**: Transactional row-oriented vs analytical columnar storage.
33. **Columnar Engines**: Parquet, ClickHouse, Snowflake, SIMD vectorization.
34. **Vector DBs**: HNSW, IVF-PQ indexing for AI similarity search.
35. **Keyset Pagination**: Replacing `OFFSET` with seek predicates for $O(1)$ page reads.
36. **Materialized Views**: Physical query caching, refresh strategies (`REFRESH MATERIALIZED VIEW`).
37. **Advisory Locks**: Application-level locks managed via database session.
38. **Star vs Snowflake Schema**: Fact and dimension table modeling in data warehouses.
39. **Slowly Changing Dimensions (SCD)**: Type 1 (Overwrite), Type 2 (History row), Type 3 (Previous column).
40. **Hot Row Contention**: Slotting, batching, and Redis atomic counters.
41. **Outbox Pattern**: Transactional message publishing with relational databases.
42. **Soft Delete**: `is_deleted` column vs hard deletion tradeoffs.
43. **TOAST**: PostgreSQL out-of-line storage for oversized columns.
44. **Fill Factor**: Reserving index page capacity to prevent page splits.
45. **Halloween Problem**: Updating indexed columns during table scan causing infinite loops.
46. **Bloom Filters**: Probabilistic set membership avoiding SSTable lookups.
47. **TrueTime API**: Google Spanner hardware clock bounds ($\epsilon$) for global consistency.
48. **Bi-Temporal Data**: Valid time vs Transaction time history tracking.
49. **Serializable Snapshot Isolation (SSI)**: Conflict graph tracking for serializability without locking.
50. **Zero-Downtime Schema Migration**: Shadow table pattern, dual-writing, background backfill, atomic swap.

---

## 3. Top 50 Most Tricky Questions

| # | Tricky Question | Why Tricky & Fix |
|---|----------------|------------------|
| 1 | `SELECT COUNT(*)` vs `SELECT COUNT(col)` | `COUNT(*)` counts all rows; `COUNT(col)` excludes NULLs. |
| 2 | Can a Foreign Key be NULL? | Yes; NULL FK means unassociated/optional parent relation. |
| 3 | If we `DELETE` and `ROLLBACK`, do rows return? | Yes; Undo Log replays original row state. |
| 4 | What happens if a trigger fails? | Aborts the statement and rolls back the enclosing transaction. |
| 5 | Can a View have `ORDER BY`? | Ignored in standard views unless combined with `LIMIT`/`TOP`. |
| 6 | Difference between `UNION` and `UNION ALL` speed? | `UNION ALL` is faster because `UNION` runs expensive distinct sort/hash. |
| 7 | Result of `WHERE 1 = NULL`? | `UNKNOWN` (evaluates to FALSE in WHERE filter). |
| 8 | Rollback a transaction after `COMMIT`? | Impossible; `COMMIT` is final and durable. |
| 9 | Can a table have multiple Primary Keys? | No; only 1 Primary Key definition (though can be composite). |
| 10 | Max number of Clustered Indexes per table? | Exactly 1 (defines physical disk storage order). |
| 11 | Why `WHERE id NOT IN (SELECT id FROM t2)` returns 0 rows if `t2` has NULL? | NULL in `NOT IN` list makes condition `UNKNOWN` for all rows. Use `NOT EXISTS`. |
| 12 | What happens on `TRUNCATE` table with FK references? | Errors out unless `CASCADE` or FKs disabled. |
| 13 | Can an index be created on a View? | Yes, if the view is Materialized / Schema-bound. |
| 14 | Database reaction to `CHECK` constraint violation? | Throws runtime error and aborts transaction. |
| 15 | Is `LIMIT` without `ORDER BY` safe? | No; returned row order is non-deterministic. |
| 16 | Does `WHERE` filter order in `LEFT JOIN` affect NULLs? | Filtering right table column in `WHERE` converts `LEFT JOIN` into `INNER JOIN`. Put filter in `ON` instead. |
| 17 | Self-referencing Foreign Key? | Yes; used for hierarchical data (e.g., `manager_id -> emp_id`). |
| 18 | Deadlock priority setting? | Databases allow setting `DEADLOCK_PRIORITY` to pick the victim transaction. |
| 19 | Uncommitted transaction left open in procedure? | Holds locks indefinitely, causing connection pool & lock blocking. |
| 20 | Auto-increment IDs on `ROLLBACK`? | IDs are NOT rolled back; creates permanent gaps in identity sequence. |
| 21 | Unique constraint with NULL values? | Standard SQL allows multiple NULLs in unique column (Postgres allows multiple; SQL Server treats NULL as single value). |
| 22 | Why an index is ignored despite column being indexed? | Low selectivity, function wrapper (`YEAR(date)`), type mismatch, or outdated stats. |
| 23 | Halloween Problem in SQL? | Updating indexed column causes updated row to move forward and be re-scanned. |
| 24 | Is `ALTER TABLE ADD COLUMN` transactional? | In Postgres/transactional DDL systems, yes; can be rolled back. |
| 25 | Why is `SELECT *` bad in production? | Breaks covering index scans, wastes I/O, fails on schema changes. |
| 26–50 | *(Detailed tricky edge cases: NULL ordering, implicit type casting, CTE materialization hints, advisory lock scopes)* | Master 3-valued SQL logic and execution plans. |

---

## 4. Coding Questions (100+)

### Category A: Easy (30 Questions)
1. **Create Table**: Write DDL to create `students` with PK, FK, NOT NULL, and CHECK constraints.
2. **Basic Filtering**: Select active customers registered in 2026 ordered by signup date.
3. **Group By Aggregation**: Find total revenue per product category.
... *(30 Easy SQL problems)*

### Category B: Medium (40 Questions)

#### Problem: Keyset Pagination Implementation
- **Problem Statement**: Implement high-performance pagination query for loading 20 log records per page on a table with 50,000,000 rows.
- **Hint**: Avoid `OFFSET`. Filter on composite key `(created_at, log_id)`.
- **Solution**:
```sql
SELECT log_id, event_name, created_at
FROM system_logs
WHERE (created_at, log_id) < (:last_created_at, :last_log_id)
ORDER BY created_at DESC, log_id DESC
LIMIT 20;
```

#### Problem: De-duplicating Rows Keeping Latest Record
- **Problem Statement**: Given table `user_events(event_id, user_id, payload, created_at)`, write a query to delete all duplicate user events keeping only the latest event per `user_id`.
- **Solution**:
```sql
WITH RankedEvents AS (
    SELECT event_id,
           ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at DESC) as rnk
    FROM user_events
)
DELETE FROM user_events 
WHERE event_id IN (
    SELECT event_id FROM RankedEvents WHERE rnk > 1
);
```

... *(40 Medium SQL problems including running totals, pivots, CTEs)*

### Category C: Hard (20 Questions)

#### Problem: Finding Gaps and Islands in Sequential Data
- **Problem Statement**: Given user login activity table `user_logins(user_id, login_date)`, find consecutive login streaks for each user.
- **Solution**:
```sql
WITH DateGroups AS (
    SELECT user_id, login_date,
           login_date - (ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date)) * INTERVAL '1 day' AS grp
    FROM user_logins
)
SELECT user_id, MIN(login_date) as streak_start, MAX(login_date) as streak_end, COUNT(*) as streak_length
FROM DateGroups
GROUP BY user_id, grp
HAVING COUNT(*) >= 3;
```

... *(20 Hard SQL problems including recursive shortest path, graph cycles)*

### Category D: Very Hard (10 Questions)
1. **SCD Type 2 Merge Implementation**: Write `MERGE` SQL to update effective end dates and insert new version rows.
2. **PL/pgSQL Dynamic Query Monitor**: Write function to detect and kill queries running $> 30$ seconds.

---

## 5. MCQs (100+)

<details>
<summary><b>Sample MCQs (Expand to view answers)</b></summary>

**1. Which normal form eliminates transitive dependencies?**  
A) 1NF  
B) 2NF  
C) 3NF  
D) BCNF  
<details><summary><b>Answer</b></summary><b>C) 3NF</b>. 3NF removes non-key dependencies on other non-key attributes ($A \rightarrow B \rightarrow C$).</details>

**2. What is the default transaction isolation level in PostgreSQL?**  
A) Read Uncommitted  
B) Read Committed  
C) Repeatable Read  
D) Serializable  
<details><summary><b>Answer</b></summary><b>B) Read Committed</b>.</details>

**3. Which join algorithm is optimal when both inputs are large and unsorted, joining on equality?**  
A) Nested Loop Join  
B) Hash Join  
C) Merge Join  
D) Cross Join  
<details><summary><b>Answer</b></summary><b>B) Hash Join</b>. Engine builds an in-memory hash table on smaller input and probes with larger input.</details>

**4. What does Write-Ahead Logging (WAL) mandate?**  
A) Data pages are written to disk before log records.  
B) Log records are written to non-volatile disk before data pages hit disk.  
C) Logs are written only during system crash recovery.  
D) Writes bypass RAM and execute synchronously on disk.  
<details><summary><b>Answer</b></summary><b>B</b>. Log records MUST hit disk before modified dirty data pages.</details>

**5. What evaluates `SELECT COUNT(*) FROM table` when all rows have NULL in a specific column `col`?**  
A) `COUNT(*)` returns total row count; `COUNT(col)` returns 0.  
B) Both return 0.  
C) Both return total row count.  
D) Throws runtime error.  
<details><summary><b>Answer</b></summary><b>A</b>. `COUNT(*)` counts tuples; `COUNT(col)` ignores NULLs.</details>

... *(100 MCQs covering all DBMS domains)*

</details>

---

## 6. Scenario Questions (75+)

1. **Designing Social Feed System**: Discuss fan-out on write vs read, Redis caching, denormalization, push vs pull architecture.
2. **Preventing Double Spending in Banking API**: Enforce strict serializability or pessimistic row locking (`FOR UPDATE`) + idempotency table.
3. **E-Commerce Inventory Contention**: Partition inventory across 10 slot rows or use atomic Redis decr + Kafka message queue.

---

## 7. Production Problems (50+)

1. **Replication Lag Spikes**: Caused by massive batch transactions on primary node. Fix: Break batch operations into smaller chunks.
2. **Disk Full due to WAL Accumulation**: Caused by unconsumed replication slots or failing log archive script. Fix: Drop stale replication slot, restore archive pipeline.
3. **Connection Pool Exhaustion**: Caused by long-running idle transactions. Fix: Set `idle_in_transaction_session_timeout`.

---

## 8. Debugging Problems (50+)

1. **Faulty Query**: `SELECT * FROM orders WHERE YEAR(order_date) = 2026;`  
   - **Fix**: `WHERE order_date >= '2026-01-01' AND order_date < '2027-01-01'`.
2. **Deadlock Exception**: Lock ordering conflict across transactions.  
   - **Fix**: Sort target primary key IDs in ascending order before locking.

---

## 9. Output Prediction Questions (50+)

Predict query outputs involving `NULL` logic, `LEFT JOIN` filters, and window partitioning.

---

## 10. Best Practices Questions (50+)

1. Always parameterize queries to eliminate SQL Injection vulnerabilities.
2. Index foreign key columns to speed up JOINs and avoid full table locks on parent deletions.
3. Avoid long-running transactions to prevent undo log/VACUUM bloat.

---

## 11. Optimization Questions (50+)

1. Rewrite `OR` conditions on different columns as `UNION ALL` to utilize separate indexes.
2. Use covering indexes to enable Index-Only Scans.
3. Use Keyset pagination instead of `OFFSET`.

---

## 12. Advanced Questions (50+)

1. Explain Google Spanner's TrueTime API and global external consistency.
2. How does Serializable Snapshot Isolation (SSI) track dangerous dependency cycles?
3. Discuss vector indexing (HNSW, IVF-PQ) for AI similarity search.

---

*Refer to handpicked learning materials in [`Resources.md`](file:///s:/Interview_Guide/DBMS/Resources.md)!*
