# 🔝 Top Questions: Repeated, Difficult, Rejected & Differentiating

This document presents the 4 primary question tiers asked in DBMS interviews across Big Tech, FinTech, and high-scale startups.

---

## 📌 Top 25 Most Repeated Questions

These are the DBMS questions that appear in virtually every interview loop.

| # | Question | Difficulty | Level | Company Categories |
|---|----------|------------|-------|-------------------|
| 1 | Explain ACID properties. Give real-world violations. | Medium | SDE1/2 | All |
| 2 | Difference between DELETE, TRUNCATE, and DROP. | Easy | Fresher | All |
| 3 | What is normalization? Explain 1NF, 2NF, 3NF, BCNF. | Medium | SDE1/2 | All |
| 4 | How does a B-tree index work? Why are B-trees preferred in DBMS? | Hard | SDE2 | Big Tech |
| 5 | What are the different isolation levels? Provide examples of anomalies. | Hard | SDE2/Senior | Big Tech, FinTech |
| 6 | What is a deadlock? How can it be prevented or detected? | Medium | SDE2 | All |
| 7 | Explain JOIN types with examples (INNER, LEFT, RIGHT, FULL, CROSS). | Easy | Fresher | All |
| 8 | Difference between WHERE and HAVING clauses. | Easy | Fresher | All |
| 9 | What is a clustered vs non-clustered index? How many of each can a table have? | Medium | SDE2 | Big Tech |
| 10 | Describe the SQL execution order of a SELECT statement. | Easy | Fresher | All |
| 11 | What is a view? Can it be updated? What are materialized views? | Medium | SDE1/2 | All |
| 12 | Explain the difference between a primary key, unique key, and foreign key. | Easy | Fresher | All |
| 13 | What is a transaction? How is consistency guaranteed in concurrent transactions? | Medium | SDE2 | FinTech, Big Tech |
| 14 | How would you optimize a slow query? (missing index, non-sargable WHERE) | Medium | SDE2 | All |
| 15 | What is database replication? Compare master-slave vs master-master. | Hard | Senior | Big Tech, Unicorn |
| 16 | CAP theorem – explain the trade-offs and real DB examples. | Hard | Senior | Big Tech, Unicorn |
| 17 | What is sharding? How do you pick a sharding key? | Hard | Senior | Big Tech, Unicorn |
| 18 | What are triggers? When to use (and not use) them? | Medium | SDE2 | All |
| 19 | What is a checkpoint in a database? How does it help recovery? | Medium | SDE2 | Big Tech |
| 20 | Explain WAL (Write-Ahead Logging) and its role in crash recovery. | Hard | Senior | Big Tech |
| 21 | What is the difference between optimistic and pessimistic locking? | Medium | SDE2 | FinTech |
| 22 | Why are indexes not always beneficial? When would you avoid an index? | Medium | SDE2 | All |
| 23 | How do you prevent SQL injection? | Medium | SDE1/2 | All |
| 24 | What is a composite index? How does column order matter? | Medium | SDE2 | Big Tech |
| 25 | What is eventual consistency? When is it acceptable? | Hard | Senior | Big Tech, Unicorn |

<details>
<summary><b>📌 Click to expand detailed answers for each Top 25 question</b></summary>

### 1. ACID properties
- **Atomicity**: All or nothing; transaction either fully completes or fully rolls back. Example: money transfer – debit and credit must both happen or none.
- **Consistency**: Transaction brings database from one valid state to another, preserving constraints (foreign keys, uniqueness, etc.).
- **Isolation**: Concurrent transactions should not interfere; isolation levels define degree of interference (dirty reads, non-repeatable reads, phantoms).
- **Durability**: Once committed, changes survive system crashes (via WAL and checkpointing).
- **Real-world violation**: Without isolation, a withdrawal from an ATM might read a balance that hasn't been updated yet, causing overdraft.

### 2. DELETE vs TRUNCATE vs DROP
- `DELETE` removes rows based on WHERE clause, logs each row, can be rolled back, slower, fires triggers, reclaims space within table.
- `TRUNCATE` removes all rows, minimal logging (DDL), cannot rollback in some DBs, resets identity counters, faster, does not fire individual row triggers.
- `DROP` removes the entire table structure and data, cannot rollback, frees all space.
- **Follow-up**: Which one is DML vs DDL? (`DELETE`=DML, `TRUNCATE`=DDL, `DROP`=DDL).

