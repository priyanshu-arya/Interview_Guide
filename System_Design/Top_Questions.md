# 🏆 Top System Design Interview Questions (2026–2027)

*A curated collection of the top repeated, most difficult, frequently rejected, and differentiating system design questions across FAANG and top tier product companies.*

---

## 📌 Section 1: Top 25 Most Repeated Questions (Deep Breakdown)

---

### 1. Design a URL Shortener (e.g., TinyURL)
- **Difficulty**: Medium | **Level**: SDE-2 | **Company Categories**: All (Google, Amazon, Meta, Microsoft)
- **Why Asked**: Tests basic distributed system concepts: hashing, key-value storage, encoding, scaling reads, and capacity estimation.

#### 🎯 Detailed Design Approach
1. **Requirements & Scope**:
   - *Functional*: Given a long URL, return a unique 7-character short URL (`https://tinyurl.com/abc123X`). Redirect short URL to long URL (301 Permanent vs 302 Temporary). Custom aliases, optional expiration.
   - *Non-Functional*: Ultra-low latency ($<10\text{ms}$ redirect), high availability ($99.99\%$), 100:1 read-to-write ratio.
2. **Capacity Estimation**:
   - $100\text{M}$ new URLs/month $\rightarrow \sim 40 \text{ writes/sec}$. $4,000 \text{ reads/sec}$.
   - Storage for 5 years: $100\text{M} \times 12 \times 5 = 6\text{B}$ records. At $500 \text{ bytes/record} \rightarrow 3 \text{ TB}$ storage.
3. **API Design**:
   - `POST /api/v1/shorten` $\rightarrow$ `{ "longUrl": "https://...", "customAlias": "my-link", "expireDate": "2027-01-01" }`
   - `GET /{shortCode}` $\rightarrow$ Returns HTTP `301 Moved Permanently` (browser caches redirect, lower server load) or `302 Found` (if analytics tracking per click is mandatory).
4. **Key Generation Strategies**:
   - *Option A: MD5 / Base62 Hashing*: Base62 uses $[a-z, A-Z, 0-9]$. Length of 7 characters gives $62^7 \approx 3.5 \text{ Trillion}$ unique keys. MD5 produces 128-bit hash; take first 7 chars. Handle collisions by appending random salt and re-hashing.
   - *Option B: Offline Key Generation Service (KGS)*: Dedicated service pre-generates unique 7-char Base62 keys in advance and stores them in two SQL tables (`unused_keys` and `used_keys`). App servers fetch key batches into RAM. Guarantees zero runtime collisions and zero hashing latency!
5. **Storage Architecture**:
   - Key-Value Store / NoSQL (DynamoDB or Cassandra) partitioned by `short_code`.
   - Redis cluster caching top $20\%$ hot URLs (LRU eviction).

```
Client -> Load Balancer -> Web Server -> Redis Cache (Hit: 302 Redirect)
                                    |-> (Miss) -> NoSQL DB (Read -> Update Cache -> Redirect)
```

- **Follow-up Questions**: *How to handle malicious links? How to expire keys automatically?*
- **Common Mistakes**: Using auto-increment SQL IDs without encoding (predictable, security vulnerability).

---

### 2. Design a Real-Time Chat System (e.g., WhatsApp / Slack)
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Meta, Telegram, Slack, Uber
- **Why Asked**: Tests persistent real-time connections, message fan-out, sequence ordering, and offline message storage.

#### 🎯 Detailed Design Approach
1. **Key Architecture Components**:
   - **Chat Terminal Service**: Handles persistent WebSocket connections. Manages connection state mapped to `user_id` in a distributed Redis session store.
   - **Message Sync & Persistence**: Append-only NoSQL storage (Cassandra/HBase) partitioned by `chat_id` with `message_id` (Snowflake ID) as clustering key.
2. **Message Delivery Flow**:
   - User A sends message to User B via WebSocket.
   - WebServer receives message, assigns monotonic `message_id`, writes to Kafka topic `chat-messages`.
   - Worker writes message to Cassandra DB.
   - Worker queries Redis Session Store for User B's connected WebSocket server.
   - *If Online*: Forward message via WebSocket to User B's server $\rightarrow$ User B.
   - *If Offline*: Send APNs / FCM Push Notification. When User B re-connects, fetch unread messages where `message_id > last_received_id`.
