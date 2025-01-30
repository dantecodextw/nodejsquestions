---

## **Node.js Questions**

### **1. What is the Event Loop in Node.js, and how does it work?**
#### **Expected Answer:**
- The **event loop** is the core mechanism that allows Node.js to handle asynchronous operations efficiently.
- It operates in **phases** (Timers, I/O callbacks, Idle, Poll, Check, Close callbacks).
- It works on a **single-threaded, non-blocking** architecture using **libuv**.
- Example: Handling multiple concurrent requests without blocking.

**Follow-up:** How does the event loop behave differently when working with `setTimeout()` and `fs.readFile()`?

---

### **2. How does Node.js handle concurrency if it’s single-threaded?**
#### **Expected Answer:**
- Uses an **asynchronous, non-blocking** model with **callbacks, Promises, and async/await**.
- Delegates I/O operations (file system, network) to **worker threads (via libuv)**.
- Uses **worker threads** for CPU-intensive tasks.

**Follow-up:** When would you use Worker Threads in Node.js?

---

### **3. What is the difference between process.nextTick() and setImmediate()?**
#### **Expected Answer:**
- `process.nextTick()`: Executes after the current operation but before the event loop continues.
- `setImmediate()`: Executes in the **Check phase** of the event loop.
- Example:
  ```javascript
  process.nextTick(() => console.log("nextTick"));
  setImmediate(() => console.log("setImmediate"));
  console.log("Main");
  ```
  **Output:** 
  ```
  Main
  nextTick
  setImmediate
  ```
- **Key Concept:** `nextTick()` should be used cautiously as excessive usage can block the event loop.

---

### **4. How do you manage memory leaks in a Node.js application?**
#### **Expected Answer:**
- **Identifying memory leaks** using tools like **Chrome DevTools, Node.js heap snapshots, and memory profiling**.
- Common sources:
  - **Global variables** retaining references.
  - **Event listeners** not being removed.
  - **Closures capturing unnecessary objects**.
- Fixes:
  - Use `WeakMap`/`WeakSet` for weak references.
  - Remove event listeners properly (`removeListener`).
  - Use `heapdump` to analyze memory.

---

### **5. How would you design a high-performance API in Node.js?**
#### **Expected Answer:**
- Use **caching** (Redis, in-memory caching).
- Use **load balancing** (Nginx, AWS ALB).
- Optimize database queries (Indexes, Pagination).
- Use **asynchronous processing** (RabbitMQ, Kafka, SQS).
- Apply **rate limiting** (Redis, API Gateway).
- Optimize serialization (use **MessagePack instead of JSON** for heavy payloads).

---

## **DevOps & AWS Questions**

### **6. What is the difference between Docker and Kubernetes?**
#### **Expected Answer:**
- **Docker**: Containerization platform (build, ship, run applications in containers).
- **Kubernetes**: Orchestration platform for managing containerized workloads.
- Kubernetes features:
  - **Scaling** (auto-scaling pods).
  - **Load balancing** (Service, Ingress).
  - **Self-healing** (Restart failed pods).
- **Analogy:** Docker = Containers, Kubernetes = Fleet Manager.

**Follow-up:** How do you deploy a Node.js app using Kubernetes?

---

### **7. How does AWS Auto Scaling work?**
#### **Expected Answer:**
- Auto Scaling dynamically **adjusts the number of EC2 instances** based on demand.
- Components:
  - **Launch configuration** (Instance type, AMI).
  - **Auto Scaling Group (ASG)** (Manages EC2 lifecycle).
  - **Scaling Policies** (CPU utilization, Request count).
- Works with **Elastic Load Balancer (ELB)** for traffic distribution.

**Follow-up:** How would you handle sudden traffic spikes in AWS for a Node.js API?

---

### **8. How do you secure a Node.js API in AWS?**
#### **Expected Answer:**
- **Authentication**: Use AWS Cognito, JWT.
- **Authorization**: IAM policies, Role-based Access Control (RBAC).
- **DDoS Protection**: AWS Shield, WAF.
- **Data Encryption**: KMS (Key Management Service).
- **Network Security**: Use **VPC, Security Groups, Private Subnets**.

---

## **MongoDB & SQL Questions**

### **9. How does MongoDB handle transactions, and when would you use them?**
#### **Expected Answer:**
- MongoDB supports **ACID transactions** (since v4.0) using `session.startTransaction()`.
- Use cases:
  - **Financial transactions** (ensuring consistency).
  - **Multi-document updates** (when referential integrity is needed).
- Transactions impact performance due to **locking mechanisms**.

**Follow-up:** How do MongoDB transactions differ from SQL transactions?

---

### **10. How do you optimize queries in MongoDB?**
#### **Expected Answer:**
- **Indexes** (`createIndex()` to speed up lookups).
- **Aggregation Framework** (Efficiently process large data sets).
- **Sharding** (Distribute data across multiple nodes).
- **Avoid $where queries** (as they are slow and block indexes).
- **Use projection (`find({}, { field: 1 })`)** to retrieve only necessary fields.

