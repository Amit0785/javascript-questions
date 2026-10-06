# Node.js Interview Questions & Answers — 4 Years Experience

A comprehensive interview-preparation guide covering Node.js fundamentals, Event Loop, asynchronous programming, Express.js, authentication, security, MongoDB integration, performance, production scenarios, testing, TypeScript, and system-design-oriented questions.

---

## 1. Node.js Fundamentals

### 1. What is Node.js?

**Answer:**  
Node.js is a JavaScript runtime built on Google's V8 JavaScript engine. It allows JavaScript to run outside the browser and is commonly used to build APIs, backend services, real-time applications, and microservices.

### 2. Why is Node.js popular for backend development?

**Answer:**  
Because it provides:

- Non-blocking I/O
- Event-driven architecture
- High concurrency
- JavaScript/TypeScript across frontend and backend
- Large npm ecosystem
- Good performance for I/O-heavy applications

### 3. Is Node.js a programming language?

**Answer:**  
No. JavaScript is the programming language, while Node.js is a runtime environment that executes JavaScript outside the browser.

### 4. Is Node.js a framework?

**Answer:**  
No. Node.js is a runtime. Frameworks such as Express.js, NestJS, and Fastify run on top of Node.js.

### 5. What is V8?

**Answer:**  
V8 is Google's JavaScript engine, written primarily in C++. It compiles JavaScript into machine code and executes it.

### 6. What is libuv?

**Answer:**  
libuv is a C library used by Node.js to provide asynchronous I/O capabilities.

It handles things such as:

- Event loop
- File system operations
- Networking
- Timers
- Thread pool

### 7. What is the Event Loop?

**Answer:**  
The Event Loop allows Node.js to perform asynchronous operations without blocking the main JavaScript thread.

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer");
}, 0);

