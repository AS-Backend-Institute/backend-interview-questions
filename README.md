<h1 align="center">Backend Developer Interview Questions & Answers</h1>

<p align="center">
  A curated, growing collection of <b>real backend & Node.js interview questions</b> with clear, correct answers —
  covering <b>Node.js, Express, MongoDB, MySQL, Redis, System Design & DSA</b>.
  <br/>Perfect for freshers and experienced developers preparing for backend interviews in 2026.
</p>

<p align="center">
  <a href="https://github.com/AS-Backend-Institute/backend-interview-questions/stargazers"><img src="https://img.shields.io/github/stars/AS-Backend-Institute/backend-interview-questions?style=flat&color=6366f1" alt="Stars"/></a>
  <a href="https://github.com/AS-Backend-Institute/backend-interview-questions/network/members"><img src="https://img.shields.io/github/forks/AS-Backend-Institute/backend-interview-questions?style=flat&color=8b5cf6" alt="Forks"/></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"/></a>
  <a href="https://asbackendinstitute.com"><img src="https://img.shields.io/badge/Maintained%20by-AS%20Backend%20Institute-FF3355" alt="Maintained by AS Backend Institute"/></a>
  <img src="https://img.shields.io/badge/questions-40%2B-blue" alt="40+ questions"/>
</p>

<p align="center">
  <b><a href="https://asbackendinstitute.com">🌐 asbackendinstitute.com</a></b> ·
  <a href="https://asbackendinstitute.com/blog">📚 Free Tutorials</a> ·
  <a href="https://asbackendinstitute.com/dsa-sheet">📝 DSA Sheet</a> ·
  <a href="https://asbackendinstitute.com/interview-prep">🎯 Interview Prep</a>
</p>

---

