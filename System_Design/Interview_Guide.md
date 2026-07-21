# 📖 System Design Comprehensive Interview Guide

*An exhaustive, 3-tier deep dive covering foundational building blocks, intermediate architectural patterns, and senior-level distributed system concepts.*

---

## 🟢 1. Beginner Level (SDE-1 / Fundamentals)

### 📌 Core Topics & Concepts

#### 1. Monolithic vs. Microservices Architecture
- **Monolith**: Single codebase, unified deployment, simple debugging and transactions. Bottlenecks: tight coupling, single point of scaling, long build/deploy cycles.
- **Microservices**: Decoupled, domain-driven services communicating via network protocols (HTTP/gRPC/Kafka). Benefits: independent scaling, fault isolation, tech stack flexibility. Challenges: network overhead, distributed tracing, complex deployment, eventual consistency.

#### 2. Client-Server & Communication Protocols
- **HTTP/1.1 vs HTTP/2 vs HTTP/3**:
  - *HTTP/1.1*: Sequential request-response, head-of-line blocking, TCP handshake overhead.
  - *HTTP/2*: Multiplexed streams over a single TCP connection, header compression (HPACK), server push.
  - *HTTP/3*: Based on QUIC (UDP), resolves TCP head-of-line blocking, faster connection establishment.
- **REST vs. gRPC**:
  - *REST*: Stateless, HTTP/JSON, human-readable, ubiquitous, but higher payload size.
  - *gRPC*: HTTP/2 based, Protocol Buffers (binary serialization), strongly typed, ultra-low latency, bidirectional streaming. Ideal for inter-service communication.
- **WebSockets vs. Long Polling vs. Server-Sent Events (SSE)**:
  - *Long Polling*: Client requests, server holds response until data available, client reconnects. High connection creation overhead.
  - *WebSockets*: Full-duplex, persistent TCP connection. Ideal for chat, collaborative tools, live gaming.
  - *SSE*: Mono-directional (server -> client) over standard HTTP. Ideal for news feeds, stock tickers, notification feeds.

#### 3. Basic Load Balancing & Proxying
- **Forward Proxy vs. Reverse Proxy**:
  - *Forward Proxy*: Sits in front of clients (e.g., VPN, enterprise security proxy). Hides client IP.
  - *Reverse Proxy*: Sits in front of servers (e.g., NGINX, HAProxy). Handles SSL termination, load balancing, security, compression, and routing. Hides backend server IPs.
- **Load Balancing Algorithms**:
  - *Round Robin / Weighted Round Robin*: Distributes requests sequentially.
  - *Least Connections*: Routes to server with fewest active connections.
  - *IP Hash / Consistent Hashing*: Maps client IP/key to specific server for session stickiness.

#### 4. Caching Fundamentals
- **Cache Locations**:
  - *Client-side*: Browser cache, local storage.
  - *CDN*: Edge servers caching static assets (images, CSS, JS, videos).
  - *Application cache*: In-memory cache (Redis, Memcached) residing between app servers and database.
- **Cache Eviction Policies**:
  - *LRU (Least Recently Used)*: Evicts item not accessed for longest time (implemented via Doubly Linked List + Hash Map).
  - *LFU (Least Frequently Used)*: Evicts item accessed least number of times.
  - *FIFO / TTL*: Time to live expiration.

### ⚠️ Common Beginner Mistakes
1. **Hardcoding IP Addresses**: Forgetting to use DNS or load balancers for service discovery.
2. **Ignoring Statelessness**: Storing user session state directly on application server memory instead of centralized Redis cache.
3. **Overusing Database for Static Assets**: Storing images/videos directly as BLOBs in SQL DB instead of Object Storage (S3) + CDN.

### ❓ Expected Beginner Interview Questions
- *Q: How does a web browser fetch `https://example.com` from scratch?*  
  **Answer**: DNS lookup (browser cache -> OS cache -> resolver -> root/TLD/authoritative nameserver) -> TCP Handshake (3-way) / TLS Handshake -> HTTP GET request routed via load balancer -> Web App Server -> DB fetch -> HTTP Response (200 OK) -> HTML/CSS parsing and asset rendering.

