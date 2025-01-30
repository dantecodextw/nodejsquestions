### **Core Node.js Concepts**
1. **Explain the Node.js event loop. How does it handle asynchronous operations?**  
   *Expected Answer*:  
   The event loop is a single-threaded, non-blocking loop that processes asynchronous callbacks. It has phases like **timers (setTimeout)**, **I/O polling**, **check (setImmediate)**, and **close callbacks**. Asynchronous tasks (e.g., I/O operations) are offloaded to the system kernel (via libuv), and their callbacks are executed in the event loop once completed, ensuring non-blocking behavior.

2. **What is the difference between `blocking` and `non-blocking` code in Node.js? Provide an example.**  
   *Expected Answer*:  
   - Blocking code halts further execution until the current operation completes (e.g., `fs.readFileSync`).  
   - Non-blocking code uses callbacks/promises to allow other operations to run (e.g., `fs.readFile` with a callback).  
   Example:  
   ```javascript
   // Blocking
   const data = fs.readFileSync('file.txt');
   console.log(data);

   // Non-blocking
   fs.readFile('file.txt', (err, data) => {
     if (err) throw err;
     console.log(data);
   });
   ```

---

### **Asynchronous Programming**
3. **How would you handle multiple concurrent asynchronous operations and aggregate their results?**  
   *Expected Answer*:  
   Use `Promise.all([...])` to execute promises in parallel and wait for all to resolve. For example:  
   ```javascript
   const [user, posts] = await Promise.all([
     fetchUser(userId),
     fetchPosts(userId)
   ]);
   ```  
   *Bonus*: Mention error handling in `Promise.allSettled` if partial failures are acceptable.

4. **What is a `callback hell`, and how do you avoid it?**  
   *Expected Answer*:  
   Callback hell refers to deeply nested callbacks, making code unreadable. Solutions include:  
   - Using **Promises** with `.then()` chaining.  
   - Using **async/await** for synchronous-like code.  
   - Modularizing code into reusable functions.

---

### **RESTful APIs & Middleware**
5. **How would you design a RESTful API for a blog (CRUD operations) using Express?**  
   *Expected Answer*:  
   Define routes like:  
   ```javascript
   app.get('/posts', getPosts);
   app.post('/posts', createPost);
   app.put('/posts/:id', updatePost);
   app.delete('/posts/:id', deletePost);
   ```  
   Use middleware like `express.json()` for parsing and `morgan` for logging.

6. **What is middleware in Express? Write a custom middleware to log request time.**  
   *Expected Answer*:  
   Middleware are functions that execute during the request-response cycle. Example:  
   ```javascript
   app.use((req, res, next) => {
     console.log(`Request at ${Date.now()}`);
     next();
   });
   ```

---

### **Authentication & Security**
7. **How does JWT authentication work in Node.js?**  
   *Expected Answer*:  
   - User logs in with credentials.  
   - Server generates a signed JWT (header, payload, signature) and sends it to the client.  
   - Client includes the JWT in subsequent requests (via headers).  
   - Server verifies the JWT signature and grants access.  
   *Bonus*: Mention storing JWT in HTTP-only cookies for security.

8. **How would you prevent SQL injection and XSS attacks in a Node.js app?**  
   *Expected Answer*:  
   - Use parameterized queries (e.g., with `pg` or `mysql2`).  
   - Sanitize user input with libraries like `validator` or `xss-clean`.  
   - Set security headers using `helmet`.

---

### **Error Handling & Debugging**
9. **How do you handle uncaught exceptions in Node.js?**  
   *Expected Answer*:  
   Use `process.on('uncaughtException', (err) => { ... })` to log errors and gracefully shut down. However, best practice is to fix the root cause and use `try/catch` with async/await.

10. **Write an error-handling middleware in Express.**  
    *Expected Answer*:  
    ```javascript
    app.use((err, req, res, next) => {
      console.error(err.stack);
      res.status(500).json({ error: 'Server error' });
    });
    ```

---

### **Performance & Scalability**
11. **How would you scale a Node.js application to handle high traffic?**  
    *Expected Answer*:  
    - Use the **cluster module** to fork child processes (utilize multiple CPU cores).  
    - Implement **caching** with Redis.  
    - Load balance using Nginx or PM2.  
    - Optimize database queries and use connection pooling.

12. **What is the purpose of `streams` in Node.js? Provide a use case.**  
    *Expected Answer*:  
    Streams process data in chunks, reducing memory usage. Example: Piping a large file from a read stream to a write stream:  
    ```javascript
    fs.createReadStream('input.txt')
      .pipe(fs.createWriteStream('output.txt'));
    ```