**Follow-up:** How do you analyze MongoDB query performance?

---

### **11. What are the differences between SQL and NoSQL databases?**
#### **Expected Answer:**
| Feature | SQL (MySQL, PostgreSQL) | NoSQL (MongoDB) |
|---------|-------------------------|----------------|
| **Schema** | Fixed schema (tables, columns) | Flexible schema (documents) |
| **Scaling** | Vertical scaling (scale-up) | Horizontal scaling (sharding) |
| **Transactions** | Strong ACID compliance | Eventual consistency, ACID supported (MongoDB 4+) |
| **Use case** | Structured data (ERP, banking) | Unstructured data (IoT, logs, JSON storage) |

---

### **12. How does indexing work in SQL, and when should you use it?**
#### **Expected Answer:**
- **Indexing improves query performance** by reducing lookup time.
- Types:
  - **Primary Index** (Clustered Index).
  - **Secondary Index** (Non-clustered Index).
  - **Full-Text Index** (Used in search operations).
- **Downside:** Too many indexes slow down writes.

**Follow-up:** What is the difference between a clustered and non-clustered index?

---

## **Scenario-Based Questions**

### **13. If a Node.js API using MongoDB starts slowing down under high load, how would you debug and optimize it?**
#### **Expected Answer:**
- **Check query performance** using `db.collection.explain()`.
- **Index Optimization**: Ensure correct indexing strategy.
- **Connection Pooling**: Increase MongoDB pool size.
- **Caching**: Use Redis for frequently accessed data.
- **Sharding**: Distribute load across multiple nodes.
- **Database Writes**: Use **Bulk Writes** to reduce latency.

---

### **14. You need to deploy a high-availability Node.js application on AWS. What architecture would you use?**
#### **Expected Answer:**
- **Load Balancer (ALB)** distributes traffic.
- **Auto Scaling Group (ASG)** scales EC2 instances.
- **RDS (for SQL) / DynamoDB (for NoSQL)** for database storage.
- **Redis / ElastiCache** for caching.
- **S3 + CloudFront** for static assets.
- **CloudWatch & Logging** for monitoring.

---

## **Advanced MongoDB Questions**

### **1. Explain MongoDB Sharding and its Architecture.**
#### **Expected Answer:**
- **Sharding** is MongoDB’s strategy to **horizontally scale** large databases by distributing data across multiple nodes.
- **Components of Sharding:**
  - **Shard Servers**: Store actual data.
  - **Config Servers**: Store metadata and sharding configuration.
  - **Mongos**: Acts as a query router.
- **Shard Key Selection:**
  - Should ensure **even distribution** (Avoid hotspot issues).
  - Avoid **monotonically increasing fields** (like timestamps, ObjectId).
- **Sharding Strategies:**
  - **Range-Based Sharding**: Divides data into contiguous chunks.
  - **Hashed Sharding**: Hashes shard key to evenly distribute data.
  - **Zone-Based Sharding**: Allows specific data to be stored in specific shards.

**Follow-up:** What are the major challenges with sharding in MongoDB?

---

### **2. What happens when a shard goes down in a sharded MongoDB cluster?**
#### **Expected Answer:**
- If **replica sets are enabled**, the **secondary node** becomes primary.
- If **no replication**, that part of data becomes **inaccessible**.
- Queries on missing shard **fail unless partial reads are allowed (`readConcern: "available"`)**.
- **Rebalancing** occurs when a new shard is added or a downed shard is removed.

**Follow-up:** How does MongoDB prevent data loss in sharding?

---

### **3. Explain the Aggregation Framework in MongoDB and compare it with SQL GROUP BY.**
#### **Expected Answer:**
- **Aggregation Framework** is a powerful tool to process and transform data.
- Works like a **pipeline** with multiple stages:
  - `$match`: Filters documents (like `WHERE` in SQL).
  - `$group`: Groups data (like `GROUP BY` in SQL).
  - `$project`: Reshapes output fields.
  - `$lookup`: Performs **left outer joins** (similar to SQL `JOIN`).
  - `$unwind`: Decomposes arrays into multiple documents.
  - `$sort`, `$limit`, `$skip`: Used for pagination.

| Feature | SQL (`GROUP BY`) | MongoDB (`$group`) |
|---------|-----------------|--------------------|
| **Use Case** | Summarizing rows | Aggregating documents |
| **Operations** | SUM, COUNT, AVG, MAX, MIN | `$sum`, `$count`, `$avg`, `$max`, `$min` |
| **Joins** | Uses `JOIN` | Uses `$lookup` |
| **Performance** | Indexes improve speed | Works in pipeline stages |

**Follow-up:** How does aggregation affect query performance? How do you optimize it?

---

### **4. How would you design a large-scale real-time analytics system using MongoDB?**
#### **Expected Answer:**
- **Use Aggregation Pipelines for real-time analytics.**
- **Precompute data** and store it in a **summary collection** to avoid expensive queries.
- **Sharding** ensures **load distribution**.
- Use **TTL Indexes** to delete old logs automatically.
- Use **Change Streams** for **real-time event tracking**.

