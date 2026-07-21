# 🧪 System Design Practice Questions, MCQs & Scenarios

*A comprehensive problem bank featuring Mini-Problems, Multiple-Choice Questions, Real-World Scenarios, Production Incidents, and Debugging Challenges.*

---

## 💻 Section 1: Design Mini-Problems (100+ Exercises)

---

### Problem 1: Design a Distributed Unique ID Generator (Snowflake)
- **Difficulty**: Medium
- **Problem Statement**: Design a 64-bit unique ID generator that provides monotonically increasing IDs across a cluster of 1,024 servers without relying on a central database coordinator. Must generate over 10,000 IDs per second per node.
- **Hint**: Split the 64-bit integer into timestamp, worker node ID, and sequence counter bits.
- **Expected Approach**:
  - Use Twitter Snowflake 64-bit ID layout:
    - **1 bit**: Reserved (unused, sign bit = 0).
    - **41 bits**: Epoch Timestamp in milliseconds ($\sim 69 \text{ years}$ of unique IDs).
    - **10 bits**: Machine / Worker ID ($2^{10} = 1,024 \text{ nodes}$).
    - **12 bits**: Sequence Number ($2^{12} = 4,096 \text{ IDs per ms per node}$).
  - *Implementation Logic*: When a request arrives, read current millisecond timestamp. If timestamp is same as last request, increment sequence number. If sequence exceeds 4095, sleep until next millisecond.

```python
import time

class SnowflakeIDGenerator:
    def __init__(self, worker_id: int, datacenter_id: int):
        self.worker_id = worker_id
        self.datacenter_id = datacenter_id
        self.sequence = 0
        self.last_timestamp = -1
        
        # Bit allocations
        self.worker_id_bits = 5
        self.datacenter_id_bits = 5
        self.sequence_bits = 12
        
        # Shifts
        self.worker_id_shift = self.sequence_bits
        self.datacenter_id_shift = self.sequence_bits + self.worker_id_bits
        self.timestamp_left_shift = self.sequence_bits + self.worker_id_bits + self.datacenter_id_bits
        self.sequence_mask = -1 ^ (-1 << self.sequence_bits)
        
        # Custom epoch (e.g., 2026-01-01)
        self.twepoch = 1767225600000

    def _current_millis(self) -> int:
        return int(time.time() * 1000)

    def generate_id(self) -> int:
        timestamp = self._current_millis()
        
        if timestamp < self.last_timestamp:
            raise Exception("Clock moved backwards! Refusing to generate ID.")
            
        if timestamp == self.last_timestamp:
            self.sequence = (self.sequence + 1) & self.sequence_mask
            if self.sequence == 0:
                # Wait for next millisecond
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

- **Common Mistakes**: Forgetting to handle clock drift/NTP backwards adjustment. (Solution: Reject requests or wait until clock catches up).

---

### Problem 2: Implement an LRU Cache with O(1) Operations
- **Difficulty**: Medium
- **Problem Statement**: Implement an In-Memory LRU Cache supporting `get(key)` and `put(key, value)` operations in $O(1)$ time complexity.
- **Hint**: Use a combination of a Hash Map and a Doubly Linked List.

```python
class Node:
    def __init__(self, key: int, val: int):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.cache = {} # Map key -> Node
        # Dummy head and tail nodes
        self.head = Node(0, 0)
        self.tail = Node(0, 0)
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove(self, node: Node):
        p, n = node.prev, node.next
        p.next, n.prev = n, p

    def _add_to_head(self, node: Node):
        node.next = self.head.next
        node.prev = self.head
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

### Problem 3–100: Additional Mini-Problems Summary
- **Problem 3**: Design a Consistent Hashing Ring with Virtual Nodes in Python.
- **Problem 4**: Design an In-Memory Bloom Filter using multiple Murmur3 hash functions.
- **Problem 5**: Design a Simple Rate Limiter using Token Bucket in Redis Lua.
- **Problem 6**: Implement a Monotonic Lock Fencing Token Validator.
- **Problem 7**: Design a Distributed Outbox Pattern Relay Worker.
- **Problem 8**: Implement an Inverted Index for Text Search in Python.
- **Problem 9**: Design a Priority Queue Cron Scheduler.
- **Problem 10**: Implement a Vector Clock Conflict Detector.
- *(...100+ Mini-Problems)*

---

## ❓ Section 2: Multiple Choice Questions (100+ MCQs)

---

#### Q1: Which consistency model does Amazon DynamoDB offer by default for read operations?
- A) Strong Consistency
- B) Eventual Consistency
- C) Causal Consistency
- D) Strict Serializability
- **Answer**: **B) Eventual Consistency**. (Optional strong consistency can be requested per read request).