---

### **Database Integration**
13. **How does Mongoose improve MongoDB interactions?**  
    *Expected Answer*:  
    Mongoose provides schema validation, middleware (pre/post hooks), and query building. Example schema:  
    ```javascript
    const userSchema = new mongoose.Schema({
      name: { type: String, required: true },
      email: { type: String, unique: true }
    });
    ```

14. **What is database connection pooling, and why is it important?**  
    *Expected Answer*:  
    Connection pooling reuses existing database connections instead of opening/closing them per request. It reduces latency and improves performance (e.g., configure pool size in `mysql2` or `pg`).

---

### **Advanced Concepts**
15. **When would you use worker threads in Node.js?**  
    *Expected Answer*:  
    Worker threads handle CPU-intensive tasks (e.g., image processing) without blocking the event loop. Example:  
    ```javascript
    const { Worker } = require('worker_threads');
    new Worker('./heavy-task.js');
    ```

---

### **Real-World Scenarios**
16. **How would you debug a memory leak in a Node.js application?**  
    *Expected Answer*:  
    - Use `node --inspect` with Chrome DevTools to take heap snapshots.  
    - Monitor memory usage with `process.memoryUsage()`.  
    - Check for global variables or uncleared timers/event listeners.

17. **Describe a time you optimized a slow Node.js API endpoint.**  
    *Expected Answer*:  
    Look for answers mentioning **database indexing**, **query optimization**, **caching**, or **code profiling** with tools like `clinic.js`.

---

---

Understood! Here’s a revised list of **theoretical questions** for MongoDB and SQL, focusing on concepts like sharding, aggregation, normalization, and scalability. Answers are included for verification:

---

### **MongoDB Concepts**
1. **What is the purpose of a document database like MongoDB, and how does it differ from a relational database?**  
   *Expected Answer*:  
   MongoDB is schema-less, stores data as JSON-like documents (BSON), and allows nested structures. Unlike relational databases, it does not enforce rigid table schemas or require joins for related data, prioritizing flexibility and scalability.

2. **Explain how MongoDB achieves horizontal scalability.**  
   *Expected Answer*:  
   MongoDB uses **sharding**, where data is partitioned into chunks and distributed across multiple servers (shards). The `mongos` router directs queries to the appropriate shard, enabling parallel processing and handling large datasets.

3. **What is a shard key? What factors make a shard key effective?**  
   *Expected Answer*:  
   A shard key determines how data is distributed across shards. A good shard key has:  
   - **High cardinality** (many unique values to avoid hotspots).  
   - **Even distribution** (prevents uneven load on shards).  
   - **Alignment with query patterns** (e.g., using `userId` for user-centric queries).

4. **Describe the role of the aggregation pipeline in MongoDB.**  
   *Expected Answer*:  
   The aggregation pipeline processes documents through stages (e.g., `$match`, `$group`, `$project`) to filter, transform, and compute results. It enables complex operations like grouping, sorting, and joining collections (via `$lookup`).

5. **How does MongoDB ensure data consistency in a sharded cluster?**  
   *Expected Answer*:  
   MongoDB provides **eventual consistency** by default. For strong consistency, queries can use **read concerns** (e.g., `"majority"`) or **write concerns** (e.g., `{ w: "majority" }`) to ensure data is replicated across nodes before confirming success.

---

### **SQL Concepts**
6. **What is normalization in SQL? Explain 1NF, 2NF, and 3NF.**  
   *Expected Answer*:  
   Normalization reduces redundancy by structuring data into tables.  
   - **1NF**: Eliminate repeating groups; ensure atomic values.  
   - **2NF**: Remove partial dependencies (all non-key fields depend on the full primary key).  
   - **3NF**: Remove transitive dependencies (non-key fields depend only on the primary key).

7. **What are ACID properties, and why are they important?**  
   *Expected Answer*:  
   ACID ensures reliable transactions:  
   - **Atomicity**: Transactions succeed or fail entirely.  
   - **Consistency**: Valid data transitions.  
   - **Isolation**: Concurrent transactions don’t interfere.  
   - **Durability**: Committed data persists after crashes.  
   They are critical for systems like banking where data integrity is non-negotiable.

8. **What is the difference between a primary key and a unique key?**  
   *Expected Answer*:  
   - **Primary key**: Uniquely identifies a row; cannot be `NULL`.  
   - **Unique key**: Ensures uniqueness but allows one `NULL` value (depending on the database).