console.log("End");
```

Output:

```text
Start
End
Timer
```

The timer callback executes asynchronously.

### 8. Why is Node.js single-threaded?

**Answer:**  
JavaScript execution in Node.js primarily happens on a single thread. This simplifies concurrency and avoids many traditional thread synchronization problems.

However, Node.js internally uses multiple threads through libuv for certain operations.

### 9. Is Node.js completely single-threaded?

**Answer:**  
No.

The JavaScript execution thread is single-threaded, but Node.js can use:

- libuv thread pool
- Worker Threads
- Child processes
- Cluster processes

### 10. What is non-blocking I/O?

**Answer:**  
Non-blocking I/O means Node.js doesn't wait for an I/O operation to finish before continuing execution.

For example:

```js
const users = await User.find();
```

While the database operation is being processed, Node.js can handle other asynchronous work.

---

## 2. Event Loop

### 11. What are the phases of the Event Loop?

**Answer:**

Important phases are:

1. Timers
2. Pending callbacks
3. Idle/prepare
4. Poll
5. Check
6. Close callbacks

### 12. What is the timers phase?

**Answer:**  
The timers phase executes callbacks scheduled by functions such as:

```js
setTimeout()
setInterval()
```

The specified delay is a minimum threshold, not an exact execution time.

### 13. What is the poll phase?

**Answer:**  
The poll phase handles I/O-related callbacks and waits for new I/O events when appropriate.

### 14. What is the check phase?

**Answer:**  
The check phase executes callbacks scheduled using:

```js
setImmediate()
```

### 15. What is `process.nextTick()`?

**Answer:**  
`process.nextTick()` schedules a callback to execute after the current operation but before the Event Loop proceeds to the next phase.

```js
process.nextTick(() => {
  console.log("next tick");
});
```

### 16. `process.nextTick()` vs `setImmediate()`?

**Answer:**

`process.nextTick()` executes before the Event Loop moves to the next phase.

`setImmediate()` executes during the check phase.

Using `process.nextTick()` excessively can starve the Event Loop.

### 17. What are microtasks?

**Answer:**  
Microtasks include Promise callbacks and `queueMicrotask()` callbacks.

```js
Promise.resolve().then(() => {
  console.log("Promise");
});
```

They are processed before the Event Loop proceeds to later phases.

### 18. What is Event Loop starvation?

**Answer:**  
Event Loop starvation occurs when one task or a continuously replenished queue prevents the Event Loop from processing other operations.

For example, excessive synchronous computation can block all incoming requests.

### 19. What happens if you execute a CPU-heavy operation in Node.js?

**Answer:**  
It blocks the main JavaScript thread and prevents the Event Loop from efficiently processing other requests.

For CPU-heavy operations, I would consider:

- Worker Threads
- Background jobs
- Separate services
- Child processes

### 20. What is the difference between synchronous and asynchronous code?

**Answer:**

Synchronous code blocks execution:

```js
const data = fs.readFileSync("file.txt");
```

Asynchronous code allows other operations to proceed:

```js
fs.readFile("file.txt", (err, data) => {});
```

---

## 3. Asynchronous JavaScript

### 21. What is a callback?

**Answer:**  
A callback is a function passed to another function to be executed later.

```js
fs.readFile("file.txt", (err, data) => {
  console.log(data);
});
```

### 22. What is callback hell?

**Answer:**  
Callback hell occurs when multiple asynchronous operations are nested inside each other, making code difficult to read and maintain.

```js
getUser(id, () => {
  getOrders(() => {
    getProducts(() => {
      // ...
    });
  });
});
```

Promises and `async/await` help solve this problem.

### 23. What is a Promise?

**Answer:**  
A Promise represents the eventual result of an asynchronous operation.

It can be:

- Pending
- Fulfilled
- Rejected

### 24. What is `async/await`?

**Answer:**  
`async/await` is syntax built on top of Promises that makes asynchronous code easier to read.

```js
const user = await User.findById(id);
```

### 25. What happens when an async function returns a value?

**Answer:**  
An `async` function always returns a Promise.

```js
async function getData() {
  return "Hello";
}
```

Conceptually:

```js
Promise.resolve("Hello");
```

### 26. How do you handle errors with async/await?

**Answer:**

```js
try {
  const user = await User.findById(id);
} catch (error) {
  console.error(error);
}
```

In Express applications, errors can also be passed to centralized error middleware.

### 27. What is `Promise.all()`?

**Answer:**  
It executes multiple promises concurrently and resolves when all succeed.

```js
const [users, products] = await Promise.all([
  getUsers(),
  getProducts()
]);
```

If one rejects, `Promise.all()` rejects.

### 28. `Promise.all()` vs `Promise.allSettled()`?

**Answer:**

`Promise.all()` fails when one Promise rejects.

`Promise.allSettled()` waits for every Promise and returns the result of each operation.

### 29. What is `Promise.race()`?

**Answer:**  
It returns the result of the first Promise that settles, whether fulfilled or rejected.

### 30. What is `Promise.any()`?

**Answer:**  
It returns the first fulfilled Promise.

If all Promises reject, it throws an `AggregateError`.

---

## 4. Node.js Modules

### 31. What is a module in Node.js?

**Answer:**  
A module is a reusable unit of code that can expose functionality to other files.

### 32. What is CommonJS?

**Answer:**

CommonJS uses:

```js
const express = require("express");

module.exports = router;
```

### 33. What are ES Modules?

**Answer:**

ES Modules use:

```js
import express from "express";

