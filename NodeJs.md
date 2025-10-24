Here’s a **hard, last-30-minute Node.js interview QA** list for **experienced developers (5–8 years)**, including **security-focused questions like Helmet**, best practices, and tricky scenarios:

---

## **1️⃣ Node.js Core & Advanced**

1. **Explain the Node.js event loop in detail.**

   * Single-threaded, non-blocking I/O.
   * **Phases:** timers, pending callbacks, idle/prepare, poll, check (setImmediate), close callbacks.
   * Can explain difference between **process.nextTick** and **setImmediate**.

2. **How does Node.js handle concurrency?**

   * Uses **event loop + libuv thread pool** for I/O operations.
   * CPU-bound tasks can block the loop → solution: **worker threads or child processes**.

3. **Difference between process.nextTick and setImmediate.**

   | Method           | When it executes                             |
   | ---------------- | -------------------------------------------- |
   | process.nextTick | Before the next event loop iteration         |
   | setImmediate     | After the current poll phase, next iteration |

4. **How to handle uncaught exceptions?**

   ```javascript
   process.on("uncaughtException", err => console.error("Exception:", err));
   process.on("unhandledRejection", err => console.error("Promise rejection:", err));
   ```

5. **Streams & backpressure:**

   * **Readable, Writable, Duplex, Transform streams**
   * Backpressure occurs when writable destination is slower than readable source → handle via `pipe()` or manual `drain` event.

---

## **2️⃣ Security (Helmet + Express)**

1. **What is Helmet and why use it?**

   * Middleware for Express that **secures HTTP headers**.
   * Prevents: XSS, Clickjacking, MIME sniffing, HSTS, etc.

   **Example:**

   ```javascript
   import express from "express";
   import helmet from "helmet";

   const app = express();
   app.use(helmet());
   ```

2. **Common Helmet headers:**

   | Header                    | Purpose                          |
   | ------------------------- | -------------------------------- |
   | X-DNS-Prefetch-Control    | Controls browser DNS prefetching |
   | X-Frame-Options           | Prevents clickjacking            |
   | X-Content-Type-Options    | Prevents MIME sniffing           |
   | Strict-Transport-Security | Enforces HTTPS                   |
   | Content-Security-Policy   | Prevents XSS                     |

3. **Other Node.js security best practices:**

   * Use **rate limiting** (`express-rate-limit`)
   * Enable **CORS properly** (`cors` package)
   * Validate input to prevent **NoSQL/SQL injection**
   * Encrypt sensitive data at rest and in transit
   * Use **environment variables** for secrets (`dotenv`)

---

## **3️⃣ Node.js + API Security Scenarios**

1. **JWT Authentication & Refresh Tokens**

   * Use short-lived JWTs, long-lived refresh tokens
   * Store refresh tokens securely (DB or Redis)
   * Example:

   ```javascript
   import jwt from "jsonwebtoken";
   const token = jwt.sign({ userId: "123" }, process.env.JWT_SECRET, { expiresIn: "15m" });
   ```

2. **Protect routes with middleware**

   ```javascript
   function auth(req, res, next) {
     const token = req.headers.authorization?.split(" ")[1];
     if (!token) return res.status(401).send("Unauthorized");
     jwt.verify(token, process.env.JWT_SECRET, (err, decoded) => {
       if (err) return res.status(403).send("Forbidden");
       req.user = decoded;
       next();
     });
   }
   app.get("/secure", auth, (req, res) => res.send("Secure Data"));
   ```

3. **Handling file uploads securely**

   * Limit file size (`multer`)
   * Validate MIME type
   * Scan for malware if needed
   * Avoid storing directly in `/uploads` → use **S3 or sanitized paths**

4. **Prevent Server-Side Template Injection (SSTI)**

   * Avoid unsafe template rendering
   * Sanitize user input if using EJS, Handlebars, or Pug

5. **CSRF protection**

   * For state-changing operations (`POST`, `PUT`, `DELETE`)
   * Use `csurf` middleware in Express

---

## **4️⃣ Hard Node.js Questions (Scenario-Based)**

| Question                                                | Tip/Answer                                                                                 |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| How to prevent event loop blocking for CPU-heavy tasks? | Use `worker_threads` or offload to microservice                                            |
| How to scale Node.js in production?                     | Use **PM2** or **cluster module** + load balancer                                          |
| Difference between cluster and worker_threads?          | Cluster → multiple Node.js processes (multi-core), worker_threads → threads within process |
| How to handle multiple DB connections in Lambda?        | Use **connection pooling** or **RDS Proxy**                                                |
| How to secure REST APIs for multi-tenant apps?          | Validate tenant, enforce RBAC, rate limiting, JWT + scopes                                 |
| How does Helmet’s CSP prevent XSS?                      | Defines allowed sources for scripts/styles/images; blocks inline or unauthorized scripts   |
| How to protect against NoSQL injection in MongoDB?      | Use **parameterized queries** or **Mongoose schemas**, avoid `$where` with user input      |
| How to handle untrusted headers in Express?             | Whitelist headers, validate origin, sanitize inputs                                        |
| How to detect slow endpoints in Node.js?                | Use **middleware logging** + APM tools like Dynatrace/NewRelic                             |

---

## **5️⃣ Node.js + Security Best Practices Checklist**

1. Use `helmet()` in Express for HTTP headers
2. Enable **rate limiting** (`express-rate-limit`)
3. Validate all user inputs (schema validation: Joi/Zod)
4. Use HTTPS + HSTS
5. Secure JWT + refresh tokens
6. Sanitize DB queries
7. Handle unhandled promises & exceptions
8. Use environment variables for secrets
9. Log securely without leaking sensitive info
10. CORS policies → only allow trusted origins

---

If you want, I can **create a mini “Node.js security + advanced features cheat sheet”** with **5–10 hardest questions and code examples**, which you can **memorize in 10 minutes** for a client round.

Do you want me to do that next?
