# 🏆 Top System Design Interview Questions (2026–2027)

*An exhaustive, production-grade collection of repeated, difficult, tricky, rejected, and senior-differentiating system design questions across FAANG and top-tier product companies.*

---

## 📌 Section 1: Top 25 Most Repeated Questions (Exhaustive Architecture Breakdowns)

---

### 1. Design a URL Shortener (e.g., TinyURL / Bitly)
- **Difficulty**: Medium | **Level**: SDE-2 | **Company Categories**: All Tech Companies
- **Why Interviewers Ask It**: Tests fundamental distributed system concepts: Base62 encoding vs key generation services, SQL vs NoSQL key-value lookups, read-heavy scaling, 301 vs 302 HTTP redirects, and cache eviction strategies.
- **Detailed Answer & Simple Explanation**:
  - **Simple Explanation**: Map a long URL (e.g., `https://example.com/a/b/c/d`) to a short unique 7-character string (`https://tinyurl.com/aB3x9Z1`). When a user visits the short URL, the service resolves the key and redirects the client to the long URL.
  - **API Contract**:
    - `POST /api/v1/shorten` $\rightarrow$ Payload: `{ "longUrl": "https://...", "customAlias": "myLink", "expireSeconds": 86400 }` $\rightarrow$ Response: `{ "shortUrl": "https://tinyurl.com/aB3x9Z1" }`
    - `GET /{shortCode}` $\rightarrow$ Returns HTTP `301 Permanent Redirect` (browser caches redirect, saving server QPS) or HTTP `302 Temporary Redirect` (routes through server on every click, enabling click analytics & telemetry).
  - **Key Generation Approaches**:
    - *MD5 / SHA-256 Base62 Encoding*: Hash long URL and take first 7 Base62 characters ($62^7 \approx 3.5 \text{ Trillion}$ keys). Must handle hash collisions by appending a unique incrementing salt.
    - *Offline Key Generation Service (KGS)*: Dedicated service pre-generates unique 7-character Base62 keys in advance and stores them in SQL tables (`unused_keys` & `used_keys`). Web app servers load batches of 1,000 keys into RAM. Zero runtime collision check required!
  - **Storage & Caching Architecture**:
    - NoSQL Key-Value Store (DynamoDB or Cassandra) sharded by `short_code`. Schema: `short_code (PK), long_url, user_id, created_at, expire_at`.
    - Redis Cluster caching top 20% hot URLs (80/20 Pareto principle) with LRU eviction.
- **Practical Example**:
  - Daily scale: 10M new URLs/day $\rightarrow \sim 115 \text{ write QPS}$. 1B clicks/day $\rightarrow \sim 11,500 \text{ read QPS}$ (Read-heavy 100:1 ratio). Storage: 10M $\times 500 \text{ bytes} = 5 \text{ GB/day} \implies 1.8 \text{ TB/year}$.
- **Follow-up Questions**:
  - *How to purge expired URLs?* $\rightarrow$ Passive deletion upon GET request miss + active background cron sweep scanning expired key indexes during low traffic.
  - *How to prevent malicious user URL spamming?* $\rightarrow$ Rate limiting by API key / IP via Redis Token Bucket.
- **Common Mistakes & Interview Tips**:
  - ❌ *Mistake*: Using auto-increment SQL IDs exposed in URLs (`tinyurl.com/10254`), making URL scraping trivial.
  - 💡 *Tip*: State the 301 vs 302 redirect trade-off immediately to impress the interviewer.

---