export default router;
```

### 34. CommonJS vs ES Modules?

**Answer:**

CommonJS:

```js
require()
module.exports
```

ES Modules:

```js
import
export
```

ES Modules are the standard JavaScript module system.

### 35. What is `require()`?

**Answer:**  
`require()` is used by CommonJS to load modules.

```js
const fs = require("fs");
```

### 36. What is `module.exports`?

**Answer:**  
It defines what a CommonJS module exposes.

```js
module.exports = {
  getUsers,
  createUser
};
```

### 37. What is npm?

**Answer:**  
npm is the Node Package Manager. It is used to install, manage, publish, and execute Node.js packages.

### 38. What is `package.json`?

**Answer:**  
It contains project metadata, scripts, dependencies, and configuration.

```json
{
  "name": "my-api",
  "scripts": {
    "start": "node server.js"
  }
}
```

### 39. `dependencies` vs `devDependencies`?

**Answer:**

`dependencies` are required when the application runs in production.

Examples:

```text
express
mongoose
jsonwebtoken
```

`devDependencies` are primarily required during development.

Examples:

```text
eslint
jest
nodemon
typescript
```

### 40. What is `package-lock.json`?

**Answer:**  
It locks exact dependency versions and their dependency tree, helping produce consistent installations across environments.

---

## 5. Express.js

### 41. What is Express.js?

**Answer:**  
Express.js is a lightweight web framework built on Node.js for creating APIs and web applications.

### 42. What is middleware?

**Answer:**  
Middleware is a function that can access:

```js
(req, res, next)
```

It can perform authentication, validation, logging, etc.

### 43. What does `next()` do?

**Answer:**  
`next()` passes control to the next middleware in the chain.

```js
app.use((req, res, next) => {
  console.log("Middleware");
  next();
});
```

### 44. What is application-level middleware?

**Answer:**

Middleware registered directly on the Express application:

```js
app.use(express.json());
```

### 45. What is router-level middleware?

**Answer:**  
Middleware attached to an Express Router.

```js
router.use(authMiddleware);
```

### 46. What is error-handling middleware?

**Answer:**  
Express error middleware has four parameters:

```js
app.use((err, req, res, next) => {
  res.status(500).json({
    message: err.message
  });
});
```

### 47. How would you structure a Node.js application?

**Answer:**

A common structure is:

```text
src/
 ├── controllers/
 ├── services/
 ├── models/
 ├── routes/
 ├── middleware/
 ├── validators/
 ├── utils/
 ├── config/
 └── app.js
```

For larger applications, I prefer keeping business logic in services rather than putting everything inside controllers.

### 48. Controller vs Service?

**Answer:**

The controller handles HTTP concerns:

```text
req
res
status codes
```

The service contains business logic.

This separation improves maintainability and testability.

### 49. What is REST API?

**Answer:**  
REST is an architectural style for designing APIs around resources.

For example:

```text
GET    /users
GET    /users/:id
POST   /users
PUT    /users/:id
DELETE /users/:id
```

### 50. PUT vs PATCH?

**Answer:**

`PUT` generally represents replacing/updating a resource representation.

`PATCH` is intended for partial updates.

Example:

```http
PATCH /users/123
```

```json
{
  "name": "Amit"
}
```

---

## 6. Authentication & Security

### 51. Authentication vs Authorization?

**Answer:**

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

### 52. How does JWT authentication work?

**Answer:**

```text
Login
 ↓
Validate credentials
 ↓
Generate JWT
 ↓
Client sends token
 ↓
Middleware verifies token
 ↓
