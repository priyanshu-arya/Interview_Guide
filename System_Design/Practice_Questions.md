# 🧪 System Design Practice Questions, MCQs & Scenarios

*A comprehensive problem bank featuring Mini-Problems, Multiple-Choice Questions, Real-World Scenarios, Production Incidents, Debugging Challenges, Output Predictions, Best Practices, Optimization Exercises, and Advanced Design Questions.*

---

## 💻 Section 1: Design Mini-Problems (100+ Exercises)

---

### Problem 1: Implement Twitter Snowflake 64-Bit Unique ID Generator
- **Difficulty**: Medium
- **Problem Statement**: Implement a 64-bit unique ID generator supporting 1,024 worker nodes without central locks, generating $>10,000$ IDs/sec per node.

```python
import time

class SnowflakeIDGenerator:
    def __init__(self, worker_id: int, datacenter_id: int):
        self.worker_id = worker_id
        self.datacenter_id = datacenter_id
        self.sequence = 0
        self.last_timestamp = -1
        
        self.worker_id_bits = 5
        self.datacenter_id_bits = 5
        self.sequence_bits = 12
        
        self.worker_id_shift = self.sequence_bits
        self.datacenter_id_shift = self.sequence_bits + self.worker_id_bits
        self.timestamp_left_shift = self.sequence_bits + self.worker_id_bits + self.datacenter_id_bits
        self.sequence_mask = -1 ^ (-1 << self.sequence_bits)
        self.twepoch = 1767225600000 # Custom Epoch

    def _current_millis(self) -> int:
        return int(time.time() * 1000)

    def generate_id(self) -> int:
        timestamp = self._current_millis()
        if timestamp < self.last_timestamp:
            raise Exception("Clock moved backwards!")
        if timestamp == self.last_timestamp:
            self.sequence = (self.sequence + 1) & self.sequence_mask
            if self.sequence == 0:
                while timestamp <= self.last_timestamp:
                    timestamp = self._current_millis()
        else:
            self.sequence = 0
        self.last_timestamp = timestamp
        return ((timestamp - self.twepoch) << self.timestamp_left_shift) | \
               (self.datacenter_id << self.datacenter_id_shift) | \
               (self.worker_id << self.worker_id_shift) | \
               self.sequence
```

---

### Problem 2: Implement an O(1) LRU Cache
- **Difficulty**: Medium
- **Problem Statement**: Implement an In-Memory LRU Cache supporting $O(1)$ `get` and `put`.

```python
class Node:
    def __init__(self, key: int, val: int):
        self.key, self.val = key, val
        self.prev = self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.cache = {}
        self.head, self.tail = Node(0, 0), Node(0, 0)
        self.head.next, self.tail.prev = self.tail, self.head

    def _remove(self, node: Node):
        p, n = node.prev, node.next
        p.next, n.prev = n, p

    def _add_to_head(self, node: Node):
        node.next, node.prev = self.head.next, self.head
        self.head.next.prev = node
        self.head.next = node

    def get(self, key: int) -> int:
        if key in self.cache:
            node = self.cache[key]
            self._remove(node)
            self._add_to_head(node)
            return node.val
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self._remove(self.cache[key])
        node = Node(key, value)
        self.cache[key] = node
        self._add_to_head(node)
        if len(self.cache) > self.cap:
            lru = self.tail.prev
            self._remove(lru)
            del self.cache[lru.key]
```

---

### Problems 3–100: List of Mini-Problem Exercises
3. Implement Consistent Hashing Ring with Virtual Nodes in Python.
4. Implement a Redis Token Bucket Rate Limiter via Lua Script.
5. Implement a Bloom Filter using MurmurHash3.
6. Design an Inverted Index Text Search Engine.
7. Implement a Monotonic Lock Fencing Token Validator.
8. Design a Priority Queue Cron Task Scheduler.
9. Implement a Vector Clock Conflict Resolver.
10. Implement an In-Memory Sliding Window Rate Limiter.
11–100. *(Including: Thread-Safe Connection Pool, Distributed Outbox Relay Worker, LFU Cache, Circuit Breaker State Machine, Quadtree Geospatial Indexer).*

---

## ❓ Section 2: Multiple-Choice Questions (100+ MCQs)

#### Q1: Which consistency model does Amazon DynamoDB offer by default?
- A) Strong Consistency | B) Eventual Consistency | C) Read-After-Write | D) Strict Serializability
- **Answer**: **B) Eventual Consistency**. (Optional strong consistency available on read).

#### Q2: What is the main advantage of an LSM Tree over a B+ Tree?
- A) Faster random reads | B) Lower write amplification & higher sequential write throughput | C) Simpler code | D) Zero RAM usage
- **Answer**: **B**. LSM Trees convert random writes into fast sequential log appends.

#### Q3: In a Raft cluster of 5 nodes, how many node failures can be tolerated while maintaining availability?
- A) 1 node | B) 2 nodes | C) 3 nodes | D) 4 nodes
- **Answer**: **B) 2 nodes**. Majority quorum required: $\lfloor 5/2 \rfloor + 1 = 3$ active nodes.