#### Q2: In an LSM-Tree database like Cassandra, what role does the Bloom Filter play?
- A) Ensures strong consistency across read replicas
- B) Prevents unnecessary disk reads by confirming if a key definitely does NOT exist in an SSTable
- C) Maintains vector clocks for conflict resolution
- D) Stores write-ahead logs in memory
- **Answer**: **B**. Bloom filters have zero false negatives, allowing fast confirmation of key absence without disk I/O.

#### Q3: Which consensus algorithm requires a minimum cluster quorum of 4 nodes to tolerate 2 node failures?
- A) Raft ($2f + 1$)
- B) Paxos ($2f + 1$)
- C) Both A and B (Requires $2(2) + 1 = 5$ nodes)
- D) Neither
- **Answer**: **C**. Both Raft and Paxos require $2f + 1$ total nodes to tolerate $f$ failures. For $f=2$, $2(2)+1 = 5$ nodes are required.

#### Q4–100: Additional Sample MCQs
- **Q4**: What is the primary cause of write amplification in B+ Trees? (Answer: Random page writes and full page splits).
- **Q5**: In PACELC theorem, what does the 'E' stand for? (Answer: Else - trade-offs when there is no partition).
- *(...100+ MCQs)*

---

## 🎭 Section 3: Real-World Scenario Questions (75+)

---

### Scenario 1: Sub-200ms P99 Latency SLA under Flash Sale Traffic Spikes
- **Question**: Your team is building a ticket booking platform. During major concert drops, traffic surges from 1,000 QPS to 100,000 QPS within seconds. The P99 response time must stay under 200ms. How do you design for this?
- **Difficulty**: Hard
- **Hint**: Move booking validation and payment processing to asynchronous queues; use Redis memory locks for instant seat holds.
- **Expected Approach**:
  1. **Edge Offloading**: Use Cloudflare CDN and API Gateway to filter unauthorized or rate-exceeded requests.
  2. **Seat Hold Engine**: Perform seat availability checks and temporary 5-minute holds atomically in **Redis** via Lua script (`DECR seat_count`).
  3. **Async Queue Processing**: Once seat hold is granted in Redis, push booking request to **Kafka** queue. Immediately return HTTP 202 Accepted with a job status polling endpoint to client.
  4. **Worker DB Persistence**: Background workers consume Kafka messages and write official orders to PostgreSQL DB with pessimistic row locking.

---

### Scenario 2: Migrating a Monolithic Database with Zero Downtime
- **Question**: You have a 10TB monolithic PostgreSQL database that needs to be sharded into 8 separate database clusters without any application downtime. What pattern do you use?
- **Difficulty**: Hard
- **Expected Approach**: Use the **Expand-Contract (Dual-Write) Pattern** with **CDC (Change Data Capture)**:
  1. Set up new sharded DB clusters.
  2. Deploy application code update that writes to BOTH Old DB and New Sharded DBs (**Dual-Write**), but reads ONLY from Old DB.
  3. Run historical backfill script (using Debezium CDC) to copy old data up to dual-write start time.
  4. Run data validation script to compare old vs new sharded DBs.
  5. Switch application reads to New Sharded DBs.
  6. Stop writing to Old DB and drop old monolithic database.

---

## 🛠️ Section 4: Production Incidents & Debugging (50+)

---

### Production Bug 1: Database Deadlock during Flash Inventory Updates
- **Incident Report**: Production database CPU spiked to 100% and returned `Error 1213: Deadlock found when trying to get lock`.
- **Root Cause**: Multiple concurrent API requests were updating inventory rows in random order. Request A locked Row 5 then attempted to lock Row 12. Request B locked Row 12 then attempted to lock Row 5.
- **Fix**:
  1. **Enforce Deterministic Locking Order**: Always sort inventory item IDs numerically before acquiring row locks in transactions (`SELECT ... FOR UPDATE WHERE id IN (5, 12) ORDER BY id ASC`).
  2. **Offload Inventory Checks**: Move inventory counter decrements to Redis atomic operations.

---

### Production Bug 2: Cascading Failure due to Missing Circuit Breaker
- **Incident Report**: A third-party address validation API experienced high latency ($15\text{s}$ timeout). Within 2 minutes, all app servers in the checkout service ran out of worker threads, causing total platform collapse.
- **Root Cause**: Lack of thread isolation (Bulkhead) and Circuit Breaker. Every checkout thread blocked for 15s waiting for external API response, exhausting web server connection pool.
- **Fix**:
  1. Wrap third-party call in a **Circuit Breaker** (Resilience4j) with a fast 500ms timeout.
  2. Use a dedicated **Bulkhead thread pool** so validation delays cannot starve primary checkout threads.