Access protected API
```

### 53. What is JWT?

**Answer:**  
JWT stands for JSON Web Token. It contains claims that can be digitally signed and used for authentication.

### 54. What is the difference between access and refresh tokens?

**Answer:**

Access token:

- Short-lived
- Used for API requests

Refresh token:

- Longer-lived
- Used to obtain new access tokens
- Should be protected carefully

### 55. How should passwords be stored?

**Answer:**  
Passwords should never be stored as plain text.

Use a password hashing algorithm such as:

```text
bcrypt
Argon2
```

Example:

```js
const hash = await bcrypt.hash(password, 12);
```

### 56. What is CORS?

**Answer:**  
CORS stands for Cross-Origin Resource Sharing. It controls which origins can access resources from a server.

### 57. How can you secure an Express API?

**Answer:**

I would use:

- HTTPS
- Authentication
- Authorization
- Input validation
- Rate limiting
- Security headers
- CORS configuration
- Password hashing
- Environment variables
- Request size limits
- Proper error handling

### 58. What is rate limiting?

**Answer:**  
Rate limiting restricts how many requests a client can make within a certain time period.

It helps protect APIs against:

- Abuse
- Brute-force attacks
- Excessive traffic

### 59. What is input validation?

**Answer:**  
Input validation ensures that incoming data follows expected rules before processing it.

Libraries include:

```text
Joi
Zod
express-validator
```

### 60. Why shouldn't sensitive information be stored in source code?

**Answer:**  
Secrets such as:

```text
Database passwords
JWT secrets
API keys
Cloud credentials
```

should be stored in environment variables or a secure secret-management system.

---

## 7. Database & MongoDB Integration

### 61. How do you connect Node.js to MongoDB?

**Answer:**

Using Mongoose:

```js
await mongoose.connect(process.env.MONGO_URI);
```

### 62. What is Mongoose?

**Answer:**  
Mongoose is an ODM for MongoDB and Node.js.

It provides:

- Schemas
- Models
- Validation
- Middleware
- Population
- Query helpers

### 63. What is a Mongoose schema?

**Answer:**

A schema defines the structure and validation rules for documents.

```js
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true,
    unique: true
  }
});
```

### 64. What is a Mongoose model?

**Answer:**  
A model is created from a schema and provides an interface for interacting with a MongoDB collection.

```js
const User = mongoose.model("User", userSchema);
```

### 65. `find()` vs `findOne()`?

**Answer:**

`find()` returns multiple matching documents.

```js
User.find({ active: true });
```

`findOne()` returns the first matching document.

```js
User.findOne({ email });
```

### 66. What is `findById()`?

**Answer:**  
It is a convenient method for finding a document by `_id`.

```js
User.findById(userId);
```

### 67. What is `populate()`?

**Answer:**  
`populate()` resolves referenced documents.

```js
Post.find().populate("author");
```

### 68. What is MongoDB aggregation?

**Answer:**  
Aggregation processes documents through a pipeline.

```js
Order.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $group: {
      _id: "$userId",
      total: {
        $sum: "$amount"
      }
    }
  }
]);
```

### 69. What is `$match`?

**Answer:**  
It filters documents in an aggregation pipeline.

### 70. What is `$group`?

**Answer:**  
It groups documents and performs calculations such as:

```text
$sum
$avg
$count
$max
$min
```

---

## 8. Advanced Node.js

### 71. What are streams?

**Answer:**  
Streams allow data to be processed incrementally instead of loading everything into memory.

Types:

- Readable
- Writable
- Duplex
- Transform

### 72. What is a Buffer?

**Answer:**  
A Buffer represents binary data in Node.js.

It is commonly used with:

- Files
- Network protocols
- Streams
- Images

### 73. What is backpressure?

**Answer:**  
Backpressure occurs when a data producer generates data faster than the consumer can process it.

Streams help manage this situation efficiently.

### 74. What are Worker Threads?

**Answer:**  
Worker Threads allow JavaScript code to execute in separate threads.

They are useful for CPU-intensive tasks.

### 75. Worker Threads vs Cluster?

**Answer:**

**Worker Threads:**

Used for CPU-intensive JavaScript tasks.

**Cluster:**

Creates multiple Node.js processes, allowing applications to use multiple CPU cores.

### 76. What is clustering?

**Answer:**  
Clustering allows multiple Node.js processes to run and share server workloads.

This can help utilize multiple CPU cores.

### 77. What is a child process?

**Answer:**  
A child process allows Node.js to execute another process.

Common methods:

```text
spawn()
exec()
execFile()
fork()
```

### 78. `exec()` vs `spawn()`?

**Answer:**

`exec()` is convenient when you want the complete command output.

`spawn()` streams output and is better suited for commands producing large amounts of data.

### 79. What is graceful shutdown?

**Answer:**  
Graceful shutdown means allowing the application to finish existing requests and close resources before exiting.

For example:

```js
process.on("SIGTERM", async () => {
  await mongoose.connection.close();
  server.close();
});
```

### 80. Why is graceful shutdown important?

**Answer:**  
It prevents:

- Dropped requests
- Incomplete database operations
- Resource leaks
- Corrupted processes

It is especially important when running containers or applications behind load balancers.

---

## 9. Node.js Error Handling

### 81. What is an operational error?

**Answer:**  
Operational errors are expected runtime problems such as:

- Invalid input
- Database unavailable
- Network failure
- File not found

These should be handled gracefully.

### 82. What is a programmer error?

**Answer:**  
A programmer error is usually caused by a bug, such as:

```js
undefinedVariable.foo();
```

These should be identified and fixed rather than silently ignored.

### 83. What is an unhandled Promise rejection?

**Answer:**  
It occurs when a Promise rejects and no appropriate rejection handler handles it.

```js
Promise.reject(new Error("Failed"));
```

Production applications should monitor and handle such failures appropriately.

### 84. What is an uncaught exception?

**Answer:**  
It occurs when an exception reaches the process without being caught.

These can put the application into an unsafe state, so production systems should use process supervision and graceful recovery/restart strategies.

### 85. Should we continue after an uncaught exception?

**Answer:**  
Generally, an uncaught exception can leave the process in an unknown state. A safer production strategy is to log the error, perform necessary cleanup, and restart the process using a process manager or container orchestrator.

---

## 10. Performance

### 86. How do you improve Node.js performance?

**Answer:**

I would consider:

- Avoiding blocking operations
- Optimizing database queries
- Adding indexes
- Caching
- Connection pooling
- Pagination
- Streaming large data
- Worker Threads for CPU-heavy tasks
- Compression where appropriate
- Horizontal scaling

### 87. What is caching?

**Answer:**  
Caching stores frequently accessed data so that future requests can be served faster.

Redis is commonly used for distributed caching.

### 88. Where can Redis be used?

**Answer:**

Redis can be used for:

- Caching
- Sessions
- Rate limiting
- Queues
- Temporary data
- Distributed locks

### 89. How would you improve an API that makes five database queries?

**Answer:**

I would first determine whether all queries are necessary.

Possible improvements:

- Combine queries
- Use aggregation
- Use `populate()` carefully
- Use parallel independent queries with `Promise.all()`
- Cache frequently accessed data

### 90. When should you use `Promise.all()`?

**Answer:**  
When multiple operations are independent.

```js
const [user, orders] = await Promise.all([
  getUser(),
  getOrders()
]);
```

This can reduce total waiting time compared with sequential execution.

---

## 11. Real-World Scenario Questions

### 91. Your API receives 1,000 requests per second. What would you do?

**Answer:**

I would consider:

```text
Load Balancer
      ↓
