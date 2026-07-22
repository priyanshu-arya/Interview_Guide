# 🧪 System Design Practice Questions, MCQs & Scenarios

*An exhaustive hands-on problem bank featuring Concept Questions, Coding Implementations, Debugging Challenges, Output Predictions, Scenario-Based Problems, Tricky Questions, Practical Problems, and Senior Interview Challenges.*

---

# 🧠 Section 1: Concept Questions

---

### Concept Question 1: Explain PACELC Theorem vs CAP Theorem
- **Difficulty**: Intermediate
- **Hint**: Consider what happens when there is NO network partition in a distributed system.
- **Expected Approach**:
  - State CAP theorem: Under network Partition ($P$), choose between Consistency ($C$) and Availability ($A$).
  - Explain PACELC extension: **If Partition ($P$)**, trade off Availability ($A$) vs Consistency ($C$); **Else ($E$)**, trade off Latency ($L$) vs Consistency ($C$).
  - Provide database examples: MongoDB (CP / PC/EC), Cassandra (AP / PA/EL), Spanner (CP / PC/EC).
- **Common Mistakes**: Claiming CAP theorem applies during normal non-partitioned operation.

---

### Concept Question 2: LSM-Tree vs B+ Tree Storage Engine Write Amplification
- **Difficulty**: Advanced
- **Hint**: Analyze how random disk writes are converted into sequential writes in LSM trees.
- **Expected Approach**:
  - Contrast B+ Tree random page modifications (high write amplification due to in-place updates) with LSM-Tree append-only writes (WAL + MemTable).
  - Detail LSM compaction levels (L0 to L1) and trade-off with read amplification (requiring Bloom Filters).
- **Common Mistakes**: Believing LSM trees provide faster random point reads than B+ Trees.

---

# 💻 Section 2: Coding Questions

---

### Coding Question 1: Implement Twitter Snowflake 64-Bit Unique ID Generator
- **Difficulty**: Medium
- **Hint**: Use bit shifting to combine timestamp offset, datacenter ID, worker ID, and sequence number into a single 64-bit integer.
- **Expected Approach**:

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
        self.twepoch = 1767225600000 # Custom Epoch (2026-01-01)

    def _current_millis(self) -> int:
        return int(time.time() * 1000)

    def generate_id(self) -> int:
        timestamp = self._current_millis()
        if timestamp < self.last_timestamp:
            raise Exception("Clock moved backwards! Refusing to generate ID.")
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

- **Common Mistakes**: Not handling NTP clock skew going backwards, causing duplicate ID generation.

---

### Coding Question 2: Implement an O(1) Thread-Safe LRU Cache
- **Difficulty**: Medium
- **Hint**: Combine a Hash Map for $O(1)$ key lookups with a Doubly Linked List for $O(1)$ eviction node re-ordering.
- **Expected Approach**:

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

- **Common Mistakes**: Forgetting to update linked list pointers when modifying an existing key's value.

---

### Coding Question 3: Implement Consistent Hashing Ring with Virtual Nodes
- **Difficulty**: Hard
- **Hint**: Use MD5 hashing to map both physical server v-nodes and user keys onto a sorted ring structure.
- **Expected Approach**: Implement binary search (`bisect`) over sorted v-node hash values to locate the designated server moving clockwise.
- **Common Mistakes**: Assigning only 1 node entry on ring, leading to severe load imbalance when nodes join/leave.

---

# 🐞 Section 3: Debugging Questions

---

### Debugging Question 1: Inventory Overselling During Flash Sale
- **Problem Statement**: An e-commerce platform sold 1,250 units of an item during a flash sale even though stock was strictly capped at 1,000. Web server logs show high concurrent HTTP `POST /checkout` requests.
- **Difficulty**: Medium
- **Hint**: Look for non-atomic read-then-write database operations (`SELECT count ... THEN UPDATE`).
- **Expected Approach**:
  - Identify race condition where multiple threads read `count = 1` simultaneously before any thread executes the `UPDATE`.
  - Fix by enforcing atomic Redis decrements (`DECRBY item_stock 1`) or atomic SQL conditional updates (`UPDATE items SET stock = stock - 1 WHERE id = 101 AND stock > 0`).
- **Common Mistakes**: Wrapping the non-atomic check in an application-level lock that only works within a single server instance.

---

### Debugging Question 2: Cascading Web Server Collapse Under Third-Party API Outage
- **Problem Statement**: When an external payment Gateway API experienced a 15-second response delay, the entire main web application crashed with 504 Gateway Timeouts for ALL endpoints (even non-payment endpoints).
- **Difficulty**: Medium
- **Hint**: Check web server thread pool exhaustion.
- **Expected Approach**:
  - Analyze thread pool saturation: incoming requests blocked web worker threads waiting synchronously for 15s payment API responses, starving other endpoints of free worker threads.
  - Fix by implementing **Circuit Breakers** (fast fail after 5 consecutive timeouts), setting aggressive 2-second remote HTTP timeouts, and isolating payment calls to a dedicated **Bulkhead thread pool**.
- **Common Mistakes**: Increasing web server worker thread limits without setting call timeouts, causing severe OOM crashes.

---

# 🔮 Section 4: Output Prediction Questions

---

### Output Prediction 1: Token Bucket Rate Limiter Under Concurrent Request Spike
- **Problem Statement**: A Token Bucket rate limiter is configured with capacity $C = 10$ tokens and a refill rate $R = 2 \text{ tokens/second}$. The bucket is completely full at $t = 0$. At $t = 0.0\text{s}$, a spike of 15 requests arrives simultaneously. What is the status of each request?
- **Difficulty**: Medium
- **Hint**: Calculate available tokens at $t = 0$.
- **Expected Approach**:
  - First 10 requests instantly consume the 10 available tokens $\rightarrow$ Status: **200 OK**.
  - Remaining 5 requests find bucket empty ($0$ tokens left) $\rightarrow$ Status: **429 Too Many Requests**.
  - At $t = 1.0\text{s}$, 2 new tokens refill the bucket $\rightarrow$ Next 2 incoming requests succeed.