### 2. Design a Real-Time Chat System (e.g., WhatsApp / Slack / Discord)
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Meta, Slack, Telegram, Discord, Uber
- **Why Interviewers Ask It**: Evaluates handling of stateful persistent TCP connections, guaranteed message delivery, message ordering across devices, group chat fan-out models, and presence heartbeat tracking.
- **Detailed Answer & Simple Explanation**:
  - **Simple Explanation**: Enable instant message transmission between two or more users, supporting online/offline presence status, typing indicators, and historical chat storage.
  - **Protocols & Stateful Connections**:
    - WebSockets for bi-directional real-time message transmission. Web clients establish persistent TCP handshakes with a scalable WebSocket Gateway cluster.
  - **Storage Architecture**:
    - *Message Store*: Cassandra / ScyllaDB (Append-only wide-column store, optimized for sequential writes). Primary Key: `(chat_id, message_id)`. `message_id` is a 64-bit Snowflake ID (time-sorted).
    - *User Session & Presence Store*: Redis Cluster mapping `user_id` $\rightarrow$ `websocket_server_ip` and online heartbeat timestamps.
  - **Message Flow (1-on-1)**:
    1. User A sends message to WebSocket Gateway Node 4.
    2. Gateway generates `message_id` and publishes message to Kafka topic `chat-messages`.
    3. Worker service persists message to Cassandra DB.
    4. Worker queries Redis Session Store for User B's gateway IP.
    5. If User B is online on Node 9 $\rightarrow$ Forward message via gRPC to Node 9 $\rightarrow$ Push to User B via WebSocket.
    6. If User B is offline $\rightarrow$ Send payload to APNs/FCM Push Notification Service.
- **Practical Example**:
  - Handling 100,000 concurrent WebSocket connections per gateway server using Linux `epoll` I/O multiplexing and tuned TCP buffers.
- **Follow-up Questions**:
  - *How to handle massive group chats with 50,000 members (Discord)?* $\rightarrow$ Switch from Push fan-out to Pull model (publish message once to channel stream; active clients pull by offset).
- **Common Mistakes & Interview Tips**:
  - ❌ *Mistake*: Polling HTTP REST endpoints every 1 second for new messages.
  - 💡 *Tip*: Explain why Snowflake IDs are critical for ordering messages when clients have clock skew.

---

### 3. Design a Social Media News Feed (e.g., Facebook Feed / Twitter Timeline)
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Meta, Twitter, LinkedIn
- **Why Interviewers Ask It**: Evaluates fan-out architectures (push vs pull), feed pre-computation, hybrid models for celebrity accounts, and real-time ML ranking pipelines.
- **Detailed Answer & Simple Explanation**:
  - **Simple Explanation**: Construct a customized, real-time list of posts created by people/entities a user follows, ranked by relevance and freshness.
  - **Fan-Out Models**:
    - *Push Model (Fan-out on Write)*: When user posts, push post ID to feed cache (Redis sorted set) of all followers. **Fast Reads** ($O(1)$), **Slow Writes** (unscalable for users with millions of followers).
    - *Pull Model (Fan-out on Read)*: When user opens app, query recent posts of all followed users and merge-sort. **Fast Writes** ($O(1)$), **Slow Reads** ($O(F \log F)$ where $F$ is follower count).
    - *Hybrid Model (FAANG Standard)*: Push for regular users ($<5,000$ followers). Pull for celebrity accounts ($>10,000$ followers). When a user refreshes feed, fetch pre-computed Redis feed and merge with pulled celebrity posts.
  - **Storage & Ranking**:
    - *Post DB*: PostgreSQL / MongoDB for original post metadata.
    - *Feed Cache*: Redis Sorted Sets per user (`ZADD feed:user_id <timestamp> <post_id>`). Truncated to top 800 items.
    - *Ranking Service*: ML Inference pipeline (C++ / gRPC) re-ranking top 100 posts based on user engagement signals (affinity, post type, freshness).
- **Practical Example**:
  - Twitter celebrity (e.g., Elon Musk with 180M followers) posting a tweet $\rightarrow$ System inserts tweet into primary DB and pushes to follower pull-buffer; does NOT attempt 180M Redis writes!
- **Follow-up Questions**:
  - *How to handle pagination?* $\rightarrow$ Cursor-based pagination (`max_id`) instead of SQL `OFFSET` (prevents missing/duplicate posts when new items arrive).
- **Common Mistakes & Interview Tips**:
  - ❌ *Mistake*: Using SQL `OFFSET` and `LIMIT` for news feed pagination under high write concurrency.
  - 💡 *Tip*: Always calculate write amplification factor for celebrity accounts to justify the hybrid fan-out model.