Multiple Node.js instances
      ↓
Redis
      ↓
MongoDB
```

Then optimize:

- Database queries
- Indexes
- Connection pools
- Caching
- Rate limiting
- Horizontal scaling

### 92. One API endpoint suddenly becomes slow. How do you investigate?

**Answer:**

I would check:

1. API response time
2. Logs
3. Database query time
4. MongoDB `explain()`
5. External API latency
6. CPU
7. Memory
8. Event Loop delay
9. Recent deployments

### 93. MongoDB query takes 5 seconds. What do you do?

**Answer:**

First:

```js
.explain("executionStats")
```

Then inspect:

- Collection scans
- Index usage
- Documents examined
- Keys examined
- Execution time

Then optimize indexes/query structure.

### 94. How would you handle millions of records?

**Answer:**

I would use:

- Proper indexing
- Pagination/cursor pagination
- Projection
- Efficient aggregation
- Archiving
- Appropriate schema design
- Query optimization
- Potential sharding for very large-scale workloads

### 95. How would you prevent duplicate users?

**Answer:**

I would enforce uniqueness at the database level.

```js
email: {
  type: String,
  unique: true
}
```

And create a unique MongoDB index.

Application-level checks alone are not sufficient because concurrent requests can race.

### 96. How would you implement file upload?

**Answer:**

Typically:

```text
Client
 ↓
Node.js API
 ↓
Multer / streaming
 ↓
Object Storage
 ↓
Save file metadata in MongoDB
```

For large files, streaming is preferable to loading the entire file into memory.

### 97. How would you design an authentication system?

**Answer:**

```text
Register
 ↓
Hash Password
 ↓
Store User
 ↓
Login
 ↓
Verify Password
 ↓
Access Token + Refresh Token
 ↓
Protected APIs
 ↓
