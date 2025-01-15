---
layout: post
title: Before Sharding - Steps and Alternatives
subtitle: A Guide to When You Should Shard Your Database
gh-repo: Abhishake63/abhishake-guides
gh-badge: [star, fork, follow]
tags: [database, dynamodb, aws]
author: Abhishake Sen Gupta
---


### **Understand the Problem**

- Identify whether the issue is **slow reads** or **slow writes**.
- Analyze the slowest queries and understand why they are underperforming: [Analyze slow queries with this video](https://youtu.be/-qNSXK7s7_w?t=237).
- Optimize your schema and indexes before considering sharding.

### **Horizontal Partitioning**

- **Definition:** Split the database into different ranges based on the partition key (often the primary key).
    - Example: Users A-M in one range, N-Z in another.
- **Benefits:**
    - Smaller B-trees for indexes.
    - Improves query efficiency by reducing data scanned.

### **Vertical Partitioning**

- **Definition:** Separate infrequently accessed columns into a different table.
    - Example: Move metadata or logs into a separate table.
- **Benefits:**
    - Faster reads for frequent queries (as they query fewer columns).
    - Smaller B-trees reduce memory usage.
- **Tradeoff:** Slower access for queries involving the partitioned columns.

### **Master-Slave Replication (Read Optimization)**

- **Setup:**
    - Master handles **all writes**.
    - Slaves handle **all reads**.
    - Master syncs updates to slaves.
- **Benefits:**
    - Reduces read load on the master.
    - Improves scalability for read-heavy workloads.

### **Master-Master-Slave Replication (Write Optimization)**

- **Setup:**
    - Multiple **masters** handle writes across regions (e.g., EAST and WEST).
    - Slaves handle reads.
    - Masters sync with each other and push updates to slaves.
- **Benefits:**
    - Distributes write load across regions.
    - Enables regional failover and redundancy.
- **Tradeoff:**
    - Synchronization delays between masters.

### **Shard Only as a Last Resort**

- **When to Shard:**
    - When you hit database capacity limits that **partitioning and replication** cannot solve.
- **Challenges with Sharding:**
    - Adds **complex client logic** to manage shard queries.
    - Reduces ACID guarantees:
        - No transactions or isolation between shards.
    - Coupling between database and application logic.
- **Tools:** Use [Vitess](https://vitess.io/) to help manage sharding logic.

---

### **Additional Resources**

- **Partitioning Video:** [Watch here](https://www.youtube.com/watch?v=QA25cMWp9Tk).
- **Vitess Documentation:** [Learn more](https://vitess.io/).