### 3. Normalization
- **1NF**: Atomic columns (no repeating groups), each cell single value.
- **2NF**: 1NF + no partial dependency (all non-key attributes depend on the whole primary key, not a subset).
- **3NF**: 2NF + no transitive dependency (non-key attributes shouldn't depend on other non-key attributes).
- **BCNF**: Every determinant must be a candidate key (stronger than 3NF).

### 4. B-tree index
A B-tree (balanced tree) maintains sorted data with minimal disk I/O. Each node contains multiple keys and pointers to children, keeping the tree shallow. Range queries and sequential access are efficient because leaves are linked. Databases prefer B-trees over binary trees because disk access is expensive; B-trees have high fan-out, reducing tree depth.

### 5. Isolation levels & anomalies
- **Read Uncommitted**: Dirty reads possible.
- **Read Committed**: No dirty reads, but non-repeatable reads possible.
- **Repeatable Read**: No non-repeatable reads, but phantoms possible (in PostgreSQL/InnoDB prevented).
- **Serializable**: No anomalies; strictest.
- **Anomalies**: Dirty read (read uncommitted data), Non-repeatable read (same row read twice gives different values), Phantom read (new rows appearing in a range).

### 6. Deadlock
Two or more transactions waiting for each other to release locks. Detection: wait-for graph cycle. Prevention: lock ordering, lock timeout, deadlock avoidance (wound-wait, wait-die), using snapshot isolation. DBMS often detects and aborts one transaction (victim).

### 7. JOIN types
INNER returns rows with matches in both tables; LEFT returns all left rows + matches; RIGHT all right rows; FULL all rows; CROSS Cartesian product.

### 8. WHERE vs HAVING
`WHERE` filters rows before grouping; `HAVING` filters groups after `GROUP BY`. Order: FROM $\rightarrow$ WHERE $\rightarrow$ GROUP BY $\rightarrow$ HAVING $\rightarrow$ SELECT $\rightarrow$ ORDER BY.

### 9. Clustered vs non-clustered index
- **Clustered**: Data rows physically stored in index order. Only one per table (typically primary key).
- **Non-clustered**: Separate structure storing key and pointer (row locator) to actual row. Multiple allowed.

### 10. SQL execution order
`FROM` (including JOINs) $\rightarrow$ `ON` $\rightarrow$ `WHERE` $\rightarrow$ `GROUP BY` $\rightarrow$ `HAVING` $\rightarrow$ `SELECT` $\rightarrow$ `DISTINCT` $\rightarrow$ `ORDER BY` $\rightarrow$ `LIMIT/OFFSET`.

### 11. Views
Virtual table based on SELECT query. Updateable views must satisfy constraints (no aggregates, single table). Materialized views store result physically and need refresh; improve performance for complex queries.

### 12. Keys
- **Primary Key**: Unique and not null, identifies each row.
- **Unique Key**: Enforces uniqueness but allows NULLs.
- **Foreign Key**: Enforces referential integrity pointing to parent table.

### 13. Transaction consistency under concurrency
ACID's Isolation ensures serializable order of concurrent transactions using locking or MVCC. Consistency maintained by checking constraints at commit. In MVCC, readers see consistent snapshot, writers create new versions.

### 14. Slow query optimization
1. Look at execution plan (`EXPLAIN`).
2. Identify full table scans $\rightarrow$ add indexes (covering if possible).
3. Rewrite non-sargable conditions.
4. Break complex ORs into `UNION ALL`.
5. Update statistics, consider partitioning.

### 15. Replication
- **Master-Slave**: Writes to master, reads from slaves. Async/sync. Scaling reads; failover requires promotion.
- **Master-Master**: Both nodes accept writes, conflict resolution needed.

### 16. CAP theorem
In distributed systems, pick two: Consistency (all see same data), Availability (every request receives response), Partition tolerance (works despite network splits). Choose CP (HBase, MongoDB) or AP (Cassandra, Dynamo).

### 17. Sharding
Splitting data across independent databases based on a shard key (e.g., hash of `user_id`). Avoid hotspots, ensure even distribution.

### 18. Triggers
Stored procedures executed on INSERT/UPDATE/DELETE. Used for auditing/validation. Overuse causes hidden side effects and debugging issues.

### 19. Checkpoint
Point where modified buffer pages flush to disk, allowing WAL truncation. Reduces crash recovery time.

### 20. WAL (Write-Ahead Logging)
Append-only log recording modifications before data pages hit disk. Replayed during crash recovery.

### 21. Optimistic vs pessimistic locking
- **Optimistic**: Validate version at commit, retry on conflict. Read-heavy workloads.
- **Pessimistic**: Lock resources when reading (`FOR UPDATE`). High-conflict workloads.

### 22. Index downsides
Slows down INSERT/UPDATE/DELETE, consumes disk space, ineffective on small tables or low selectivity columns.

### 23. SQL injection prevention
Parameterized queries / prepared statements, input validation, least privilege DB permissions.

### 24. Composite index column order
Index on `(col1, col2)` serves queries filtering on `col1`, or `col1 AND col2`. Cannot filter on `col2` alone. Put most selective columns first.

### 25. Eventual consistency
Replicas eventually converge to same value if no updates occur. Used in social feeds, DNS, caching.

</details>

---

## 🏋️ Top 50 Most Difficult Questions

| # | Question | Key Challenge |
|---|----------|---------------|
| 1 | B-tree vs LSM tree storage engines. When to choose each? | Write-optimized vs read-optimized trade-offs. |
| 2 | How does MVCC work in PostgreSQL/MySQL? | Snapshot isolation, transaction IDs, visibility, vacuum. |
| 3 | Design a distributed unique ID generator without collision. | Snowflake, logical clocks, coordination. |
| 4 | Change data capture (CDC) pipeline with exactly-once semantics. | Log parsing, deduplication, idempotent writes. |
| 5 | What is phantom read? How does gap locking prevent it? | Next-key locking, InnoDB internals. |
| 6 | Two-phase commit (2PC) and its failure modes. | Coordinator failure, blocking, XA transactions. |
| 7 | How does Raft consensus algorithm work in distributed DBs? | Leader election, log replication, safety. |
| 8 | Difference between snapshot isolation and serializable isolation. | Write skew anomaly example. |
| 9 | Slowly changing dimension type 2 table point-in-time SQL. | Temporal queries, effective dates. |
| 10 | How does PostgreSQL's VACUUM work and why is it necessary? | MVCC cleanup, freeze, transaction wraparound. |
| 11 | Row-based vs columnar storage trade-offs. | OLTP vs OLAP, compression, vectorization. |
| 12 | Shard a database without downtime. | Online rebalancing, dual writes, consistent hashing. |
| 13 | Query optimizer cost model & join order selection. | Statistics, histograms, cardinality estimation. |
| 14 | Pessimistic locking vs serializable isolation. | Trade-offs, deadlock risk, throughput. |
| 15 | How does DB handle a WAL that has grown too large? | Archiving, replication slots, logical decoding. |
| 16 | Design DB schema for IAM hierarchical roles. | Recursive relationships, row-level security. |
| 17 | Detect and resolve split-brain in master-master replication. | Quorum, fencing, STONITH. |
| 18 | Bloom filter optimization for DB queries. | Probabilistic existence, reduce disk lookups. |
| 19 | InnoDB undo log and redo log interaction. | Crash recovery, rollback, durability. |
| 20 | Implement graph DB on relational DB & recursive shortest path. | Adjacency list, recursive CTE, performance. |
| 21 | Inverted index & full-text search mechanics. | Tokenization, postings, ranking. |
| 22 | Transaction log reader crash recovery reconstruction steps. | Redo, undo, checkpoint. |
| 23 | Referential integrity across shards without cross-shard FKs. | Application-level checks, eventual consistency, sagas. |
| 24 | Logical vs physical replication. | Statement vs row vs WAL shipping. |
| 25 | Design rate limiter using database. | Token bucket, sliding window, race conditions. |
| 26 | Predicate locks and index-range locks. | Serializable isolation, conflict detection. |
| 27 | PostgreSQL TOAST handling large columns. | Compression, out-of-line storage. |
| 28 | Find gaps in sequence of IDs using window functions. | Row number difference, islands & gaps. |
| 29 | Correlated subquery efficiency & performance impact. | Execution plan, rewriting as JOIN. |
| 30 | Covering index eliminating key lookups. | Include columns, index-only scan. |
| 31 | B-tree vs B+ tree; why databases prefer B+ tree. | Leaf-only data, linked leaves for range scans. |
| 32 | Message queue with exactly-once delivery using relational DB. | Outbox pattern, idempotent consumer, deduplication table. |
| 33 | Write skew anomaly real-world example. | Snapshot isolation failure, doctor on call. |
| 34 | Tuning checkpoint frequency and WAL settings for write-heavy workload. | IOPS, recovery time objective (RTO). |
| 35 | Multi-tenant DB with RLS and logical data isolation. | Policy-based RLS, partition per tenant. |
| 36 | Challenges of eventual consistency in shopping cart. | Conflict resolution, merge semantics, CRDTs. |
| 37 | Cost-based optimizer deciding between hash, nested loop, merge join. | Data sizes, sorted inputs, memory (`work_mem`). |
| 38 | Write path of Write-Ahead Log in LSM tree (Cassandra/RocksDB). | Memtable, SSTable, compaction. |
| 39 | Hotspot in distributed DB and mitigation. | Key distribution, salting, reverse indexes. |
| 40 | Data integrity in event-sourced architecture. | Command validation, snapshots, idempotency. |
| 41 | Serializable snapshot isolation (SSI) conflict detection. | SSI, dangerous structures, dependency graphs. |
| 42 | Pitfalls of database triggers for business logic. | Hidden side effects, testing, debugging. |
| 43 | Modeling time-series data in relational DB. | Partitioning by time, downsampling, materialized views. |
| 44 | Distributed SQL query engine (Presto) join execution. | Cross-DC shuffle, broadcast vs partitioned joins. |
| 45 | Hard parse vs soft parse in Oracle/MySQL. | Cursor cache, SQL matching, literal vs bind variables. |
| 46 | Safe schema migration on large table without downtime. | Online DDL, shadow table, atomic swap. |
| 47 | LSM tree compaction strategies (Size-Tiered vs Leveled). | Write amplification vs read amplification vs space overhead. |
| 48 | Role of transaction coordinator in distributed DBMS. | 2PC, failure handling, commit decision. |
| 49 | Globally distributed DB with strong consistency (Spanner). | Spanner, TrueTime, atomic clocks. |
| 50 | Compare ACID transactions with BASE transactions. | Airbnb booking (ACID) vs Twitter likes (BASE). |

---

## ❌ Top 50 Frequently Rejected Questions

Candidates often fail these due to shallow textbook answers without real-world depth.

| # | Question | Why Candidates Get Rejected | Ideal Senior Answer |
|---|----------|-----------------------------|---------------------|
| 1 | "How to optimize a slow query?" | Saying "Just add an index." | Analyze execution plan (`EXPLAIN ANALYZE`), check for non-sargable predicates, check statistics, consider covering index or partitioning. |
| 2 | "Design DB for a blog/social app." | Over-normalizing everything into 3NF. | Balance normalization for writes with denormalization/caching for read-heavy feeds. |
| 3 | "Difference between TRUNCATE and DELETE?" | Saying "Both delete all rows." | Detail DDL vs DML, row-level logging vs page deallocation, trigger execution, and identity reset. |
| 4 | "Explain Normalization." | Reciting textbook definitions. | Give concrete real-world schema violation and show table splitting step-by-step. |
| 5 | "What is CAP theorem?" | Claiming a system can achieve CA during partition. | Explain that partitions are network reality; system must trade off Availability vs Consistency (CP or AP). |
| 6 | "Handle concurrent updates?" | Saying "Put it in a transaction." | Transaction alone doesn't prevent lost updates; explain locking (`FOR UPDATE`), optimistic versioning, or isolation levels. |
| 7 | "When to use NoSQL?" | Saying "For Big Data." | Detail specific access pattern trade-offs (schema flexibility, horizontal scale, document store vs relational join complexity). |
| 8 | "What is a deadlock?" | Only defining it without prevention methods. | Detail Wait-For graphs, lock ordering, timeouts, and application-level retries. |
| 9 | "How does B-tree index work?" | Drawing a binary search tree. | Draw M-way B+ Tree with high fan-out, internal node key pointers, and linked leaf pages. |
| 10 | "Why use a covering index?" | Not knowing what it means. | Explain Index-Only Scan avoiding expensive secondary index bookmark lookups to table heap. |
| 11 | "Can Primary Key be NULL?" | Pausing or guessing. | Absolutely NO; Primary Key enforces Entity Integrity (`NOT NULL` + `UNIQUE`). |
| 12 | "Is `COUNT(*)` faster than `COUNT(1)`?" | Claiming `COUNT(1)` is faster. | Modern query engines optimize both identically; `COUNT(*)` counts rows, `COUNT(col)` excludes NULLs. |
| 13 | "How to paginate 10M rows?" | Using `OFFSET 10000000`. | Explain that `OFFSET` scans and discards 10M rows; use Keyset/Seek Pagination instead. |
| 14 | "Why is `SELECT *` bad?" | Saying "It takes more memory." | Detail cache pollution, breaking covering index scans, network payload, and schema change vulnerability. |
| 15 | "What happens on `ROLLBACK`?" | Thinking data is deleted. | Explain Undo Log replay restoring original tuple versions. |
| 16 | "Difference between UNION and UNION ALL?" | Not knowing performance impact. | `UNION` performs expensive distinct sort/hash to remove duplicates; `UNION ALL` streams directly. |
| 17 | "How does WAL ensure durability?" | Confusing WAL with data files. | Explain log records hit non-volatile disk via `fsync` before transaction commit returns success. |
| 18 | "Can foreign key be NULL?" | Saying "No". | Yes; NULL FK represents an optional relationship (unassociated parent). |
| 19 | "What is Write Skew?" | Confusing with dirty read. | Explain Snapshot Isolation anomaly where two TXs read overlapping data and update disjoint rows, breaking global invariant. |
| 20 | "How to scale relational writes?" | Saying "Add read replicas." | Replicas scale READS; writes require sharding, functional partitioning, or LSM engines. |
| 21–50 | *(See detailed breakdown in Practice_Questions.md)* | Missing production depth. | Explain disk I/O, locks, memory pools, and distributed consensus. |

---

## 🌟 Top 50 Questions That Differentiate Top Candidates

These questions differentiate SDE2 from Senior/Staff/Lead candidates.

1. How would you implement a global transaction across microservices with different databases? *(Saga vs 2PC)*
2. Design a database schema for a ride-hailing app (Uber). Include geospatial queries, real-time driver locations, and consistency.
3. Explain how PostgreSQL's MVCC vacuum avoids transaction ID wraparound.
4. When would you choose an LSM-tree based storage engine over a B-tree?
5. How would you design a database change management process that avoids breaking schema changes in production?
6. What is the impact of a long-running transaction on database performance?
7. How do you handle a hot row problem in a high-concurrency application?
8. Compare InnoDB and MyISAM for a write-heavy workload.
9. How would you design a system that uses database as a queue (with `FOR UPDATE SKIP LOCKED`) and why it might be a good/bad idea?
10. How does Google Spanner achieve external consistency across data centers globally?
11. What are the trade-offs of Serializable Snapshot Isolation (SSI) vs 2-Phase Locking?
12. How do you implement zero-downtime database sharding rebalancing?
13. How does columnar database vectorization speed up analytical aggregations by 100x?
14. How to detect and resolve split-brain in master-master replication without data corruption?
15. What is the role of Bloom Filters in LSM-tree read path?
16. How to model bi-temporal data (valid time vs transaction time) in SQL?
17. Explain ARIES recovery algorithm phases (Analysis, Redo, Undo).
18. How do connection poolers like PgBouncer manage transaction-level vs session-level pooling?
19. How to achieve exactly-once message processing using database outbox pattern?
20. Compare size-tiered compaction vs leveled compaction in RocksDB/Cassandra.
21–50. *(Detailed architectural answers provided across `Interview_Guide.md` and `Company_Questions.md`)*

---

*Continue your preparation with [`Company_Questions.md`](file:///s:/Interview_Guide/DBMS/Company_Questions.md)!*
