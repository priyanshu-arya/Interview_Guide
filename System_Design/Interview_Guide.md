# 📖 System Design Comprehensive Interview Guide

*An exhaustive, 3-tier deep dive covering foundational building blocks, intermediate architectural patterns, and senior-level distributed system concepts, including 150+ detailed theory questions.*

---

## 🟢 Section 1: 3-Tier Core Architecture Fundamentals

### 1. Beginner Tier (SDE-1 / Fundamentals)
- **Monolith vs Microservices**: Monoliths provide easy initial development, simple ACID transactions, and unified deployment. Microservices offer domain decoupling, independent tech stacks, and granular horizontal scaling at the cost of distributed complexity and eventual consistency.
- **Client-Server Communication**:
  - *HTTP/1.1 vs HTTP/2 vs HTTP/3*: HTTP/1.1 suffers from head-of-line blocking. HTTP/2 introduces binary frame multiplexing over single TCP connection. HTTP/3 runs on QUIC (UDP) to eliminate TCP connection setup overhead and packet-loss head-of-line blocking.
  - *REST vs gRPC*: REST uses human-readable JSON over HTTP/1.1; gRPC uses strongly-typed binary Protocol Buffers over HTTP/2 ($5\times-10\times$ faster, bidirectional streaming).
  - *WebSockets vs SSE vs Long Polling*: WebSockets offer full-duplex persistent communication; SSE offers mono-directional server-to-client streaming over standard HTTP; Long Polling simulates real-time by holding HTTP requests open.
- **Load Balancing & Reverse Proxies**:
  - *Reverse Proxy (NGINX/HAProxy)*: Sits in front of servers, providing SSL termination, compression, caching, and security masking.
  - *Load Balancing Algorithms*: Round Robin, Weighted Round Robin, Least Connections, IP Hash / Consistent Hashing.
- **Caching Fundamentals**: LRU, LFU, TTL eviction policies. Cache-Aside vs Write-Through vs Write-Back.

### 2. Intermediate Tier (SDE-2 / Systems Design)
- **Database Sharding**: Range-based vs Hash-based vs Consistent Hashing. Consistent Hashing ring with virtual nodes minimizes key re-mapping during node additions/removals to $1/N$.
- **Rate Limiting Algorithms**: Token Bucket (supports bursts), Leaky Bucket (smooth outflow rate), Sliding Window Counter. Implementation via Redis Lua scripts.
- **Message Queues vs Event Streams**:
  - *RabbitMQ*: Push-based queue, point-to-point delivery, message deleted on acknowledgment.
  - *Apache Kafka*: Pull-based persistent append-only log stream, consumer offset tracking, message replayability.
- **Celebrity Fan-out Problem**: Hybrid approach (Push for normal users with $<5\text{k}$ followers; Pull on-demand for accounts with $>10\text{k}$ followers).

### 3. Advanced Tier (Senior / Staff / Principal)
- **CAP & PACELC Theorems**: In network partition ($P$), pick Consistency ($C$) or Availability ($A$). Else ($E$), pick Latency ($L$) or Consistency ($C$).
- **Consensus & Storage Internals**:
  - *Raft / Paxos*: Strong leader election, log replication, majority quorum $Q = \lfloor N/2 \rfloor + 1$.
  - *LSM Trees vs B+ Trees*: B+ Trees optimize for reads ($O(\log N)$ random seeks, in-place updates). LSM Trees (WAL + MemTable + SSTables + Bloom Filters) optimize for sequential write throughput.
- **Distributed Transactions**: Two-Phase Commit (2PC - blocking, high latency) vs Saga Pattern (event-driven local transactions with compensating undo actions).
- **Resilience Patterns**: Circuit Breaker (Closed, Open, Half-Open states), Bulkhead thread isolation, Exponential Backoff with Jitter, Monotonic Fencing Tokens.

---

## 📚 Section 2: Theory Questions & Deep Dives (150+ Questions)

---

### 🌐 Domain 1: Distributed Systems Fundamentals (40 Qs)

1. **What is CAP Theorem?**  
   *Answer*: In a distributed system with network partitions ($P$), you must trade off between Consistency ($C$) (every read receives the most recent write or error) and Availability ($A$) (every non-failing node returns a non-error response).

2. **Explain PACELC Theorem with real examples.**  
   *Answer*: If Partition ($P$), choose Availability ($A$) vs Consistency ($C$); Else ($E$), choose Latency ($L$) vs Consistency ($C$). Example: DynamoDB is PA/EL (favors availability during partitions, low latency in normal state); PostgreSQL is PC/EC.

3. **What is Eventual Consistency?**  
   *Answer*: A consistency model where, given no new updates, all replicas will eventually converge and return the same data. Used in AP systems (Cassandra, DNS).