3. **Group Chat Fan-out**:
   - For small groups ($<500$ members): Fan-out on write (publish message to each member's queue).
   - For large channels (Slack $10,000+$ members): Fan-out on read (publish to channel queue; members pull from offset).

---

### 3. Design a Social Media News Feed (e.g., Facebook / Twitter)
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Meta, Twitter, LinkedIn
- **Why Asked**: Evaluates push vs pull architectural trade-offs, feed generation, celebrity fan-out problems, and ML ranking integration.

#### 🎯 Detailed Design Approach
1. **Push vs. Pull Architecture**:
   - *Fan-out on Write (Push)*: When user posts, write post ID to feed cache (Redis sorted set) of all followers. **Fast Reads** ($O(1)$), **Slow Writes** (unscalable for users with millions of followers).
   - *Fan-out on Read (Pull)*: When user opens app, pull recent posts from all followed users and merge-sort. **Fast Writes** ($O(1)$), **Slow Reads** ($O(F \log F)$ where $F$ is followed count).
   - *Hybrid Model (FAANG Standard)*: Push model for regular users ($<5,000$ followers). Pull model for celebrity accounts ($>10,000$ followers). When a regular user opens feed, combine their pre-computed Redis feed cache with on-demand pulled posts from celebrities they follow.
2. **Storage**:
   - *Post Service DB*: Relational DB or Document DB (MongoDB) for post content & media metadata.
   - *Feed Cache*: Redis Sorted Sets per user: `ZADD feed:user_123 <timestamp> <post_id>`. Limit to top 800 items per user.

---

### 4. Design a Ride-Hailing Service (e.g., Uber / Lyft)
- **Difficulty**: Very Hard | **Level**: Staff | **Company Categories**: Uber, Lyft, Grab
- **Why Asked**: Evaluates real-time geospatial location indexing, state machine management, match algorithms, and surge pricing.

#### 🎯 Detailed Design Approach
1. **Geospatial Indexing**:
   - Drivers send GPS updates every 4 seconds.
   - Standard DB queries (`WHERE distance < 5km`) cannot scale at $100\text{k}$ updates/sec.
   - Use **Geohash** or **Google S2 Geometry Cells** (Quadtree). Index driver locations in Redis using Geospatial Data Structures (`GEOADD drivers_ring <lon> <lat> <driver_id>`).
2. **Matching Engine**:
   - Rider requests ride $\rightarrow$ Location Service calculates rider's Geohash cell $\rightarrow$ Queries Redis for nearby active drivers in current and adjacent cells.
   - Dispatch Service uses weighted matching (distance, driver rating, ETA) and sends offer via WebSocket to candidate driver with 15-second acceptance lease.

---

### 5. Design a Distributed Key-Value Store (e.g., Amazon DynamoDB)
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Amazon, Google, Meta
- **Why Asked**: Tests fundamental distributed systems knowledge: consistent hashing, quorum consensus, conflict resolution, LSM trees, and anti-entropy.

#### 🎯 Detailed Design Approach
1. **Data Partitioning**: Consistent Hashing Ring with virtual nodes for even distribution.
2. **Replication & Quorum**: Replicate each key to next $N$ physical nodes on ring.
   - Tunable consistency: $N$ (replicas), $W$ (write quorum), $R$ (read quorum).
   - Strong consistency when $W + R > N$. Eventual consistency when $W + R \le N$.
3. **Conflict Resolution**: Vector Clocks / Version Vectors to detect concurrent writes. Last-Write-Wins (LWW) as fallback using wall clock timestamps.
4. **Anti-Entropy**: Merkle Trees created per node partition to rapidly sync out-of-date replicas in background with minimal bandwidth.
5. **Storage Engine**: LSM Tree (MemTable + WAL + SSTables + Bloom Filters).

---

### 6–25. Summary Matrix for Top Repeated Designs

| # | System | Core Architectural Focus | Primary Tech Stack |
| :--- | :--- | :--- | :--- |
| **6** | **Rate Limiter** | Redis Lua scripts, Sliding window counter, API Gateway placement. | Redis, NGINX, Lua |
| **7** | **YouTube / Netflix** | Chunked upload, HLS/DASH adaptive streaming, CDN edge distribution, distributed transcoding queue. | AWS S3, CloudFront, FFmpeg, Kafka |
| **8** | **Search Autocomplete** | Trie data structure with pre-computed top-K frequencies, offline MapReduce updates. | In-Memory Trie, Elasticsearch, Spark |
| **9** | **Notification System** | Decoupled channel workers (Email, SMS, Push), idempotency keys, exponential backoff. | RabbitMQ, Twilio, APNs, FCM |
| **10** | **E-Commerce (Amazon)** | Inventory reservation locks, Saga pattern checkout, read-heavy catalog caching. | PostgreSQL, Redis, Kafka, Saga Orchestrator |
| **11** | **Pastebin** | S3 object store for text payload, SQL metadata, automated expiration workers. | S3, PostgreSQL, Redis |
| **12** | **Web Crawler** | Distributed URL Frontier (priority queue), robots.txt parser, Bloom Filter deduplication. | Kafka, Redis, HBase |
| **13** | **Distributed Cache** | Consistent hashing, LRU eviction, multi-master replication, cache stampede prevention. | C++, Redis internals |
| **14** | **Recommendation System** | Feature store, candidate generation (ANN vector search), online re-ranking service. | Pinecone, Spark, Redis, Ray |
| **15** | **CI/CD Pipeline** | Distributed build agents, worker DAG orchestration, docker registry caching. | Kubernetes, Go, MinIO |
| **16** | **Kafka-like Queue** | Zero-copy read (`sendfile`), append-only disk segments, Consumer Group offsets. | Java/Scala, PageCache |
| **17** | **Google Drive / Dropbox** | Block-level file splitting (4MB chunks), content-addressable hash deduplication. | S3, MySQL, Redis |
| **18** | **Job Scheduler** | Distributed cron, leader election (Zookeeper), heartbeat leases, idempotent workers. | Go, Redis, Zookeeper |
| **19** | **Leaderboard** | Redis Sorted Sets (`ZADD`, `ZREVRANGE`), DB persistence for historical logs. | Redis, PostgreSQL |
| **20** | **Voting System (Reddit)** | Decay ranking algorithm, eventual consistency upvote counters, write batching. | Cassandra, Redis |
| **21** | **Stock Exchange Engine** | Lock-free order book matching (Price-Time priority), ring buffer (Disruptor pattern). | C++, LMAX Disruptor |
| **22** | **API Gateway** | Rate limiting, SSL termination, JWT auth validation, dynamic request routing. | Kong, Envoy, NGINX |
| **23** | **URL Bookmarking** | Full-text search tagging, async preview snapshot generator. | Elasticsearch, S3 |
| **24** | **Global CDN** | Anycast BGP routing, HTTP reverse proxy caching, purge invalidation pub/sub. | NGINX, Varnish, Anycast |
| **25** | **Multiplayer Leaderboard** | Real-time WebSocket sync, Redis cluster sharding by game session. | Redis, WebSockets |

---

## 💥 Section 2: Top 50 Most Difficult Questions

1. **Google Spanner Architecture**: How TrueTime API with GPS and atomic clocks provides external consistency globally.
2. **CRDT Real-Time Collaborative Editor**: Conflict-free Replicated Data Types vs Operational Transformation (OT) in Google Docs.
3. **1M Transactions/sec Fraud Detection**: Real-time feature calculation over sliding windows using Apache Flink & Cassandra.
4. **HyperLogLog Cardinality Estimation**: Counting 1 Billion unique visitors with 99% accuracy using 1.5KB memory.
5. **Distributed Lock Manager with Fencing**: Preventing race conditions in split-brain environments without clock assumptions.
6. **1M Concurrent WebSockets Multiplayer Backend**: Linux kernel tuning (`epoll`, `somaxconn`), connection multiplexing, and Redis pub/sub routing.
7. **Distributed Vector Search (Pinecone/Milvus)**: Billion-scale HNSW (Hierarchical Navigable Small World) index sharding and ANN queries.
8. **Financial Trading Engine with Sub-Millisecond Deterministic Latency**: FPGA acceleration, lock-free ring buffers, and zero-copy network stacks.
9. **Streaming SQL Engine (ksqlDB)**: Exactly-once stream processing guarantees, rocksDB state stores, and sliding window joins.
10. **Globally Distributed Rate Limiter**: Syncing rate counters across 5 continents without adding cross-DC WAN latency.
11. **Saga Pattern Transaction Coordinator**: Compensating workflow execution, out-of-order event handling, and dead letter queues.
12. **High-Throughput Log Processing (100 TB/day)**: Columnar compression, vectorization, ClickHouse indexing, and Kafka ingestion.
13. **Real-Time Bidding (RTB) Ad Engine**: Sub-10ms response SLAs, ML model inference at edge, and budget pacing algorithms.
14. **Graph Database Sharding**: Min-cut graph partitioning for social network queries to minimize multi-hop cross-node network hops.
15. **DDoS Attack Mitigation Pipeline**: Real-time traffic analysis, eBPF/XDP packet filtering, and Anycast BGP scrubbing centers.
16. **Offline-First Data Sync**: Vector clocks, delta synchronization, and CRDT local-first DBs (RxDB / WatermelonDB).
17. **Zero-Downtime Multi-Region Database Failover**: Active-Active PostgreSQL / MySQL with multi-primary replication limits.
18. **Distributed File System (HDFS / GFS)**: NameNode metadata sharding, heartbeats, and block replication strategy.
19. **Live Video Conferencing (Zoom / Google Meet)**: WebRTC, SFU (Selective Forwarding Unit) vs MCU (Multipoint Control Unit) video routing.
20. **Petabyte-Scale Time-Series Storage**: Downsampling, retention policies, LSM rollup compaction in InfluxDB/TimescaleDB.
21. **Distributed Cron Scheduler for Millions of Tasks**: Priority queues, leader election, and distributed timing wheel.
22. **Zero-Copy Network Messaging**: Linux `sendfile` syscall, kernel page cache, and hardware DMA offloading.
23. **High-Availability Service Discovery**: Gossip protocol vs Raft consensus in Consul / Eureka under network partitions.
24. **Dynamic Image Rendering at Scale**: On-the-fly resizing, WebP conversion, GPU acceleration, and edge CDN caching.
25. **Multi-Currency Digital Wallet Ledger**: Double-entry bookkeeping, strict ACID transaction isolation, and auditability.
26–50. *(Including: Real-Time ASR Speech Transcription Pipeline, Hardware Security Module Key Management, Real-Time Event-Driven Outbox Pattern, IoT Telemetry Ingestion for 10M Devices, Global Anycast DNS Routing System).*

---

## 🚫 Section 3: Top 50 Frequently Rejected Questions (Candidate Traps)

| Candidate Mistake / Trap | Why It Leads to Rejection | How to Fix It |
| :--- | :--- | :--- |
| **1. Jumping into drawing boxes immediately** | Shows lack of structure and disregard for constraints. | Spend 5 minutes on Requirements & Capacity Estimation first. |
| **2. Single Point of Failure (SPOF)** | Designing a system where a single DB or server crash brings down the app. | Always place load balancers and DB replicas (Active-Passive or Active-Active). |
| **3. Using Redis for primary persistence** | Risk of data loss during power outage or RAM eviction. | Use Redis for caching; persist primary source of truth in SQL/Cassandra. |
| **4. Ignoring Database Writes** | Assuming database scale is solved just by adding read replicas. | Discuss DB sharding, partitioning, write-heavy LSM stores, or queues. |
| **5. Claiming "Exactly-Once" without caveats** | Ignores network failure reality (two generals problem). | Explain idempotent consumer design + deduplication tables. |
| **6. Over-Engineering Simple Requirements** | Introducing Kafka, Kubernetes, and Spark for an app with 10 requests/min. | Match architecture complexity to given QPS and SLA constraints. |
| **7. Ignoring Security & Auth** | Leaving APIs wide open without OAuth2/JWT or rate limiting. | Mention API Gateway auth validation and HTTPS encryption. |
| **8. Forgetting Cache Invalidation** | Updating the DB but leaving stale data in Redis indefinitely. | Discuss TTLs, Write-Through, or CDC (Debezium) cache invalidation. |
| **9. Auto-Increment SQL IDs for Public URLs** | Exposes total count and allows easy malicious scraping. | Use Base62 UUIDs or Snowflake IDs. |
| **10. Celebrity Fan-out Crash** | Writing a post to 100M Redis feed lists synchronously on post creation. | Implement hybrid push/pull model for high-follower accounts. |

---

## 🌟 Section 4: Top 50 Differentiating Senior Questions

1. *“How do you guarantee exactly-once processing in an end-to-end Kafka-to-Database streaming pipeline?”*  
   **Answer**: Combine Kafka transactional producer with idempotent database upsert queries (`INSERT INTO ... ON CONFLICT DO UPDATE`) using deterministic message business keys.
2. *“How do you prevent cache stampedes (thundering herd) when a hot key expires in Redis?”*  
   **Answer**: Use **Distributed Locking** (Redlock) so only 1 worker queries DB and updates cache, or use **Probabilistic Early Expiration** (XFetch algorithm) / mutex pre-warming.
3. *“What happens when a distributed system experiences a 500ms Java GC pause during a master lease renewal?”*  
   **Answer**: The master may lose its lease while still assuming it is master. Storage nodes will reject its stale requests if clients pass a monotonic **Fencing Token** issued by the lock service.