9. **How does indexing improve query performance? When might it be counterproductive?**  
   *Expected Answer*:  
   Indexes allow faster data retrieval by creating a lookup structure (e.g., B-tree). However, they can slow down write operations (insert/update/delete) and consume additional storage, so over-indexing should be avoided.

10. **Explain the difference between horizontal and vertical scaling in SQL.**  
    *Expected Answer*:  
    - **Horizontal scaling**: Adding more servers (sharding/partitioning).  
    - **Vertical scaling**: Upgrading existing server resources (CPU, RAM).  
    SQL databases often prioritize vertical scaling, while NoSQL systems like MongoDB are built for horizontal scaling.

---

### **Sharding & Scalability (Both)**
11. **What are the trade-offs of sharding a database?**  
    *Expected Answer*:  
    - **Pros**: Handles large datasets, improves read/write throughput.  
    - **Cons**: Complexity in setup, potential for uneven data distribution (hotspots), and challenges in cross-shard transactions/joins.

12. **How would you handle a query that requires data from multiple shards in MongoDB?**  
    *Expected Answer*:  
    The `mongos` router scatters the query to all relevant shards, gathers results, and returns them to the client. However, cross-shard queries can be slower, so shard keys should align with common query patterns to minimize this.

13. **What is database partitioning in SQL? How does it differ from sharding?**  
    *Expected Answer*:  
    Partitioning splits a table into smaller segments (e.g., by date) within the same database. Sharding distributes partitions across multiple servers. Partitioning is logical; sharding is physical distribution.

---

### **Aggregation & Optimization**
14. **What is the purpose of the SQL `HAVING` clause? How is it different from `WHERE`?**  
    *Expected Answer*:  
    `WHERE` filters rows before aggregation, while `HAVING` filters groups after aggregation (e.g., with `GROUP BY`). Example: Filtering departments with average salary > $50k.

15. **In MongoDB, how would you design an aggregation pipeline to compute sales metrics by region?**  
    *Expected Answer*:  
    Use stages like:  
    1. `$match` to filter relevant sales.  
    2. `$group` to sum sales by region.  
    3. `$sort` to rank regions.  
    4. `$project` to reshape output (e.g., add computed fields).

16. **What is a covering index in SQL? Why is it useful?**  
    *Expected Answer*:  
    A covering index includes all columns required by a query, allowing the database to return data directly from the index without accessing the table. This reduces I/O and speeds up queries.

---

### **Scenario-Based Questions**
17. **How would you troubleshoot a slow aggregation query in MongoDB?**  
    *Expected Answer*:  
    - Check for missing indexes on filtered/sorted fields.  
    - Use `explain()` to analyze the query execution plan.  
    - Optimize pipeline stages (e.g., place `$match` early to reduce data flow).  

18. **A SQL query is performing poorly despite an index. What could be the issue?**  
    *Expected Answer*:  
    - The index might not be used due to functions or type conversions in the query (e.g., `WHERE UPPER(name) = 'JOHN'`).  
    - Outdated statistics causing the optimizer to choose a poor execution plan.  
    - Table/index fragmentation.

19. **When would you prefer embedding documents over referencing them in MongoDB?**  
    *Expected Answer*:  
    Embedding is ideal for:  
    - Small, frequently accessed sub-documents (e.g., comments on a blog post).  
    - Data that requires atomic updates.  
    Referencing (normalization) is better for large or shared datasets to avoid duplication.

---

### **Advanced Concepts**
20. **What is the CAP theorem? How does MongoDB comply with it?**  
    *Expected Answer*:  
    The CAP theorem states that a distributed system can only guarantee two of **Consistency**, **Availability**, and **Partition Tolerance**. MongoDB prioritizes **CP** (Consistency and Partition Tolerance) in sharded clusters, ensuring data consistency during partitions but potentially sacrificing availability temporarily.

21. **What is eventual consistency, and where is it acceptable?**  
    *Expected Answer*:  
    Eventual consistency means all nodes will reflect the latest data eventually, but not immediately. It’s acceptable in systems like social media feeds or comment sections where slight delays in data sync are tolerable.

---

### **Key Evaluation Criteria**
- **Depth of Understanding**: Look for clarity on trade-offs (e.g., embedding vs referencing, horizontal vs vertical scaling).  
- **Problem-Solving**: Can they articulate strategies for optimization or scalability without code?  
- **Real-World Analogies**: Senior candidates should use examples from past projects (e.g., “We sharded our MongoDB cluster to handle 10x traffic spikes”).  

These questions assess theoretical knowledge while mimicking real-world decision-making. Adjust follow-ups based on their responses!