**Follow-up:** How does MongoDB compare to Apache Kafka or Elasticsearch for real-time analytics?

---

## **Advanced Node.js Questions**

### **5. Explain Backpressure in Node.js Streams and How to Handle It.**
#### **Expected Answer:**
- **Backpressure** occurs when a **readable stream** produces data **faster than the writable stream** can consume.
- Solutions:
  - **Use `pipe()` with throttling**.
  - **Use `pause()` and `resume()` methods**.
  - **Handle drain events properly**:
    ```javascript
    writableStream.on("drain", () => readableStream.resume());
    ```
- Real-world Example: Streaming a large file to an HTTP response.

**Follow-up:** How does backpressure impact API performance?

---

### **6. How would you prevent DDoS attacks in a Node.js API?**
#### **Expected Answer:**
- **Rate Limiting** (using Redis and libraries like `express-rate-limit`).
- **IP Whitelisting & Blacklisting**.
- **Reverse Proxies (Nginx, Cloudflare)** to mitigate traffic surges.
- **Use AWS WAF (Web Application Firewall)** for additional protection.

**Follow-up:** How do you handle API security in a microservices architecture?

---

## **Advanced AWS & DevOps Questions**

### **7. How does AWS Lambda scale, and what are its limitations?**
#### **Expected Answer:**
- AWS Lambda **scales automatically** by increasing concurrent executions.
- **Cold Start Problem**: If a function isn’t invoked for a while, AWS **deallocates it**, causing latency on the next execution.
- **Limitations:**
  - **Execution Time Limit:** 15 minutes per execution.
  - **Memory Limit:** 10 GB.
  - **Concurrency Limit:** By default, 1000 concurrent executions per region.

**Follow-up:** How do you reduce cold start issues in AWS Lambda?

---

### **8. Explain the difference between AWS EC2 Auto Scaling and AWS Fargate.**
#### **Expected Answer:**
| Feature | EC2 Auto Scaling | AWS Fargate |
|---------|-----------------|-------------|
| **Management** | Requires managing EC2 instances | Serverless, fully managed |
| **Scaling** | Auto scales EC2 instances | Auto scales containers |
| **Use case** | Applications needing OS control | Serverless microservices |
| **Cost model** | Pay for EC2 uptime | Pay for actual container runtime |

**Follow-up:** When would you prefer AWS Lambda over Fargate?

---

## **Advanced SQL Questions**

### **9. What are Common Table Expressions (CTE) and Recursive Queries in SQL?**
#### **Expected Answer:**
- **CTEs (`WITH` queries)** allow complex queries to be structured more readably.
- **Recursive CTEs** are useful for hierarchical data (like category trees).

Example:
```sql
WITH RECURSIVE category_tree AS (
  SELECT id, name, parent_id FROM categories WHERE parent_id IS NULL
  UNION ALL
  SELECT c.id, c.name, c.parent_id FROM categories c
  INNER JOIN category_tree ct ON c.parent_id = ct.id
)
SELECT * FROM category_tree;
```
- **Use Cases:** Org hierarchies, folder structures.

**Follow-up:** What is the performance impact of using CTEs?

---

### **10. Explain the CAP theorem and how it applies to SQL vs. NoSQL.**
#### **Expected Answer:**
- **CAP Theorem:** A distributed system can only guarantee **two** out of **Consistency, Availability, Partition Tolerance**.
- **SQL (RDBMS)**:
  - Prioritizes **Consistency & Availability** (CA).
  - Example: PostgreSQL in single-node mode.
- **NoSQL (MongoDB, Cassandra)**:
  - Prioritizes **Availability & Partition Tolerance** (AP).
  - Example: MongoDB in a sharded cluster.

**Follow-up:** How does MongoDB handle consistency in a distributed environment?

---

## **Scenario-Based Questions**

### **11. Your Node.js API is slowing down due to high database load. How do you troubleshoot and optimize it?**
#### **Expected Answer:**
- **Analyze Slow Queries** (`EXPLAIN ANALYZE`, MongoDB Profiler).
- **Index Optimization** (Proper composite indexes).
- **Connection Pooling** (`pg-pool` for PostgreSQL, `useUnifiedTopology` for MongoDB).
- **Use Caching** (Redis, Memcached).
- **Optimize Queries** (Avoid `SELECT *`, minimize joins).
- **Implement Read Replicas** (for SQL, MongoDB replica sets).

**Follow-up:** How would you design a caching strategy for an e-commerce API?

---

### **12. How would you migrate a large MySQL database to MongoDB?**
#### **Expected Answer:**
- **Analyze Schema**: Map **tables to collections**, **rows to documents**.
- **Denormalize Data**: Embed where needed.
- **Use Migration Tools** (ETL processes, `mongoimport`, `mongodump`).
- **Ensure Data Integrity**: Validate counts before switching.
- **Performance Testing**: Compare query speed and optimize indexes.

---

These **advanced questions** will deeply evaluate the candidate’s expertise across **Node.js, MongoDB, AWS, DevOps, and SQL**. Let me know if you need further refinements!