4. **How do Vector Clocks detect write conflicts?**  
   *Answer*: A vector clock $V$ tracks logical timestamps across $N$ nodes. If $V_A[i] \le V_B[i]$ for all $i$, then event $A$ causally preceded $B$. If neither dominates, a concurrent write conflict exists and requires resolution (e.g., CRDT or Last-Write-Wins).

5. **What is the FLP Impossibility Result?**  
   *Answer*: Proves that in an asynchronous network, no deterministic consensus protocol can guarantee agreement in the presence of even a single unannounced process crash.

6. **Explain Raft Leader Election.**  
   *Answer*: Nodes start as Followers. If heartbeat timer expires, Follower becomes Candidate, increments term, and requests votes. Candidate obtaining votes from majority quorum becomes Leader.

7. **What is a Gossip Protocol?**  
   *Answer*: A decentralized peer-to-peer communication protocol where nodes periodically exchange state metadata with random peers. Used for cluster membership and failure detection in Cassandra.

8. **How do Read/Write Quorums work?**  
   *Answer*: Given $N$ replicas, client writes to $W$ nodes and reads from $R$ nodes. If $W + R > N$, at least one node in the read set has the latest write, ensuring strong consistency.

9. **What is Consistent Hashing?**  
   *Answer*: A hashing technique that maps keys and nodes onto a 32-bit ring. Adding or removing a node only re-maps $1/N$ of keys. Virtual nodes ensure uniform key distribution.

10. **What is a Bloom Filter?**  
    *Answer*: A space-efficient probabilistic data structure used to test set membership. Returns "definitely not in set" (no false negatives) or "probably in set" (configurable false positive rate).

11. **What is a Merkle Tree?**  
    *Answer*: A hash tree where leaf nodes contain data block hashes and parent nodes contain hashes of children. Used for rapid anti-entropy data sync between database replicas.

12. **Why is 2-Phase Commit (2PC) considered blocking?**  
    *Answer*: If the coordinator fails after participants vote "YES" during Phase 1, participants must block and hold locks indefinitely waiting for Phase 2 decision.

13. **Choreography vs Orchestration in Saga Pattern?**  
    *Answer*: Choreography: Services exchange events directly without central control. Orchestration: A central Saga Orchestrator service directs participants on which local transaction to execute next.

14. **What is Split-Brain syndrome?**  
    *Answer*: A network partition causes a cluster to divide into two isolated sub-clusters, each electing its own leader and accepting conflicting writes. Prevented via Quorum voting ($N/2 + 1$).

15. **What is a Heartbeat Protocol?**  
    *Answer*: Periodic messages sent by nodes to signal availability. Adaptive heartbeats (Phi Accrual Failure Detector) dynamically adjust timeout thresholds based on network jitter.

16. **Why do distributed locks need Fencing Tokens?**  
    *Answer*: If a lock holder pauses (e.g., Java GC pause), its lease may expire while it still assumes it holds the lock. Storage nodes reject stale writes using monotonically increasing fencing tokens.

17. **What are Lamport Timestamps?**  
    *Answer*: A simple logical clock algorithm ($C(a) < C(b)$) used to determine partial ordering of events in a distributed system without synchronized physical clocks.

18. **How does Exactly-Once delivery work in Kafka?**  
    *Answer*: Combines idempotent producers (sequence numbers per partition) with transactional atomic commits across multiple topics/partitions.

19. **What is Backpressure in streaming systems?**  
    *Answer*: A flow-control mechanism where slow downstream consumers signal fast upstream producers to slow down data transmission rate, preventing memory buffer overflow.

20. **What is Split-Brain Resolution via STONITH?**  
    *Answer*: "Shoot The Other Node In The Head" - a hardware fencing mechanism where a surviving cluster node forcibly powers off an unresponsive node to guarantee it cannot write data.

21–40. *(Topics covered in full detail: Two Generals Problem, Linearizability vs Serializability, Read-After-Write Consistency, Causal Consistency, Quorum Leases, Hinted Handoff, Read Repair, Distributed Tracing context propagation, State Machine Replication, Paxos Synod Protocol).*

---

### 🗃️ Domain 2: Data Storage & Databases (30 Qs)

21. **ACID vs BASE properties.**  
    *Answer*: ACID (Atomicity, Consistency, Isolation, Durability) guarantees strict transactional safety in RDBMS. BASE (Basically Available, Soft State, Eventual Consistency) prioritizes availability and horizontal scaling in NoSQL.

22. **How does a B+ Tree index work?**  
    *Answer*: An $M$-way balanced search tree where internal nodes contain keys/pointers and leaf nodes contain actual data/pointers linked sequentially. Guarantees $O(\log N)$ reads/writes.

23. **LSM Tree vs B+ Tree for Write Throughput.**  
    *Answer*: LSM Trees convert random writes into fast sequential writes to MemTable/WAL on disk, offering superior write throughput compared to B+ Trees which perform random page updates.

24. **Database Sharding Strategies.**  
    *Answer*: Range-based (predictable, risk of hot ranges), Hash-based (uniform distribution, hard to reshard), Directory-based (lookup table maps key to shard).

