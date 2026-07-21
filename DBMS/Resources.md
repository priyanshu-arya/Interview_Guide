# 📚 Handpicked DBMS Resources & Learning Material

A curated catalog of the highest-rated database books, university courses, official documentation, interactive playgrounds, and interview preparation platforms.

---

## 📖 Mandatory Reading & Books

### 1. "Designing Data-Intensive Applications" (DDIA) by Martin Kleppmann
- **Target Audience**: Mid-Level, Senior, Staff Engineers, System Design Preparation.
- **Why It's Essential**: The undisputed gold standard for understanding storage engines (B-Trees vs LSM-Trees), replication models, consensus protocols, transaction isolation anomalies, and distributed data systems.
- **Key Chapters**:
  - Chapter 3: Storage and Retrieval
  - Chapter 7: Transactions & Isolation Levels
  - Chapter 8 & 9: Distributed Systems & Consensus

### 2. "Database Internals: A Deep Dive into How Distributed Data Systems Work" by Alex Petrov
- **Target Audience**: Storage Engine Developers, Infrastructure Engineers, Advanced Candidates.
- **Why It's Essential**: Provides exhaustive technical depth on physical storage layouts, B+ Tree page organization, Write-Ahead Logs, ARIES recovery algorithm, and distributed consensus algorithms (Paxos/Raft).

### 3. "Use The Index, Luke!" by Markus Winand
- **Target Audience**: All Backend Engineers & Database Developers.
- **Why It's Essential**: Practical, concise guide explaining database indexing mechanics, cost-based optimizer decisions, composite key ordering, and how to write SARGable SQL queries across MySQL, Postgres, Oracle, and SQL Server.

---

## 🎓 Free University Courses & Lectures

### 1. CMU 15-445/645: Database Systems (Prof. Andy Pavlo - Carnegie Mellon University)
- **Format**: Free YouTube Lectures & Course Materials (BusTub C++ Database Engine Project).
- **Link**: [CMU Database Group YouTube](https://www.youtube.com/@CMUDatabaseGroup)
- **Why It's Essential**: World-class university course covering buffer pool management, B+ Tree index implementation, execution engines, and concurrency control from scratch in C++.

### 2. MIT 6.824: Distributed Systems (Prof. Robert Morris)
- **Format**: Free OpenCourseWare & YouTube Lectures.
- **Why It's Essential**: Deep dive into distributed consensus (Raft), fault tolerance, 2-Phase Commit, and Google Spanner architecture.

---

## 📄 Official Documentation & Architecture Specs

| Database Engine | Recommended Docs Section | Why Read |
| :--- | :--- | :--- |
| **PostgreSQL Docs** | [Postgres MVCC & Concurrency](https://www.postgresql.org/docs/current/mvcc.html) | Explains `xmin`/`xmax`, vacuuming, snapshot isolation, and locks in production depth. |
| **MySQL InnoDB** | [InnoDB Storage Engine Architecture](https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-engine.html) | Details page structure, buffer pool, undo/redo logs, and gap locking. |
| **CockroachDB Docs** | [Architecture Overview](https://www.cockroachlabs.com/docs/stable/architecture/overview.html) | Excellent modern explanation of Raft-based distributed SQL database architecture. |

---

## 💻 Practice Platforms & Interactive Tools

### 1. LeetCode Database Problem Set
- **Focus**: SQL Queries, Window Functions, Complex CTEs.
- **Key Problems to Master**:
  - #177: $N$-th Highest Salary
  - #185: Department Top 3 Salaries
  - #601: Human Traffic of Stadium (Island & Gap Problem)

### 2. Use The Index, Luke! SQL Quiz & Playground
- **Focus**: Testing index optimization comprehension and execution plan analysis.

### 3. Database System Architecture Visualizers
- **B+ Tree Visualization**: [USFCA Interactive B+ Tree Visualizer](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)
- **Raft Consensus Visualization**: [Raft Interactive Scope](https://raft.github.io/)

---

## 📌 Final Interview Checklist

- [ ] Reviewed `Interview_Guide.md` (Beginner $\rightarrow$ Advanced + 150+ Theory Qs across 8 Modules).
- [ ] Memorized quick revision tables, Mermaid diagrams & strategy in `Cheat_Sheet.md`.
- [ ] Solved Top 25 Repeated, Top 50 Difficult, Top 50 Rejected, and Top 50 Differentiating questions in `Top_Questions.md`.
- [ ] Studied company hiring patterns in `Company_Questions.md` (FAANG, FinTech, Unicorns).
- [ ] Practiced 100+ Coding, 100+ MCQs, 75+ Scenarios, 50+ Production Bugs, 50+ Debugging, and 50+ Tricky questions in `Practice_Questions.md`.

Good luck acing your DBMS interviews! 🚀