---

## 🟡 2. Intermediate Level (SDE-2 / Systems Design)

### 📌 Frequently Tested Concepts & Architectural Patterns

#### 1. Database Partitioning & Sharding
- **Vertical Partitioning**: Splitting tables by domain columns (e.g., Users table split into `UserCredentials` and `UserProfile`).
- **Horizontal Partitioning (Sharding)**: Splitting table rows across multiple database servers.
  - *Range-based Sharding*: Sharding by ID ranges (e.g., 1-10k, 10k-20k). Problem: Hotspots on latest range.
  - *Hash-based Sharding*: Sharding by `hash(key) % N`. Problem: Re-sharding when $N$ changes requires moving almost all keys.
  - *Consistent Hashing*: Maps keys and nodes onto a 32-bit ring. Adding/removing a node only re-maps $1/N$ of keys. Virtual nodes ensure uniform key distribution.

#### 2. Advanced Caching Strategies
- **Cache-Aside (Lazy Loading)**: App checks cache. On miss, reads from DB, updates cache, and returns. Good for read-heavy workloads.
- **Write-Through**: App writes to cache, cache synchronously writes to DB. Ensures consistency; higher write latency.
- **Write-Back (Write-Behind)**: App writes to cache, cache asynchronously flushes to DB in batches. Ultra-fast writes; risk of data loss on crash.
- **Write-Around**: App writes directly to DB, bypassing cache. Prevents cache pollution for rarely read data.

#### 3. Rate Limiting Algorithms
- **Token Bucket**: Fixed capacity bucket filled with tokens at constant rate. Request consumes token. Allows bursts up to capacity.
- **Leaky Bucket**: FIFO queue processing requests at a smooth, constant output rate. Drops excess requests when queue is full.
- **Sliding Window Counter**: Hybrid combining fixed window counters with weight of previous window. Memory efficient and accurate.

```python
# Redis Lua Script for Atomic Token Bucket Rate Limiting
"""
KEYS[1]: rate_limit_key (e.g., rate:user_123)
ARGV[1]: max_tokens (e.g., 100)
ARGV[2]: refill_rate_per_sec (e.g., 10)
ARGV[3]: current_timestamp (seconds)
ARGV[4]: requested_tokens (e.g., 1)
"""
lua_script = """
local key = KEYS[1]
local max_tokens = tonumber(ARGV[1])
local refill_rate = tonumber(ARGV[2])
local now = tonumber(ARGV[3])
local requested = tonumber(ARGV[4])

local data = redis.call('HMGET', key, 'tokens', 'last_updated')
local tokens = tonumber(data[1])
local last_updated = tonumber(data[2])

if tokens == nil then
    tokens = max_tokens
    last_updated = now
else
    local delta = math.max(0, now - last_updated)
    tokens = math.min(max_tokens, tokens + delta * refill_rate)
    last_updated = now
end

if tokens >= requested then
    tokens = tokens - requested
    redis.call('HMSET', key, 'tokens', tokens, 'last_updated', last_updated)
    redis.call('EXPIRE', key, math.ceil(max_tokens / refill_rate))
    return 1 -- Allowed
else
    return 0 -- Denied (Rate Limited)
end
"""
```

#### 4. Message Queues & Event Streaming
- **Message Queue (RabbitMQ / SQS)**: Push-based, point-to-point delivery. Messages deleted once acknowledged by consumer. Ideal for task queues.
- **Event Stream (Apache Kafka)**: Pull-based, persistent append-only commit log. Consumers maintain offset pointers. Messages can be replayed. High throughput, partitioned by key.

### 🔄 Real Interview Expectations & Follow-ups
- **Follow-up**: *“How do you handle celebrity users (e.g., Cristiano Ronaldo with 500M followers posting a video) in news feed push models?”*  
  - **Answer**: Use a **hybrid model**. For regular users, use fan-out on write (push to follower feeds). For celebrity users, use fan-out on read (pull celebrity posts on-demand when followers refresh feed) to avoid writing millions of cache entries on every post.

