# 📚 Curated System Design Resources & References

*Handpicked, high-quality whitepapers, books, lectures, repositories, and interactive tools for mastering distributed systems and system design interviews.*

---

## 📜 1. Classic Distributed Systems Whitepapers

Every Senior & Staff Engineer candidate should read these landmark papers to understand foundational distributed system concepts directly from the pioneers.

| Paper Title | Authors / Organization | Why It Is Worth Reading |
| :--- | :--- | :--- |
| **[Dynamo: Amazon's Highly Available Key-Value Store](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp07.pdf)** | Werner Vogels et al. (Amazon) | The foundational paper behind NoSQL DBs. Teaches consistent hashing, vector clocks, quorum consensus, hinted handoff, and Merkle trees. |
| **[Spanner: Google's Globally-Distributed Database](https://research.google/pubs/pub39966/)** | James Corbett et al. (Google) | Shows how external consistency is achieved at global scale using TrueTime API, atomic clocks, GPS receivers, and 2PC + Paxos. |
| **[In Search of an Understandable Consensus Algorithm (Raft)](https://raft.github.io/raft.pdf)** | Diego Ongaro & John Ousterhout (Stanford) | Essential guide to distributed consensus. Far easier to understand than Paxos; explains leader election, log replication, and cluster safety. |
| **[The Google File System (GFS)](https://research.google/pubs/pub51/)** | Sanjay Ghemawat et al. (Google) | Landmarked distributed block storage design. Explains master node metadata management, chunk servers, and append-heavy workloads. |
| **[Kafka: a Distributed Messaging System for Log Processing](https://net.pku.edu.cn/~tongwe/CS199/Kafka.pdf)** | Jay Kreps et al. (LinkedIn) | Teaches distributed commit logs, page cache utilization, zero-copy network reads, and consumer offset tracking. |
| **[Bigtable: A Distributed Storage System for Structured Data](https://research.google/pubs/pub27898/)** | Chang et al. (Google) | Explains LSM trees, SSTables, MemTables, and sparse wide-column storage. |

---

## 📖 2. Recommended Books

### 1. **Designing Data-Intensive Applications (DDIA)** – *Martin Kleppmann*
- **Why Read It**: Known as the "Bible of Distributed Systems". Absolutely mandatory for SDE-2 and Senior Engineers.
- **Key Chapters**:
  - Chapter 3: Storage Engines (B-Trees vs LSM Trees)
  - Chapter 5: Replication (Leader, Multi-Leader, Leaderless)
  - Chapter 6: Partitioning & Sharding
  - Chapter 7–9: Transactions, CAP, Consensus & Linearizability

### 2. **System Design Interview – An Insider's Guide (Vol 1 & 2)** – *Alex Xu*
- **Why Read It**: The best step-by-step guide for whiteboarding and interview execution. Excellent block diagrams, capacity estimates, and clear API design workflows.

### 3. **Building Microservices (2nd Edition)** – *Sam Newman*
- **Why Read It**: Teaches microservice decomposition, service boundaries, Saga transactions, API gateways, and async event-driven patterns.

---

## 🎓 3. University Courses & Video Lectures

- **[MIT 6.824: Distributed Systems](https://www.youtube.com/playlist?list=PLrw6a1wE39ggMaYeDXULTad5UDA51tMTl)** (Prof. Robert Morris)
  - The single best free video course on distributed systems. Includes lab assignments in Go (building Raft consensus from scratch).
- **[CMU 15-445/645: Database Systems](https://15445.courses.cs.cmu.edu/)** (Prof. Andy Pavlo)
  - Deep dive into database internal architectures (B+ Trees, Query Execution, Concurrency Control, Logging & Recovery).
- **[System Design Interview Channel (ByteByteGo)](https://www.youtube.com/@ByteByteGo)**
  - Clear visual animated breakdowns of real-world architecture patterns.

---

## 🛠️ 4. GitHub Repositories & Interactive Tools

- **[donnemartin/system-design-primer](https://github.com/donnemartin/system-design-primer)**
  - Over 250k stars. Comprehensive open-source guide covering all core components and flashcards.
- **[Raft Consensus Interactive Visualizer](https://raft.github.io/)**
  - Interactive visualization of leader election, log replication, and partition failure scenarios in Raft.
- **[Kafka Internals & Architecture Visualizer](https://softwaremill.com/kafka-visualized/)**
  - Visual tool to experiment with topic partitioning, consumer group rebalancing, and offsets.

---

## 🌐 5. Engineering Blogs to Follow

1. **[Netflix TechBlog](https://netflixtechblog.com/)**: Microservice resiliency, chaos engineering, CDN edge performance.
2. **[Uber Engineering Blog](https://www.uber.com/blog/engineering/)**: Geospatial indexing (H3), real-time dispatch, Ringpop.
3. **[Meta Engineering](https://engineering.fb.com/)**: TAO graph store, Memcached at scale, feed ranking pipelines.
4. **[AWS Architecture Blog](https://aws.amazon.com/blogs/architecture/)**: Cloud native patterns, serverless scaling, multi-region deployments.