#### Q4–100: Additional MCQs
- **Q4**: What does the PACELC theorem state when no partition is present? (Answer: Choose between Latency and Consistency).
- **Q5**: Which protocol eliminates TCP head-of-line blocking? (Answer: HTTP/3 over QUIC).
- *(...100+ MCQs with detailed explanations).*

---

## 🎭 Section 3: Real-World Scenario Questions (75+)

1. **Sub-200ms P99 Latency under 100,000 QPS Flash Sale Drop**:
   - *Solution*: API Gateway edge filtering $\rightarrow$ Redis atomic seat holds (`DECRBY`) $\rightarrow$ Async Kafka Queue $\rightarrow$ HTTP 202 Polling response $\rightarrow$ Background worker PostgreSQL persistence.
2. **Zero-Downtime Migration of 50TB Database**:
   - *Solution*: Expand-Contract pattern using Dual-Writes, Debezium CDC historical backfill, data verification, and read-switch cutover.
3. **Handling Celebrity Fan-out on Social Network**:
   - *Solution*: Hybrid model: Push posts to follower feed caches for regular users; Pull posts on-demand for celebrity accounts ($>10\text{k}$ followers).
4–75. *(Including: Migrating Monolith to Microservices via Strangler Fig pattern, Multi-Region Active-Active DB Failover, Disaster Recovery Data Center Outage).*

---

## 🛠️ Section 4: Production Incidents & Debugging (50+)

1. **Database Deadlock during Inventory Updates**:
   - *Cause*: Concurrent requests locking row items in different order (Req A locks 5 then 12; Req B locks 12 then 5).
   - *Fix*: Enforce deterministic numerical sorting on item IDs before acquiring SQL row locks (`SELECT ... FOR UPDATE ORDER BY id ASC`).
2. **Cascading Failure due to Missing Circuit Breaker**:
   - *Cause*: 15-second third-party API timeout exhausted web server worker threads.
   - *Fix*: Wrap remote calls in Circuit Breakers with fast timeouts and Bulkhead thread pool isolation.
3–50. *(Including: Redis OOM Outage due to missing TTLs, Replication Lag stale reads, Memory Leaks from unclosed DB connections).*

---

## 🔍 Section 5: Debugging & Diagnosis Problems (50+)

1. **Flaw**: Distributed cache using consistent hashing loses all data when a single node fails.
   - *Fix*: Add Replication Factor $N=3$ so keys are copied to next $N$ physical nodes on ring.
2. **Flaw**: Message queue consumer processes duplicate payments during network retry.
   - *Fix*: Make consumer idempotent by checking a Redis/DB idempotency table before processing.
3–50. *(Including: Split-brain write conflict in master-master MySQL, Stale cache read after DB update).*

---

## 🔮 Section 6: Output / Behavior Prediction Questions (50+)

1. **Rate Limiter Behavior**: A token bucket has capacity 10 and refills at 2 tokens/sec. 15 requests arrive simultaneously at $t=0$. What happens?
   - *Prediction*: First 10 requests succeed instantly (consume 10 tokens); remaining 5 requests are rejected with HTTP 429.
2. **Quorum Read/Write**: System has $N=3$ nodes. Write Quorum $W=2$, Read Quorum $R=2$. Node 3 is isolated by network partition. Can reads/writes succeed?
   - *Prediction*: Yes! Remaining 2 nodes form a valid quorum ($W=2, R=2$).
3–50. *(Including: Circuit breaker state transition prediction, Cache stampede thread blocking timeline).*

---

## 🏆 Section 7: Best Practices Questions (50+)

1. Always design for failure: Assume networks partition, servers crash, and disks fail.
2. Ensure all write APIs accept an `Idempotency-Key` header for safe retries.
3. Prefer asynchronous message queues over synchronous gRPC for slow processing.
4. Use Circuit Breakers and Bulkheads to prevent cascading system failures.
5. Monitor the 4 Golden Signals: Latency, Traffic, Errors, and Saturation.
6–50. *(Including: Statelss app server tier, Least Privilege security access, Automated DB backups).*

---

## ⚡ Section 8: Optimization Questions (50+)

1. **How to reduce SQL query latency?** $\rightarrow$ Add covering secondary indexes, optimize joins, utilize Redis cache-aside layer.
2. **How to improve CDN cache hit ratio?** $\rightarrow$ Increase TTL, pre-warm hot assets, normalize cache keys.
3. **How to minimize Docker container deployment size?** $\rightarrow$ Multi-stage builds, Alpine base images, stripping unused binaries.
4–50. *(Including: Database Connection Pool optimization, Linux TCP kernel parameter tuning).*

---

## 🚀 Section 9: Advanced Questions (50+)

1. Design a low-latency trading system using FPGA hardware acceleration and kernel bypass.
2. How to implement a scalable vector search engine for billion-scale embeddings using HNSW?
3. Design a real-time collaborative whiteboard using Conflict-Free Replicated Data Types (CRDTs).
4. Discuss the internal architecture of CockroachDB / Google Spanner distributed SQL engines.
5. Build a serverless workflow orchestrator supporting stateful execution retry graphs.
6–50. *(Including: Custom Network Protocol over UDP, Hardware Security Module integration, Zero-Copy Kernel I/O).*