---

### 4. Design a Ride-Hailing Service (e.g., Uber / Lyft)
- **Difficulty**: Very Hard | **Level**: Staff | **Company Categories**: Uber, Lyft, Grab, Swiggy
- **Why Interviewers Ask It**: Evaluates real-time geospatial location indexing, state machine transitions, dispatch matching algorithms, and surge pricing engines.
- **Detailed Answer & Simple Explanation**:
  - **Simple Explanation**: Match riders seeking rides with nearby available drivers in real time while tracking live vehicle locations on a map.
  - **Geospatial Location Tracking**:
    - Active drivers stream GPS coordinates every 4 seconds via WebSocket / UDP to Location Ingestion Service.
    - Compute **Geohash** (or **Google S2 Cell ID**) at resolution level 13 ($\sim 150\text{m} \times 150\text{m}$ grid).
    - Store active driver locations in **Redis Geospatial Index** (`GEOADD drivers:geohash_cell <longitude> <latitude> <driver_id>`).
  - **Dispatch & Matching Engine**:
    - Rider requests ride $\rightarrow$ Match Service calculates rider's Geohash cell $\rightarrow$ Queries Redis for nearby available drivers in current and adjacent 8 cells.
    - Dispatch Service ranks candidate drivers by ETA, acceptance rate, and distance. Sends 15-second ride offer lease to top driver via WebSocket.
  - **Trip State Machine**:
    - States: `REQUESTED` $\rightarrow$ `MATCHED` $\rightarrow$ `DRIVER_ARRIVED` $\rightarrow$ `TRIP_IN_PROGRESS` $\rightarrow$ `COMPLETED` / `CANCELLED`.
    - State transitions managed via optimistic locking or distributed locks to prevent double-matching drivers.
- **Practical Example**:
  - 1 Million active drivers sending updates every 4s $\rightarrow 250,000 \text{ write QPS}$ to Redis Geospatial cluster.
- **Follow-up Questions**:
  - *How to calculate surge pricing in real time?* $\rightarrow$ Stream processing via Apache Flink counting ride requests vs available drivers per Geohash cell over 5-minute sliding windows.
- **Common Mistakes & Interview Tips**:
  - ❌ *Mistake*: Running SQL query `SELECT * FROM drivers WHERE ST_Distance(...) < 5` every 4 seconds across millions of drivers.
  - 💡 *Tip*: Draw a clear state machine diagram for the trip lifecycle before discussing database schemas.

---

### 5. Design a Distributed Key-Value Store (e.g., Amazon DynamoDB / Cassandra)
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Amazon, Google, Meta, Apple
- **Why Interviewers Ask It**: Tests foundational distributed systems concepts: consistent hashing, virtual nodes, tunable quorum consensus, LSM trees, and anti-entropy.
- **Detailed Answer & Simple Explanation**:
  - **Simple Explanation**: Build a highly available, fault-tolerant, horizontally scalable key-value database capable of petabyte-scale storage across thousands of commodity nodes.
  - **Data Partitioning & Replication**:
    - Consistent Hashing Ring with virtual nodes (256 v-nodes per physical server) to eliminate hotspots and distribute keys uniformly.
    - Replicate data across $N$ consecutive physical nodes on the ring ($N = \text{Replication Factor}$).
  - **Tunable Quorum Consensus**:
    - Configured via $N$ (Replicas), $W$ (Write Quorum), $R$ (Read Quorum).
    - Strong Consistency when $W + R > N$. Eventual Consistency when $W + R \le N$.
  - **Conflict Resolution**:
    - Vector Clocks / Version Vectors attached to data items to detect concurrent writes. Last-Write-Wins (LWW) wall clock fallback.
  - **Storage Engine**: LSM Tree (MemTable in RAM + Write-Ahead Log on disk + SSTables + Bloom Filters).
- **Practical Example**:
  - Writing to Cassandra: $N=3, W=2, R=2$. Master node writes to local MemTable/WAL and sends parallel writes to 2 replicas; returns success once 2 ACKs received.