Authorization Middleware
```

### 98. How would you implement role-based authorization?

**Answer:**

Store roles:

```js
{
  role: "admin"
}
```

Then middleware:

```js
const authorize = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({
        message: "Forbidden"
      });
    }

    next();
  };
};
```

### 99. Authentication returns 401 or 403?

**Answer:**

Generally:

**401 Unauthorized**

Means the request is not authenticated or credentials are invalid/missing.

**403 Forbidden**

Means the user is authenticated but doesn't have permission.

### 100. How would you handle third-party API failure?

**Answer:**

I would use:

- Timeouts
- Retry with backoff where appropriate
- Error handling
- Circuit breaker where appropriate
- Logging
- Fallback behavior
- Idempotency for retryable operations

---

## 12. Advanced Production Questions

### 101. What is connection pooling?

**Answer:**  
Connection pooling maintains reusable database connections rather than creating a new connection for every request.

This improves performance and reduces connection overhead.

### 102. What happens if MongoDB goes down?

**Answer:**  
The application should:

- Handle database errors
- Avoid crashing unexpectedly
- Log failures
- Retry where appropriate
- Use MongoDB replica sets for high availability
- Return meaningful errors to clients

### 103. What is a MongoDB replica set?

**Answer:**  
A replica set is a group of MongoDB instances that maintain copies of the same data.

It provides:

- High availability
- Automatic failover
- Data redundancy

### 104. What is MongoDB sharding?

**Answer:**  
Sharding distributes data across multiple MongoDB servers.

It is useful for very large datasets and high-throughput workloads.

### 105. Replica Set vs Sharding?

**Answer:**

**Replica Set:**

Primarily provides redundancy and high availability.

**Sharding:**

Primarily distributes data and workload across multiple servers for horizontal scalability.

They can be used together.

### 106. What is idempotency?

**Answer:**  
An operation is idempotent if repeating the same operation produces the same intended result.

For example, a PUT request is generally designed to be idempotent.

For payments, idempotency keys can prevent duplicate charges when clients retry requests.

### 107. What is a circuit breaker?

**Answer:**  
A circuit breaker prevents an application from continuously calling a failing external service.

States commonly include:

```text
Closed
 ↓
Open
 ↓
Half-Open
```

It helps prevent cascading failures.

### 108. What is a message queue?

**Answer:**  
A message queue allows asynchronous processing.

Examples:

- RabbitMQ
- Kafka
- BullMQ/Redis

Example:

```text
API
 ↓
Queue
 ↓
Worker
 ↓
Email Service
```

### 109. When would you use a background job?

**Answer:**

For operations that don't need to block the API response:

- Sending emails
- Generating reports
- Image processing
- Notifications
- Data exports
- Video processing

### 110. How do you prevent a Node.js API from crashing?

**Answer:**

I would use:

- Proper error handling
- Input validation
- Timeouts
- Process monitoring
- Health checks
- Graceful shutdown
- Logging/monitoring
- Resource limits
- Correct handling of Promise rejections

---

## 13. Testing

### 111. How do you test Node.js applications?

**Answer:**

Common testing types include:

- Unit testing
- Integration testing
- API testing
- End-to-end testing

Tools include:

```text
Jest
Vitest
Mocha
Supertest
```

### 112. What is unit testing?

**Answer:**  
Testing an individual function or component in isolation.

### 113. What is integration testing?

**Answer:**  
Testing how multiple components work together.

For example:

```text
API
 ↓
Service
 ↓
MongoDB
```

### 114. Unit test vs integration test?

**Answer:**

**Unit test:**

Tests one component in isolation.

**Integration test:**

Tests interactions between multiple components.

### 115. What is mocking?

**Answer:**  
Mocking replaces a real dependency with a controlled fake implementation.

For example, mocking an external payment API during testing.

---

## 14. TypeScript + Node.js

### 116. Why use TypeScript with Node.js?

**Answer:**

TypeScript provides:

- Static typing
- Better IDE support
- Compile-time error detection
- Interfaces/types
- Better maintainability for large applications

### 117. Interface vs type in TypeScript?

**Answer:**

Both can describe object structures.

```ts
interface User {
  name: string;
}
```

```ts
type User = {
  name: string;
};
```

Interfaces are particularly useful for object contracts and declaration merging, while types are more flexible for unions, intersections, and other type compositions.

### 118. How do you type an Express request?

**Answer:**

You can use Express's generic request types.

```ts
interface Params {
  id: string;
}

interface Body {
  name: string;
}

