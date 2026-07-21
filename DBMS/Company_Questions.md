# 🏬 Company-Specific DBMS Interview Questions & Hiring Patterns

Curated interview themes and actual technical questions drawn from real engineering interview loops at top product companies (Big Tech, FinTech, High-Scale Unicorns, Infrastructure Leaders).

---

## 🌐 Category 1: Big Tech (Google, Meta, Amazon, Microsoft, Apple, Netflix)

### 1. Amazon: Write-Heavy Key-Value Store Engine Selection
- **Commonly Asked By**: Amazon (AWS DynamoDB / Retail Platform Teams)
- **Why It Matters**: Evaluates ability to select storage engine architectures based on write/read ratios.
- **Expected Answer**:
  - Choose **LSM-Tree** (RocksDB/Cassandra style) over B-Tree for write-heavy workloads.
  - Detail sequential append-only writes to **MemTable** + **WAL**, flushed to **SSTables**.
  - Mention **Bloom Filters** to prevent unnecessary SSTable disk lookups during point reads.
- **Follow-up Questions**: How do you tune compaction to avoid write stall?
- **Interview Tip**: Always discuss write amplification vs read amplification trade-offs.

### 2. Google: External Consistency & Spanner TrueTime
- **Commonly Asked By**: Google (Spanner / Cloud Spanner / Core Infrastructure)
- **Why It Matters**: Tests advanced distributed systems consensus understanding.
- **Expected Answer**:
  - Explain Spanner's use of **TrueTime API** (atomic clocks + GPS receivers) to bound clock uncertainty ($\epsilon$).
  - Monotonic commit timestamps ensure external consistency (strict serializability) across global multi-DC transactions using 2PC over Paxos groups.
- **Interview Tip**: Mention how hardware clock synchrony simplifies distributed concurrency control compared to pure logical clocks.

### 3. Meta: Online Zero-Downtime Schema Migration
- **Commonly Asked By**: Meta / WhatsApp / Instagram Database Reliability Engineering
- **Why It Matters**: Real-world operational expertise on high-volume tables.
- **Expected Answer**:
  - Use shadow table pattern (`gh-ost` / `pt-online-schema-change`).
  - Create ghost table $\rightarrow$ apply DDL $\rightarrow$ stream live binlog writes $\rightarrow$ backfill existing data $\rightarrow$ atomic table swap.
- **Follow-up Questions**: How do triggers impact database write throughput during online DDL?

---

## 💳 Category 2: FinTech & Payments (Stripe, Razorpay, PayPal, Block)

### 4. Stripe: Double-Entry Ledger Schema & Atomicity
- **Commonly Asked By**: Stripe / PayPal / Financial Ledger Teams
- **Why It Matters**: Non-negotiable accuracy in ledger operations.
- **Expected Answer**:
  - **Double-Entry Bookkeeping**: $\sum \text{Debits} = \sum \text{Credits}$.
  - Immutable ledger tables (`INSERT` only).
  - Use pessimistic locking (`SELECT ... FOR UPDATE`) with deterministic PK sorting order to avoid deadlocks.
  ```sql
  BEGIN;
  SELECT balance FROM accounts WHERE id IN (101, 102) ORDER BY id FOR UPDATE;
  INSERT INTO ledger_entries (tx_id, account_id, amount, type) 
  VALUES ('tx_100', 101, -50.00, 'DEBIT'), ('tx_100', 102, 50.00, 'CREDIT');
  COMMIT;
  ```

### 5. Razorpay: Webhook Idempotency & Unique Constraints
- **Commonly Asked By**: Razorpay / Swiggy Payments / Payment Gateways
- **Why It Matters**: Retries under network timeouts must not duplicate charges.
- **Expected Answer**:
  - Unique constraint on `idempotency_key`.
  - Store request state in DB; on duplicate key error, return stored response immediately.

---

## 🚗 Category 3: High-Scale Unicorns (Uber, Swiggy, Zomato, Airbnb)

### 6. Uber: Real-Time Geospatial Driver Location Indexing
- **Commonly Asked By**: Uber / Lyft / Location Platform Teams
- **Why It Matters**: High-frequency spatial updates crush standard B-Tree indexes.
- **Expected Answer**:
  - Convert 2D coordinates `(lat, lon)` into 64-bit spatial cell IDs using **Uber H3** or **Geohash**.
  - Store dynamic locations in RAM (Redis Geospatial), flushing periodically to PostgreSQL PostGIS `GiST` indexes.

### 7. Swiggy / Zomato: High-Contention Flash Sale Inventory Deduction
- **Commonly Asked By**: Swiggy / Zomato / E-Commerce Platforms
- **Why It Matters**: 50,000 concurrent requests on a single item cause row locking bottlenecks.
- **Expected Answer**:
  - **Redis Atomic Decr** with async Kafka queuing for DB writes.
  - Or **Inventory Slotting**: Split item count into 10 sub-rows (`item_101_slot_1..10`) to reduce row locking contention by 10x.

---

## 🏢 Category 4: Enterprise & Infrastructure (Databricks, Snowflake, Oracle, MongoDB, Nvidia)

### 8. Databricks / Snowflake: Row-Oriented vs Column-Oriented OLAP Engines
- **Commonly Asked By**: Databricks / Snowflake / Analytical Engine Teams
- **Why It Matters**: Differentiates transactional vs analytical query engines.
- **Expected Answer**:
  - Row-Oriented (Postgres/MySQL): Fast point writes/reads (`SELECT * WHERE id = X`).
  - Column-Oriented (ClickHouse/Snowflake/Parquet): Fast aggregations (`SELECT AVG(salary)`), high vector compression, SIMD execution.

### 9. MongoDB / Databricks: Sharding Key Selection & Hotspot Prevention
- **Commonly Asked By**: MongoDB / Distributed Database Infrastructure Teams
- **Why It Matters**: Poor shard key choice causes single-node write saturation.
- **Expected Answer**:
  - High cardinality, even write distribution, avoidance of monotonically increasing keys (e.g., auto-increment timestamp causes all writes to hit latest shard).

---

## 📝 Company Pattern Summary Matrix

| Company | Core Topic Tested | Preferred Answer Strategy |
| :--- | :--- | :--- |
| **Google** | Distributed Systems (Spanner, TrueTime, Paxos) | Explain hardware clock bounds ($\epsilon$) & strict serializability. |
| **Amazon** | Storage Engine Internals (LSM vs B-Tree) | Explain Memtable/SSTable write path & Bloom filters. |
| **Meta** | Database Reliability (Online DDL, Replication) | Outline shadow table migration (`gh-ost`) & binlog streaming. |
| **Stripe** | Financial Consistency (Ledger, Isolation) | Immutable double-entry bookkeeping + deterministic row locking. |
| **Uber** | Spatial Indexing (H3, PostGIS, GiST) | Convert 2D coordinates to 1D spatial cells (H3/Geohash). |
| **Snowflake** | Analytical Query Engines (Columnar, SIMD) | Columnar storage, compression, and vectorized execution. |

---

*Proceed to [`Practice_Questions.md`](file:///s:/Interview_Guide/DBMS/Practice_Questions.md) to practice hands-on coding, MCQs, scenarios, debugging, and production problems!*