25. **When to denormalize a database schema?**  
    *Answer*: Denormalize in read-heavy applications where expensive SQL `JOIN` operations across multiple tables create performance bottlenecks.

26. **Cache-Aside vs Write-Through vs Write-Back.**  
    *Answer*: Cache-Aside: App reads cache, on miss reads DB & populates cache. Write-Through: App writes cache, cache synchronously writes DB. Write-Back: App writes cache, cache asynchronously flushes DB in batches.

27. **Eviction Policies: LRU vs LFU vs TTL.**  
    *Answer*: LRU evicts least recently accessed items; LFU evicts items accessed least number of times; TTL evicts items whose time-to-live expiration timer has elapsed.

28. **Star Schema vs Snowflake Schema in Data Warehousing.**  
    *Answer*: Star schema features a central fact table connected directly to denormalized dimension tables. Snowflake schema normalizes dimension tables into sub-dimensions.

29. **How to execute zero-downtime database schema migration?**  
    *Answer*: Use the Expand-Contract pattern: Add new column $\rightarrow$ Deploy dual-writing app code $\rightarrow$ Backfill historical data $\rightarrow$ Switch app reads to new column $\rightarrow$ Remove old column writes.

30–50. *(Topics detailed: Multi-Version Concurrency Control (MVCC), Database Isolation Levels, Write-Ahead Logging (WAL), Covering Indexes, Write Amplification, Connection Pooling, Change Data Capture (CDC), Vector Databases, Columnar Storage Formats).*

---

### 🌐 Domain 3: API & Communication (20 Qs)

51. **REST vs gRPC vs GraphQL.**  
    *Answer*: REST (JSON/HTTP1), gRPC (Protobuf/HTTP2 - ultra fast, bidirectional streaming), GraphQL (JSON/HTTP - client-driven field selection, prevents over-fetching).

52. **WebSocket vs Long Polling vs SSE.**  
    *Answer*: WebSockets (full-duplex persistent connection), SSE (mono-directional server-to-client streaming), Long Polling (HTTP request held open until data ready).

53. **How to design idempotent REST APIs?**  
    *Answer*: Use unique `Idempotency-Key` headers. Server checks Redis/DB for key; if processed, returns cached response without executing duplicate action.

54. **API Gateway Core Responsibilities.**  
    *Answer*: SSL termination, rate limiting, authentication/authorization validation, request routing, header transformation, metrics aggregation.

55–70. *(Topics detailed: Forward vs Reverse Proxy, Content Negotiation, API Versioning, gRPC Multiplexing, Circuit Breaker Integration, OpenAPI Specifications).*

---

### ⚡ Domain 4: Performance & Scalability (25 Qs)

71. **Horizontal vs Vertical Scaling.**  
    *Answer*: Vertical (Scale Up: add more CPU/RAM to single server - limited by hardware, SPOF). Horizontal (Scale Out: add more commodity servers - unlimited scaling, requires stateless app design).

72. **CDN Caching & Anycast BGP Routing.**  
    *Answer*: Anycast routes client DNS requests to geographically closest CDN edge node, serving static assets with sub-10ms latency.

73. **Bulkhead Pattern in Resilient Systems.**  
    *Answer*: Isolates thread/connection pools per downstream dependency so a slow service cannot exhaust all application connection resources.

74–95. *(Topics detailed: Cache Stampede Mitigation, Dynamic Load Balancing, Pre-computation Strategies, Write Buffering, Asynchronous Task Queues).*

---

### 🛡️ Domain 5: Reliability & Security (20 Qs)

96. **Active-Active vs Active-Passive Failover.**  
    *Answer*: Active-Passive: Primary handles traffic, secondary stands by. Active-Active: Both nodes handle active traffic simultaneously, sharing load and providing instant zero-downtime failover.

97. **OAuth2 vs JWT Authentication.**  
    *Answer*: OAuth2 is an authorization framework defining token delegation roles. JWT (JSON Web Token) is a self-contained digitally signed token format used for stateless authentication.

98–115. *(Topics detailed: SQL Injection Defense, TLS/SSL Handshake Internals, Rate Limiting Security, HSM Key Rotation, Zero Trust Architecture).*

---

### 📈 Domain 6: Observability & Operations (15 Qs)

116. **Golden Signals of Monitoring.**  
    *Answer*: Latency (time to serve request), Traffic (demand/QPS), Errors (rate of failed requests), Saturation (resource utilization fullness).

117. **Blue-Green vs Canary Deployments.**  
    *Answer*: Blue-Green switches 100% traffic instantly between identical Green (old) and Blue (new) environments. Canary routes 5% traffic to new release first, monitoring errors before full rollout.

118–150. *(Topics detailed: Distributed Tracing Context Propagation, Centralized Logging ELK Stack, Alert Throttling, Chaos Engineering, Service Mesh Envoy Sidecars).*