- **Follow-up Questions**:
  - *How to repair out-of-sync nodes after a network partition?* $\rightarrow$ Background Merkle Tree comparison (anti-entropy) between replicas to identify and transfer missing range deltas.
- **Common Mistakes & Interview Tips**:
  - ❌ *Mistake*: Assuming eventual consistency means data is lost permanently.
  - 💡 *Tip*: Write down the $W + R > N$ quorum formula explicitly.

---

### 6–25. Summary Breakdowns for Remaining Top Repeated Questions

6. **Design a Distributed Rate Limiter**: Token Bucket / Sliding Window Counter using Redis Lua scripts for atomic updates without race conditions.
7. **Design a Video Streaming Platform (YouTube / Netflix)**: Upload chunking to S3 $\rightarrow$ Kafka transcoding queue (FFmpeg multi-resolution HLS/DASH) $\rightarrow$ Global CDN edge caching.
8. **Design Search Autocomplete (Typeahead)**: In-memory Trie sharded by prefix, offline MapReduce/Spark job rebuilding Trie frequency counts hourly, real-time Redis trend blending.
9. **Design a Distributed Notification System**: Priority Kafka queues per channel (Email, SMS, Push), idempotency key storage, dead-letter queues (DLQ), and Twilio/SendGrid/FCM integrations.
10. **Design an E-Commerce Platform (Amazon)**: Microservices, Redis atomic seat/inventory holds with 10-minute lease, Saga Orchestrator for checkout transactions.
11. **Design Pastebin**: S3 object store for text payloads, SQL for metadata, passive expiry checks + active cleanup cron sweep.
12. **Design a Web Crawler**: Distributed URL Frontier Priority Queue, Robots.txt parser, Bloom Filter for URL deduplication, HTML extraction workers.
13. **Design a Parking Lot System**: Low-Level OOD (Spots, Gates, Parking Floors) with database row-level locks for slot booking concurrency.
14. **Design a Distributed Cache (Redis)**: Consistent hashing ring, LRU eviction policy, Cache-Aside pattern, distributed mutex locks for stampede prevention.
15. **Design a Recommendation System**: Feature Store, Candidate Generation (ANN vector search via Pinecone), real-time re-ranking pipeline.
16. **Design a CI/CD Pipeline (GitHub Actions / Jenkins)**: Webhook ingestion, DAG dependency execution graph, distributed Docker runner pool.
17. **Design a Distributed Message Queue (Kafka)**: Append-only disk commit log, zero-copy `sendfile` kernel transfer, Consumer Group partition offset management.
18. **Design Cloud File Storage (Google Drive / Dropbox)**: Block-level file splitting (4MB chunks), SHA-256 deduplication, delta sync protocol, metadata SQL DB.
19. **Design a Distributed Job Scheduler**: Leader election via Consul/Zookeeper, timing wheel data structure, idempotent worker execution.
20. **Design a Real-Time Leaderboard**: Redis Sorted Sets (`ZADD`, `ZINCRBY`, `ZREVRANGE`), async DB flush workers for historical analytics.
21. **Design a Voting System (Reddit Upvotes)**: Decaying post score algorithm, eventual consistency write-batching via Kafka, Cassandra post counter tables.
22. **Design a Stock Exchange Matching Engine**: Lock-free order book data structures (Price-Time priority), LMAX Disruptor ring buffer, C++ low latency execution.
23. **Design an API Gateway**: SSL termination, JWT token validation, rate limiting middleware, dynamic route proxying (Envoy / Kong).
24. **Design a URL Bookmarking Service (Pocket)**: Async article scraping, readability text extraction, full-text Elasticsearch indexing.
25. **Design a Global Content Delivery Network (CDN)**: Anycast BGP routing, Edge HTTP reverse proxy caching (NGINX), pub/sub cache purge invalidation.

---

## 💥 Section 2: Top 50 Most Difficult Questions (Key Challenges & Solutions)