> **How to use this repo:** Browse by topic in the table below. Each answer is kept short and interview-ready; where a topic deserves a deeper dive, we link to a full free tutorial on [AS Backend Institute](https://asbackendinstitute.com). ⭐ **Star the repo** to get updates as new questions are added.

## 📑 Contents

| # | Topic | Questions |
|---|-------|-----------|
| 1 | [Node.js Fundamentals](#1-nodejs-fundamentals) | 8 |
| 2 | [Express & REST APIs](#2-express--rest-apis) | 6 |
| 3 | [MongoDB](#3-mongodb) | 6 |
| 4 | [MySQL / SQL](#4-mysql--sql) | 6 |
| 5 | [Redis](#5-redis) | 5 |
| 6 | [System Design](#6-system-design) | 5 |
| 7 | [DSA for Backend](#7-dsa-for-backend) | 4 |

---

## 1. Node.js Fundamentals

<details>
<summary><b>1. What is the Node.js event loop and why does it matter?</b></summary>

The event loop is what lets Node.js perform **non-blocking I/O** despite JavaScript being single-threaded. Instead of waiting for slow operations (file/DB/network), Node offloads them (to libuv's thread pool or the OS) and continues running other code; when the operation finishes, its callback is queued and run by the event loop. It processes work in phases (timers → pending callbacks → poll → check → close), draining the **microtask queue** (Promises, `process.nextTick`) between phases. This is why Node handles thousands of concurrent connections efficiently.

📖 Deep dive: [Node.js Event Loop, step by step](https://asbackendinstitute.com/blog/nodejs)
</details>

<details>
<summary><b>2. Difference between <code>process.nextTick()</code>, <code>setImmediate()</code> and <code>setTimeout()</code>?</b></summary>

- `process.nextTick()` — runs **before** the event loop continues, right after the current operation (highest priority; can starve the loop if overused).
- `setImmediate()` — runs in the **check** phase, after the poll phase (i.e., after I/O callbacks).
- `setTimeout(fn, 0)` — runs in the **timers** phase after at least the given delay.

Order inside an I/O cycle: `nextTick` → Promises → `setImmediate` vs `setTimeout` can vary at the top level but `setImmediate` fires before `setTimeout` inside an I/O callback.
</details>

<details>
<summary><b>3. Blocking vs non-blocking code — give an example.</b></summary>

Blocking code stops the event loop until it finishes; non-blocking returns immediately and uses a callback/Promise.

```js
// Blocking — freezes everything until the file is read
const data = fs.readFileSync('big.log');

// Non-blocking — event loop stays free
fs.readFile('big.log', (err, data) => { /* ... */ });
```
Avoid sync APIs (`*Sync`, heavy CPU loops, `JSON.parse` on huge payloads) on the request path — they block **all** users.
</details>

<details>
<summary><b>4. <code>module.exports</code> vs <code>exports</code> — what's the difference?</b></summary>

`exports` is just a reference to `module.exports`. You can attach properties to `exports` (`exports.foo = ...`), but if you **reassign** `exports = {...}` it no longer points to `module.exports`, so nothing is exported. To export a single value (function/class), always use `module.exports = ...`.

📖 Deep dive: [module.exports vs exports](https://asbackendinstitute.com/blog/nodejs)
</details>

<details>
<summary><b>5. How does Node.js handle CPU-intensive tasks?</b></summary>

The single main thread is bad for CPU-heavy work (it blocks the event loop). Options: **Worker Threads** (`worker_threads`) for CPU tasks in parallel, the **cluster** module or a process manager (PM2) to use multiple cores, or offload to a **job queue** (BullMQ/RabbitMQ) processed by separate workers. For scaling across machines, put a load balancer in front of multiple instances.
</details>

<details>
<summary><b>6. What are streams in Node.js and when do you use them?</b></summary>

Streams process data in **chunks** instead of loading it all into memory — essential for large files, uploads/downloads, and piping. Four types: Readable, Writable, Duplex, Transform. Example: `fs.createReadStream(file).pipe(res)` streams a file to the client without buffering the whole thing, keeping memory flat.
</details>

<details>
<summary><b>7. Callbacks vs Promises vs async/await?</b></summary>

All handle asynchronous results. Callbacks lead to nesting ("callback hell") and manual error handling. Promises flatten this with `.then/.catch`. `async/await` is syntactic sugar over Promises that reads like synchronous code and uses `try/catch` for errors. Prefer `async/await`; use `Promise.all` for parallel independent operations.
</details>

<details>
<summary><b>8. How do you handle errors in async code?</b></summary>

Wrap `await` calls in `try/catch`, and in Express use a centralized error-handling middleware. Never swallow errors silently. For unhandled cases, listen to `process.on('unhandledRejection')` and `uncaughtException` (log + graceful shutdown). Validate input early and return proper HTTP status codes (400/401/404/500).

📖 Related: [HTTP status codes reference](https://asbackendinstitute.com/http-status-codes)
</details>

<p align="right"><a href="#-contents">⬆ Back to top</a></p>

## 2. Express & REST APIs

<details>
<summary><b>1. What is middleware in Express?</b></summary>

Middleware are functions with signature `(req, res, next)` that run in order for each request. They can modify `req`/`res`, end the response, or call `next()` to pass control. Used for logging, body parsing, authentication, validation, and error handling. Error middleware has four args `(err, req, res, next)`.
</details>

<details>
<summary><b>2. How do you structure a production Express app?</b></summary>

Separate concerns: **routes → controllers → services → models**. Keep routes thin (just wiring), put business logic in services, and DB access in models. Add centralized error handling, request validation, config via env vars, and a logger. This keeps the app testable and maintainable as it grows.
</details>

<details>
<summary><b>3. What makes an API RESTful?</b></summary>

Resource-based URLs (`/users/:id`), correct HTTP methods (GET read, POST create, PUT/PATCH update, DELETE remove), correct status codes, statelessness (each request carries its own auth/context), and consistent JSON responses. Use nouns for resources, not verbs.
</details>

<details>
<summary><b>4. How does JWT authentication work?</b></summary>

On login, the server verifies credentials and returns a signed **JWT** (header.payload.signature). The client sends it in `Authorization: Bearer <token>` on each request; the server verifies the signature (no DB lookup needed) and reads the payload. Keep tokens short-lived, store secrets server-side, and use refresh tokens for long sessions. 401 = not authenticated, 403 = authenticated but not allowed.
</details>

<details>
<summary><b>5. How do you secure an Express API?</b></summary>

Validate/sanitize all input, use HTTPS, set security headers (helmet), enable CORS correctly, rate-limit (429 on abuse), hash passwords (bcrypt), never leak stack traces, keep dependencies updated, and store secrets in env vars — not in code.
</details>

<details>
<summary><b>6. What is the difference between PUT and PATCH?</b></summary>

`PUT` replaces the **entire** resource (idempotent — same request twice = same result). `PATCH` applies a **partial** update (only the fields sent). Use PATCH for small edits, PUT when you send the full object.
</details>

<p align="right"><a href="#-contents">⬆ Back to top</a></p>

## 3. MongoDB

<details>
<summary><b>1. When would you choose MongoDB over a SQL database?</b></summary>

Choose MongoDB when the schema is flexible/evolving, data is document-shaped (nested/denormalized), you need horizontal scaling (sharding), and reads/writes map to whole documents. Choose SQL for strong relational integrity, complex joins, and multi-row transactions. Many real systems use both.
</details>

<details>
<summary><b>2. What is indexing and why does it matter?</b></summary>

An index is a data structure (B-tree) that lets MongoDB find documents without scanning the whole collection. Without indexes, queries do a **COLLSCAN** (slow at scale). Create indexes on fields you filter/sort by; use compound indexes for multi-field queries; check with `.explain()`. Too many indexes slow down writes, so index deliberately.

📖 Deep dive: [MongoDB indexing & queries](https://asbackendinstitute.com/blog/mongodb)
</details>

<details>
<summary><b>3. <code>findOneAndUpdate</code> vs <code>updateOne</code> — what's the difference?</b></summary>

`updateOne` updates a document and returns only the result status. `findOneAndUpdate` updates **and returns the document** (before or after the update via `returnDocument`/`new`), and is **atomic** — useful for counters, queues, and read-modify-write without race conditions.

📖 Deep dive: [findOneAndUpdate & atomic operations](https://asbackendinstitute.com/blog/mongodb/findoneandupdate-findoneanddelete-atomic-operations)
</details>

<details>
<summary><b>4. What is the aggregation pipeline?</b></summary>

A framework to process documents through **stages** (`$match`, `$group`, `$sort`, `$lookup`, `$project`, etc.), each feeding the next. Used for analytics, joins (`$lookup`), grouping, and transformations that a simple `find` can't do. Put `$match` early to reduce the documents flowing through the pipeline.
</details>

<details>
<summary><b>5. How do you design schemas / model relationships in MongoDB?</b></summary>

Two patterns: **embedding** (store related data inside the document — fast reads, good for 1-to-few, data read together) and **referencing** (store an ObjectId and `$lookup`/populate — good for 1-to-many/many-to-many and large/independent data). Model around your query patterns, not around normalization.
</details>

<details>
<summary><b>6. What is <code>mongosh</code>?</b></summary>

`mongosh` is the modern MongoDB Shell — an interactive JavaScript REPL to connect to a database and run queries, admin commands, and scripts. It replaced the legacy `mongo` shell with better syntax highlighting, autocomplete and Node.js API compatibility.

📖 Deep dive: [mongosh — MongoDB Shell](https://asbackendinstitute.com/blog/mongodb/mongosh-mongodb-shell)
</details>

<p align="right"><a href="#-contents">⬆ Back to top</a></p>

## 4. MySQL / SQL

<details>
<summary><b>1. Explain the types of SQL JOINs.</b></summary>

- **INNER JOIN** — only rows matching in both tables.
- **LEFT JOIN** — all rows from the left table + matches from the right (NULLs if none).
- **RIGHT JOIN** — all rows from the right + matches from the left.
- **FULL OUTER JOIN** — all rows from both (MySQL emulates via UNION).

Join on indexed keys for performance.
</details>

<details>
<summary><b>2. What is database normalization?</b></summary>

Organizing tables to reduce redundancy and anomalies, in normal forms: **1NF** (atomic columns), **2NF** (no partial dependency on part of a composite key), **3NF** (no transitive dependency). Normalize for integrity; **denormalize** selectively for read performance when needed.
</details>

<details>
<summary><b>3. What is an index in SQL and what are its trade-offs?</b></summary>

An index speeds up reads by avoiding full-table scans, at the cost of slower writes and extra storage (the index must be updated on every insert/update). Index columns used in `WHERE`, `JOIN`, and `ORDER BY`. Use `EXPLAIN` to see if a query uses an index.
</details>

<details>
<summary><b>4. What are ACID properties?</b></summary>

**Atomicity** (all-or-nothing), **Consistency** (valid state to valid state), **Isolation** (concurrent transactions don't interfere), **Durability** (committed data survives crashes). Relational DBs like MySQL (InnoDB) provide ACID via transactions.
</details>

<details>
<summary><b>5. Difference between <code>WHERE</code> and <code>HAVING</code>?</b></summary>

`WHERE` filters rows **before** grouping; `HAVING` filters **after** `GROUP BY` (on aggregated results). Example: `HAVING COUNT(*) > 5` filters groups, which `WHERE` cannot do.
</details>

<details>
<summary><b>6. SQL vs NoSQL — how do you decide?</b></summary>

SQL: fixed schema, relations, joins, strong transactions (banking, orders). NoSQL: flexible schema, huge scale, document/key-value/graph data (feeds, catalogs, sessions). Decision is driven by **data shape + query patterns + consistency needs**, not hype.
</details>

<p align="right"><a href="#-contents">⬆ Back to top</a></p>

## 5. Redis

<details>
<summary><b>1. What is Redis and what is it used for?</b></summary>

Redis is an in-memory key-value data store — extremely fast because data lives in RAM. Common uses: **caching**, session storage, rate limiting, leaderboards (sorted sets), queues, and pub/sub messaging.

📖 Deep dive: [Redis fundamentals](https://asbackendinstitute.com/blog/redis)
</details>

<details>
<summary><b>2. How does caching with Redis improve performance?</b></summary>

Instead of hitting a slow database on every request, you store the result in Redis and serve subsequent requests from memory (sub-millisecond). Common pattern: **cache-aside** — check cache, on miss read DB then set cache with a TTL. Always set expiry to avoid stale/unbounded data.
</details>

<details>
<summary><b>3. What is cache invalidation and why is it hard?</b></summary>

Keeping the cache consistent with the source of truth. Strategies: TTL (expire after time), write-through (update cache on write), and explicit invalidation (delete key on change). It's hard because stale data causes bugs while over-invalidating kills the cache's benefit.
</details>

<details>
<summary><b>4. Does Redis persist data?</b></summary>

Yes, optionally. **RDB** takes point-in-time snapshots; **AOF** logs every write for better durability. You can use either, both, or none (pure cache). Trade-off: durability vs performance/memory.
</details>

<details>
<summary><b>5. How would you implement rate limiting with Redis?</b></summary>

Use an atomic counter per user/IP with an expiry: `INCR key` then `EXPIRE key 60`; if the counter exceeds the limit within the window, return **429 Too Many Requests**. Sliding-window or token-bucket algorithms give smoother limits.

📖 Related: [HTTP 429 Too Many Requests](https://asbackendinstitute.com/http-status-codes/429)
</details>

<p align="right"><a href="#-contents">⬆ Back to top</a></p>

## 6. System Design

<details>
<summary><b>1. How do you scale a backend to handle more traffic?</b></summary>

**Vertical** (bigger server) has limits, so scale **horizontally**: multiple app instances behind a **load balancer**, a caching layer (Redis), DB read replicas, and async processing via queues. Make the app **stateless** (store sessions in Redis/JWT) so any instance can serve any request.
</details>

<details>
<summary><b>2. What is a load balancer?</b></summary>

A component that distributes incoming requests across multiple servers (round-robin, least-connections, etc.), improving availability and throughput. It also does health checks and removes unhealthy instances. Examples: Nginx, HAProxy, cloud LBs.
</details>

<details>
<summary><b>3. How do you handle heavy/slow tasks (emails, reports, image processing)?</b></summary>

Don't do them in the request. Push a job to a **queue** (BullMQ/RabbitMQ/Kafka) and process it with separate **worker** processes. Return `202 Accepted` immediately and let the client poll or get notified. This keeps APIs fast and resilient.
</details>

<details>
<summary><b>4. What's the difference between horizontal and vertical scaling?</b></summary>

**Vertical** = add more power (CPU/RAM) to one machine (simple, but a ceiling + single point of failure). **Horizontal** = add more machines (near-unlimited, fault-tolerant, but needs statelessness + a load balancer). Most large systems scale horizontally.
</details>

<details>
<summary><b>5. What is database sharding vs replication?</b></summary>

**Replication** = copies of the same data (read scaling + failover). **Sharding** = splitting data across nodes by a shard key (write/storage scaling). Replication helps reads and availability; sharding helps when one node can't hold or handle all the data.
</details>

<p align="right"><a href="#-contents">⬆ Back to top</a></p>

## 7. DSA for Backend

<details>
<summary><b>1. What time complexity should common operations have?</b></summary>

Know Big-O: hash map lookup O(1), balanced tree/binary search O(log n), single loop O(n), nested loops O(n²). In interviews, always state the time **and** space complexity of your solution and whether it can be improved.
</details>

<details>
<summary><b>2. When do you use a hash map vs an array?</b></summary>

Hash map for **O(1)** lookups/inserts by key (dedup, counting, caching, "have I seen this?"). Array for ordered/indexed data and when you need to iterate or use two-pointer/sliding-window techniques. Many "optimize this loop" questions are solved by adding a hash map.
</details>

<details>
<summary><b>3. Explain the sliding window pattern.</b></summary>

A technique for problems on contiguous subarrays/substrings (max sum of size k, longest substring without repeats). You move two pointers to expand/shrink a window instead of recomputing from scratch — turning O(n²) into **O(n)**.

📖 Practice: [DSA Sheet](https://asbackendinstitute.com/dsa-sheet)
</details>

<details>
<summary><b>4. How do you approach a DSA problem in an interview?</b></summary>

1) Clarify inputs/outputs/edge cases, 2) state a brute-force + its complexity, 3) optimize (hash map, two pointers, sorting, etc.), 4) code cleanly, 5) test with examples and edge cases, 6) state final time/space complexity. Communicate throughout.
</details>

<p align="right"><a href="#-contents">⬆ Back to top</a></p>

---

## 🤝 Contributing

Contributions are welcome! To add or improve a question:

1. Fork the repo and create a branch.
2. Add your Q&A using the existing `<details>` format (keep answers concise and correct).
3. Open a Pull Request describing the change.

Please keep answers accurate, beginner-friendly, and free of spam/self-promotion (curated deep-dive links are fine). See [CONTRIBUTING.md](CONTRIBUTING.md).

## ⭐ Support

If this helped you, **star the repo** — it helps more developers find it.

For structured, mentor-led learning with live classes, real projects and a placement guarantee, check out **[AS Backend Institute](https://asbackendinstitute.com)** — plus 1000+ free bilingual (English + Hindi) tutorials.

## 📜 License

[MIT](LICENSE) — free to use, share and contribute.

---

<p align="center">
  Maintained by <a href="https://asbackendinstitute.com"><b>AS Backend Institute</b></a> ·
  <a href="https://asbackendinstitute.com/blog">Tutorials</a> ·
  <a href="https://asbackendinstitute.com/roadmap">Roadmap</a> ·
  <a href="https://asbackendinstitute.com/dsa-sheet">DSA Sheet</a> ·
  <a href="https://asbackendinstitute.com/interview-prep">Interview Prep</a>
</p>