- **Common Mistakes**: Assuming tokens refill continuously mid-millisecond to satisfy all 15 requests immediately.

---

### Output Prediction 2: Quorum Read/Write Consistency Scenario
- **Problem Statement**: A distributed database has $N = 5$ replicas across nodes. Write Quorum is configured to $W = 3$, and Read Quorum to $R = 2$. A network partition isolates 2 nodes. Can client writes and reads succeed?
- **Difficulty**: Medium
- **Hint**: Apply the $W + R > N$ strong consistency equation.
- **Expected Approach**:
  - Check Partition 1 (3 active nodes): Reaches Write Quorum ($3 \ge W$) and Read Quorum ($3 \ge R$) $\rightarrow$ Writes and Reads **Succeed**.
  - Check Partition 2 (2 isolated nodes): Cannot reach Write Quorum ($2 < W$) $\rightarrow$ Writes **Fail** (or rejected).
  - Strong consistency check: $W + R = 3 + 2 = 5 \implies W + R > N$ is NOT strictly $> N$ ($5 \ngtr 5$), meaning stale reads are theoretically possible if reads miss updated nodes.
- **Common Mistakes**: Conflating total node count with active node count in partitioned sub-clusters.

---

# 🎭 Section 5: Real-World Scenario Questions

---

### Real-World Scenario 1: Zero-Downtime Migration of 50TB SQL Database
- **Problem Statement**: Migrate a 50TB production MySQL table with 50,000 QPS to PostgreSQL without taking any downtime or losing data.
- **Difficulty**: Hard
- **Hint**: Use the Expand-Contract pattern with Change Data Capture (CDC).
- **Expected Approach**:
  1. *Dual Writes*: Deploy code that writes new/updated records to both MySQL and PostgreSQL simultaneously.
  2. *Historical Backfill*: Run a background CDC pipeline (Debezium + Kafka) backfilling historical rows created prior to dual writes.
  3. *Replication Catch-up & Verification*: Verify row counts, checksums, and shadow read comparisons.
  4. *Read Cutover*: Switch read traffic to PostgreSQL via feature flag.
  5. *Contract*: Stop dual writing to MySQL and deprecate old DB.
- **Common Mistakes**: Attempting to take a database dump lock during live peak production traffic.

---

# ⚡ Section 6: Tricky Questions

---

### Tricky Question 1: What Happens When NTP Clock Shifts Backward by 5 Seconds on a Snowflake Worker Node?
- **Problem Statement**: A server's System Clock is synchronized via NTP. NTP detects drift and adjusts the clock backwards by 5,000 milliseconds while a Snowflake ID generator is actively generating IDs.
- **Difficulty**: Hard
- **Hint**: Timestamps are a major component of 64-bit Snowflake IDs.
- **Expected Approach**:
  - If unhandled, generating IDs with a smaller timestamp causes **duplicate IDs** or breaks monotonic ordering.
  - *Fix*: Worker node detects `current_timestamp < last_timestamp`, immediately throws an exception or pauses ID generation until system clock catches up to `last_timestamp`.
- **Common Mistakes**: Ignoring system clock adjustments and generating IDs with out-of-order timestamps.

---

# 🛠️ Section 7: Practical Problems

---

### Practical Problem 1: Design an API Gateway Rate Limiter with Redis Lua Scripting
- **Problem Statement**: Design an API gateway rate limiter that limits users to 100 requests per minute per IP address without race conditions across 20 web instances.
- **Difficulty**: Medium
- **Hint**: Execute token checking and decrementing inside an atomic Redis Lua Script.
- **Expected Approach**:
  - Write a Lua script that executes atomically inside Redis:
    ```lua
    local current = redis.call('INCR', KEYS[1])
    if current == 1 then
        redis.call('EXPIRE', KEYS[1], ARGV[1])
    end
    if current > tonumber(ARGV[2]) then
        return 0
    end
    return 1
    ```
  - API gateway executes script with key `rate:ip_address`, TTL 60s, limit 100.
- **Common Mistakes**: Performing separate `GET` and `SET` Redis calls from Python web app code, creating severe race conditions under high QPS.

---

# 🏆 Section 8: Senior Interview Challenges

---

### Senior Challenge 1: Guaranteeing Exactly-Once Processing in Kafka-to-Database Pipeline
- **Problem Statement**: Design a streaming ingestion pipeline consuming from Apache Kafka and writing to PostgreSQL that guarantees **Exactly-Once Processing** semantics even if workers crash midway.
- **Difficulty**: Very Hard
- **Hint**: Combine Kafka transactional producers with idempotent database upserts.
- **Expected Approach**:
  - Enable Kafka transactional producer (`processing.guarantee=exactly_once_v2`).
  - Store Kafka consumer partition offsets inside the destination PostgreSQL database in the **same atomic SQL transaction** as the business payload write.
  - Design write statements to be idempotent using `INSERT ... ON CONFLICT (id) DO UPDATE`.
  - Alternatively, use a unique `event_id` deduplication table.
- **Common Mistakes**: Believing Kafka consumer auto-commit guarantees exactly-once database writes during worker crashes.

---

Proceed to [`Resources.md`](file:///s:/Interview_Guide/System_Design/Resources.md) for handpicked books, whitepapers, MIT courses, and interactive tools! 🚀
