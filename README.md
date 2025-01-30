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

### Evaluation Tips:
- **Depth of Knowledge**: A senior candidate should explain **why** and **how**, not just syntax.  
- **Best Practices**: Look for mentions of security (CORS, rate limiting), logging (Winston), and testing (Jest/Mocha).  
- **Real-World Experience**: Ask for examples of scaling, debugging, or collaborating on large codebases.

This list balances **core concepts** and **advanced topics** to assess both foundational understanding and hands-on expertise. Adjust based on the candidate’s specific projects!