---

## 🔴 3. Advanced Level (Senior / Staff / Principal)

### 📌 Production Concepts, Architecture & Deep Internals

#### 1. Distributed Consensus & CAP Theorem
- **CAP Theorem**: In a network partition ($P$), a distributed system must choose between Consistency ($C$) (all nodes see same data simultaneously) or Availability ($A$) (every non-failing node returns a response).
- **PACELC Theorem**: Extends CAP: If there is a Partition ($P$), choose between Availability ($A$) and Consistency ($C$); Else ($E$), choose between Latency ($L$) and Consistency ($C$).
- **Consensus Algorithms (Raft & Paxos)**:
  - *Raft*: Strong leader election, log replication, safety rules. Cluster requires majority quorum $Q = \lfloor N/2 \rfloor + 1$ to commit log entries and tolerate $f$ failures out of $2f + 1$ nodes.

#### 2. Database Internals: LSM Trees vs. B+ Trees
- **B+ Tree (Read-Optimized)**:
  - In-place mutation, balance tree, leaf nodes store data and form linked list.
  - High read performance ($O(\log N)$ disk seeks), low write throughput due to random I/O and page splits. Used in PostgreSQL, MySQL InnoDB.
- **LSM Tree (Log-Structured Merge Tree - Write-Optimized)**:
  - Sequential writes to append-only Write-Ahead Log (WAL) and in-memory MemTable (Red-Black / SkipList).
  - When MemTable fills, flushed to disk as immutable SSTable (Sorted String Table).
  - Periodic background compaction (Size-Tiered or Leveled) merges SSTables.
  - Uses **Bloom Filters** to quickly confirm key absence before disk access. Used in Cassandra, RocksDB, DynamoDB.

```
LSM Tree Architecture:
Write Path: Client -> Write-Ahead Log (WAL on disk) + MemTable (RAM)
Flush Path: MemTable -> SSTable Level 0 (Disk) -> Compaction -> SSTable Level 1..N
Read Path:  Client -> MemTable -> Bloom Filter Check -> SSTable L0..LN
```

#### 3. Distributed Transactions & Consistency Guarantees
- **Two-Phase Commit (2PC)**:
  - *Prepare Phase*: Coordinator asks all participants to prepare transaction.
  - *Commit Phase*: If all vote YES, coordinator sends COMMIT; else ABORT.
  - *Drawbacks*: Blocking protocol; coordinator single point of failure; high lock contention latency over wide area network.
- **Saga Pattern (Event-Driven / Orchestrated)**:
  - Series of local transactions. Each local transaction updates DB and publishes event.
  - If a step fails, Saga executes a sequence of **compensating transactions** in reverse to undo changes.
- **Vector Clocks & Version Vectors**:
  - Used for causal ordering and conflict detection in eventual consistency systems (DynamoDB).
  - A vector clock $V$ is an array of size $N$ (number of nodes). Node $i$ increments $V[i]$ on write.

#### 4. Resiliency & Operational Excellence
- **Circuit Breaker Pattern**: Prevents cascading failures. States: *Closed* (normal), *Open* (requests fail fast without remote call when error threshold exceeded), *Half-Open* (trial requests to test recovery).
- **Bulkhead Pattern**: Isolates resources (thread pools, connection pools) so failure in one client/service call doesn't deplete total system capacity.
- **Exponential Backoff with Jitter**:
  $$t_{\text{wait}} = \min(t_{\text{max}}, t_{\text{base}} \times 2^{\text{attempt}}) + \text{random\_jitter}$$
  Prevents thundering herd problems during service failure recovery.

### 🏆 Senior Interview Expectations & Edge Cases
- **Zero-Downtime Database Migration**: Expand-Contract pattern (Add column -> Dual Write -> Backfill old data -> Switch Reads -> Drop old column).
- **Fencing Tokens**: Used with distributed locks to prevent split-brain race conditions where a paused/GC-delayed lock holder writes after lease expiry. Lock service returns a monotonically increasing token (e.g., 84); storage rejects writes with token < highest seen token (e.g., 85).