| # | Question | Core Technical Challenge & Architecture Solution |
|---|----------|--------------------------------------------------|
| 1 | **Google Spanner Architecture** | TrueTime API using atomic clocks & GPS receivers to bound clock uncertainty ($\epsilon \le 7\text{ms}$) for global external consistency. |
| 2 | **CRDT Collaborative Editor** | Conflict-free Replicated Data Types (RGA / LSEQ) enabling peer-to-peer real-time doc editing without central locks. |
| 3 | **1M QPS Fraud Detection** | Stateful stream processing over sliding time windows using Apache Flink and low-latency feature retrieval from Cassandra. |
| 4 | **HyperLogLog Cardinality** | Estimating unique items up to billions using 64 register buckets and harmonic mean with $<1.04/\sqrt{m}$ standard error. |
| 5 | **Distributed Lock with Fencing** | Generating monotonically increasing Fencing Tokens to prevent GC-paused clients from overwriting updated state. |
| 6 | **1M Concurrent WebSockets** | Linux kernel tuning (`sysctl` `epoll`, `file-max`), connection multiplexing, and Redis pub/sub cluster routing. |
| 7 | **Billion-Scale Vector Search** | HNSW (Hierarchical Navigable Small World) graph indexing and product quantization for sub-10ms ANN embedding queries. |
| 8 | **Sub-Millisecond Trading Engine** | FPGA hardware acceleration, zero-copy kernel bypass (Solarflare OpenOnload), and lock-free Disruptor ring buffers. |
| 9 | **Streaming SQL Engine** | Exactly-once stateful processing, RocksDB embedded state management, and changelog stream compaction. |
| 10 | **Globally Distributed Rate Limiter** | Synchronizing local rate counters asynchronously across regions without cross-datacenter synchronous WAN network hops. |
| 11–50 | *(Including: Distributed Transaction Saga Coordinators, Petabyte Log Analytics ClickHouse engines, WebRTC SFU Media Servers, Time-Series Compaction Engines, High-Churn Service Discovery).* |

---

## ⚡ Section 3: Top 50 Most Tricky Questions & Hidden Assumptions

1. **URL Shortener Expiry**: Hidden Requirement: How do you purge expired keys? *Fix*: Passive check on GET + background sweep.
2. **Chat System Offline Messages**: How to order messages across devices when offline for days? *Fix*: Snowflake ID sequence numbers.
3. **Rate Limiter Bursts**: How to allow sudden bursts without blowing up server CPU? *Fix*: Token Bucket with capacity max.
4. **News Feed Celebrity Accounts**: What happens when Elon Musk posts to 180M followers? *Fix*: Hybrid pull model for celebrities.
5. **Distributed Lock Lease Expired**: What if a Java thread pauses for 10s during GC while holding lock? *Fix*: Monotonic Fencing Tokens.
6. **Unique ID Generator Clock Skew**: What if NTP adjusts server clock backward by 5 seconds? *Fix*: Refuse generation or wait until clock catches up.
7. **Cache Invalidation Under High Concurrency**: Race condition where old DB value overwrites new cache value. *Fix*: Lock or Lease Tokens on Cache misses.
8. **Consistent Hashing Node Crash**: What if a node carrying 25% of traffic crashes? *Fix*: Virtual Nodes to spread load across all remaining nodes equally.
9. **E-Commerce Inventory Flash Sale**: 1,000 items left, 100,000 concurrent clicks. *Fix*: Atomic Redis decrement (`DECRBY`) before DB hit.
10. **Web Crawler Trap**: How to prevent crawler from looping infinitely in dynamic calendar pages? *Fix*: URL depth limits and domain canonicalization.
11–50. *(Including: CDN Edge Space Exhaustion, Saga Compensation Failure, Search Autocomplete Trend Spikes, Stock Exchange Order Front-Running, Multi-Region Eventual Consistency Conflicts).*

---

## 📋 Section 4: Top 50 Must-Know Questions