app.post(
  "/users/:id",
  (req: Request<Params, {}, Body>, res: Response) => {
    // ...
  }
);
```

### 119. Why shouldn't we use `any` everywhere?

**Answer:**  
`any` disables much of TypeScript's type checking.

It can hide bugs and reduce maintainability.

Prefer:

```ts
unknown
```

when the type isn't known yet, then narrow it safely.

### 120. How do you structure a TypeScript Node.js application?

**Answer:**

```text
src/
 ├── controllers/
 ├── services/
 ├── repositories/
 ├── models/
 ├── routes/
 ├── middleware/
 ├── types/
 ├── utils/
 ├── config/
 └── app.ts
```

For larger applications, separating controllers, services, and repositories helps keep responsibilities clear.

---

## 15. Very Important Scenario Questions

### 121. What happens when 1,000 users call your API simultaneously?

**Answer:**

Node.js doesn't create one JavaScript thread per request.

The Event Loop manages asynchronous operations while the OS/libuv handles underlying I/O. The application can therefore handle many concurrent I/O-bound requests efficiently.

The real bottlenecks could be:

- CPU
- Database
- Network
- External APIs
- Connection pools
- Memory

### 122. Your Node.js server CPU reaches 100%. What do you check?

**Answer:**

I would check:

1. CPU profiling
2. Event Loop blocking
3. Infinite loops
4. CPU-heavy functions
5. Traffic spikes
6. Recent deployment
7. Worker processes
8. Database/external service behavior

If CPU-heavy JavaScript is identified, I'd consider Worker Threads or moving the work to background services.

### 123. Your Node.js server memory keeps increasing. What do you check?

**Answer:**

I would investigate:

- Memory leaks
- Global variables
- Event listeners
- Unbounded caches
- Large objects
- Timers
- Streams
- Heap snapshots

### 124. Your MongoDB CPU is high. What do you check?

**Answer:**

I would investigate:

- Slow queries
- Missing indexes
- Large aggregations
- Collection scans
- High traffic
- Inefficient `$lookup`
- Excessive concurrent queries

MongoDB profiling and `explain()` would help identify the problematic queries.

### 125. How would you design a production Node.js API?

**Answer:**

I would use something like:

```text
                    Client
                      │
                      ▼
                Load Balancer
                      │
             ┌────────┴────────┐
             ▼                 ▼
         Node API           Node API
             │                 │
             └────────┬────────┘
                      │
             ┌────────┴────────┐
             ▼                 ▼
           Redis            MongoDB
             │
             ▼
         Job Queue
             │
             ▼
          Workers
```

Along with:

- Authentication
- Authorization
- Validation
- Centralized error handling
- Logging
- Monitoring
- Rate limiting
- Database indexes
- Caching
- Graceful shutdown
- Automated testing

---

# ⭐ Top 15 Questions to Master

If you have limited time before an interview, focus heavily on these:

| # | Question |
|---:|---|
| 1 | Explain Node.js architecture |
| 2 | Explain Event Loop |
| 3 | What happens when Event Loop is blocked? |
| 4 | `process.nextTick()` vs Promise vs `setImmediate()` |
| 5 | Callback vs Promise vs async/await |
| 6 | How does Express middleware work? |
| 7 | How do you implement JWT authentication? |
| 8 | Access token vs refresh token |
| 9 | How do you handle errors in Express? |
| 10 | How do you optimize a slow API? |
| 11 | How do you handle CPU-intensive tasks? |
| 12 | Worker Threads vs Cluster |
| 13 | How do you scale Node.js horizontally? |
| 14 | How do you debug a memory leak? |
| 15 | How would you design a production Node.js backend? |

---

# Interview Answer Strategy for 4 Years Experience

For scenario questions, avoid giving only a definition.

Use this structure:

**1. Explain the concept → 2. Give a practical example → 3. Explain the production approach.**

For example, instead of saying:

> "Node.js is single-threaded."

A stronger interview answer is:

> "Node.js executes JavaScript primarily on a single thread using the Event Loop. I/O operations are handled asynchronously, so the main thread can continue processing other requests. However, CPU-intensive synchronous operations can block the Event Loop, so for those cases I would use Worker Threads, background jobs, or separate services."

This style demonstrates both conceptual understanding and practical production experience.