1. CAP Theorem in practice (CP vs AP systems).
2. SQL vs NoSQL selection criteria.
3. Consistent Hashing and Virtual Nodes.
4. Bloom Filters in system design.
5. Database Sharding strategies (Range, Hash, Directory).
6. Microservices vs Monolith trade-offs.
7. Caching strategies (Cache-Aside, Write-Through, Write-Back).
8. Load balancing algorithms (Round Robin, Least Connections, IP Hash).
9. Message Queues vs Event Streams (RabbitMQ vs Kafka).
10. Distributed Transactions (Saga Pattern vs 2PC).
11–50. *(Including: Vector Clocks, Gossip Protocol, Raft Consensus, Exactly-Once Processing, Idempotent API Design, Back-of-the-Envelope Estimation, Circuit Breakers, Service Discovery).*

---

## 🚫 Section 5: Top 50 Frequently Rejected Questions (Candidate Pitfalls)

| Candidate Failure / Trap | Reason for Rejection | How to Fix |
| :--- | :--- | :--- |
| **1. No Scale Estimation** | Designing without knowing if QPS is 10 or 1,000,000. | Always estimate QPS, Storage, Bandwidth in first 5 minutes. |
| **2. Single Point of Failure (SPOF)** | Having a single DB or server instance without redundancy. | Place Load Balancers and multi-AZ active-passive / active-active replicas. |
| **3. Using Redis for Primary Storage** | Risk of total data loss during power failure or RAM eviction. | Use Redis for caching; persist primary source of truth in SQL/Cassandra. |
| **4. Ignoring Write Bottlenecks** | Assuming read replicas solve database scale when writes are 80%. | Discuss DB sharding, LSM-tree databases, or write-buffering queues. |
| **5. Claiming "Exactly-Once" easily** | Ignores network failure reality (Two Generals Problem). | Explain idempotent consumers + deduplication tables. |
| **6. Over-Engineering Simple Needs** | Adding Kafka, Spark, and Kubernetes for 100 QPS. | Match architecture complexity to given requirements and scale. |
| **7. Ignoring API Security** | Leaving endpoints exposed without Auth or Rate Limiting. | Mention API Gateway auth (JWT/OAuth2) and rate limiting middleware. |
| **8. Forgetting Cache Invalidation** | Updating DB while leaving stale data in Redis indefinitely. | Discuss TTLs, Write-Through caching, or CDC invalidation. |
| **9. Predictable Auto-Increment IDs** | Security vulnerability allowing easy competitor scraping. | Use Base62 UUIDs or Snowflake IDs. |
| **10. Synchronous Celebrity Fan-out** | Writing a post to 100M follower feed caches synchronously. | Implement hybrid push/pull model for celebrity accounts. |
| 11–50. *(Including: Missing Monitoring Golden Signals, Ignoring Connection Pooling, Hardcoding IP addresses, Neglecting DB Schema Migrations).* |

---

## 🌟 Section 6: Top 50 Differentiating Senior Questions

1. *“How do you guarantee exactly-once processing in a Kafka-to-Database pipeline?”* $\rightarrow$ Transactional producer + Idempotent database upserts (`INSERT ... ON CONFLICT DO UPDATE`).
2. *“How do you handle cache stampedes when a hot Redis key expires?”* $\rightarrow$ Distributed locking (Redlock) or Probabilistic Early Expiration (XFetch).
3. *“What happens when a distributed node experiences a 500ms Java GC pause during master lease renewal?”* $\rightarrow$ Node loses lease; storage rejects stale writes using monotonic **Fencing Tokens**.
4. *“How do you achieve zero-downtime database schema migration for a 50TB table?”* $\rightarrow$ Expand-Contract pattern (Add column $\rightarrow$ Dual Write $\rightarrow$ Backfill $\rightarrow$ Read Switch $\rightarrow$ Drop old).
5. *“How do you design a rate limiter that enforces a global limit across 5 data centers without WAN latency?”* $\rightarrow$ Batch local counter allocations from central quota manager.
6–50. *(Including: Multi-Master Conflict Resolution via CRDTs, Dynamic Database Connection Pool Tuning, Low-Latency Feature Store Architecture, Zero-Copy Linux I/O Optimization).*

---

Proceed to [`Company_Questions.md`](file:///s:/Interview_Guide/System_Design/Company_Questions.md) for company-specific hiring patterns! 🚀
