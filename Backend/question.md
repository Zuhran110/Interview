# Node.js & Express — 100 Interview Questions & Answers

---

## Node Basics and Fundamentals

---

**Q1. What is Node.js?**

Node.js is an open-source, cross-platform **JavaScript runtime environment** built on Chrome's **V8 JavaScript engine**. It executes JavaScript code on the server side — outside the browser.

- Uses an **event-driven, non-blocking I/O** model.
- Ideal for **scalable, real-time** applications (APIs, chat, streaming).

```js
console.log('Hello from Node.js!');
// Run with: node app.js
```

---

**Q2. What is the difference between a framework and a runtime environment?**

| Runtime Environment | Framework |
|---|---|
| Provides the platform to **execute** code | Provides **structure and tools** to build applications |
| Node.js — runs JavaScript outside the browser | Express.js — routes, middleware, HTTP utilities |
| Like an engine for a car | Like the car's body and controls |

A runtime is the foundation; a framework is built on top of it.

---

**Q3. What is the difference between Node.js and Express.js?**

| Node.js | Express.js |
|---|---|
| JavaScript **runtime** | Web **framework** built on Node.js |
| Low-level: raw HTTP, file system | High-level: routing, middleware, request/response helpers |
| Built into Node | Installed via `npm install express` |
| More verbose | Less code, cleaner API |

Express makes building web servers with Node.js much faster and simpler.

---

**Q4. What are the differences between client-side and server-side?**

| Client-Side | Server-Side |
|---|---|
| Runs in the **browser** | Runs on the **server** |
| JavaScript, HTML, CSS | Node.js, Python, Java, etc. |
| Handles UI, events, animations | Handles business logic, DB, auth |
| Has access to DOM | Has access to file system, DB, OS |
| Code is visible to the user | Code is hidden from the user |

---

## Main Features of Node

---

**Q5. What are the seven main features of Node?**

1. **Asynchronous & Non-blocking I/O** — doesn't wait for operations to complete.
2. **Single-threaded Event Loop** — handles concurrency efficiently.
3. **V8 Engine** — fast JavaScript execution.
4. **Cross-platform** — runs on Windows, Mac, Linux.
5. **NPM Ecosystem** — 2M+ packages available.
6. **Event-driven architecture** — built around EventEmitter.
7. **Scalable** — handles thousands of concurrent connections.

---

**Q6. What is single-threaded programming?**

Single-threaded means only **one operation executes at a time** on the main thread. Node.js runs all JavaScript on one thread but handles I/O asynchronously via the event loop and libuv thread pool.

**Advantage:** No thread synchronization issues (no deadlocks, no race conditions).
**Disadvantage:** CPU-intensive tasks block the event loop — use Worker Threads for those.

---

**Q7. What is synchronous programming?**

Operations execute **one after another**, blocking the thread until each completes.

```js
const data = fs.readFileSync('file.txt', 'utf8'); // blocks here
console.log(data); // runs only after file is fully read
console.log('Done'); // runs after
```

Good for: simple scripts, startup config loading.
Bad for: production web servers — one slow I/O blocks all requests.

---

**Q8. What is multi-threaded programming?**

Multiple threads run **in parallel**, each handling a task. Traditional servers (Apache, Java) use one thread per request.

- **Advantage:** Good for CPU-intensive work.
- **Disadvantage:** Thread overhead, deadlocks, race conditions.

Node.js avoids this complexity by using async I/O on a single thread. For CPU work in Node, use **Worker Threads**.

---

**Q9. What is asynchronous programming?**

Operations are **initiated but not waited for**. The program continues, and a callback/promise runs when the operation finishes.

```js
fs.readFile('file.txt', 'utf8', (err, data) => {
  console.log(data); // runs when file is ready
});
console.log('This runs immediately'); // doesn't wait
```

Node.js is fundamentally async — enabling high concurrency on a single thread.

---

**Q10. What is the difference between synchronous and asynchronous programming?**

| Synchronous | Asynchronous |
|---|---|
| Blocks execution until done | Does not block — continues immediately |
| Simple to reason about | Requires callbacks/promises/async-await |
| Bad for I/O-heavy servers | Ideal for I/O-heavy servers |
| `fs.readFileSync()` | `fs.readFile()` |

---

**Q11. What are events, event emitter, event queue, event loop, and event-driven in Node?**

- **Event:** A signal that something happened (e.g., HTTP request received, file read complete).
- **EventEmitter:** A class that lets objects emit named events and register listeners for them.
- **Event Queue (Callback Queue):** Holds callbacks waiting to be executed after I/O completes.
- **Event Loop:** Continuously checks the call stack and event queue — moves callbacks to the stack when it's empty.
- **Event-driven:** Programming model where flow is controlled by events rather than sequential execution.

```js
const EventEmitter = require('events');
const emitter = new EventEmitter();
emitter.on('greet', (name) => console.log(`Hello, ${name}`));
emitter.emit('greet', 'Alice'); // Hello, Alice
```

---

**Q12. What are the main features and advantages of Node?**

**Features:** Async I/O, event-driven, V8 engine, NPM, cross-platform, single-threaded.

**Advantages:**
- High performance for I/O-bound tasks.
- Same language (JS) on frontend and backend.
- Massive NPM ecosystem.
- Great for real-time apps (chat, live updates).
- Fast startup and low memory footprint.
- Microservices-friendly.

---

**Q13. What are the disadvantages of Node? When to use or not to use?**

**Disadvantages:**
- Not suited for **CPU-intensive** tasks (image processing, ML, complex math) — blocks the event loop.
- **Callback hell** (mitigated by async/await).
- Weaker support for **relational/SQL** patterns (compared to mature Java/Python ecosystems).
- Single-threaded — one unhandled exception can crash the server.

**Use Node when:** REST APIs, real-time apps, streaming, microservices, SPAs backends.
**Avoid Node when:** Heavy CPU computation, ML pipelines, complex financial calculations.

---

## Project Setup and Modules

---

**Q14. How to set up a Node project?**

```bash
mkdir my-project && cd my-project
npm init -y              # creates package.json
touch index.js           # create entry file
npm install express      # install a package
node index.js            # run the project
```

For auto-restart during development:
```bash
npm install --save-dev nodemon
# add to package.json scripts: "dev": "nodemon index.js"
npm run dev
```

---

**Q15. What is npm? What is the role of the node_modules folder?**

**NPM (Node Package Manager):**
- CLI tool to install, manage, and publish JavaScript packages.
- Comes bundled with Node.js.

**node_modules:**
- Folder where all installed packages and their dependencies live.
- Auto-generated by `npm install`.
- Should be in `.gitignore` — recreated from `package.json` by running `npm install`.

```bash
npm install express        # installs to node_modules, updates package.json
npm install -D nodemon     # dev dependency
npm uninstall express      # removes package
```

---

**Q16. What are Node modules?**

A module is a **reusable block of code** encapsulated in its own file. Node.js uses the **CommonJS** module system by default.

Types:
1. **Core modules** — built-in (e.g., `fs`, `http`, `path`).
2. **Third-party modules** — installed via npm (e.g., `express`, `mongoose`).
3. **Custom/local modules** — files you create yourself.

---

**Q17. What is the role of the package.json file?**

`package.json` is the **configuration/manifest** of a Node project:

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": { "start": "node index.js", "dev": "nodemon index.js" },
  "dependencies": { "express": "^4.18.2" },
  "devDependencies": { "nodemon": "^3.0.0" }
}
```

Tracks: project metadata, dependencies, run scripts, version.

---

**Q18. What are modules in Node? What is the difference between a function and a module?**

| Function | Module |
|---|---|
| A block of reusable code **inside** a file | An entire **file** that can be exported/imported |
| Local scope | File scope |
| Called by name | `require()`'d by path or name |

A module CAN export functions, but a function is not a module by itself.

---

**Q19. How many ways are there to export a module?**

```js
// 1. Named exports (multiple)
module.exports.add = (a, b) => a + b;
module.exports.subtract = (a, b) => a - b;

// 2. Default export (single value/object)
module.exports = { add: (a, b) => a + b };

// 3. Export a class
module.exports = class Logger { log(msg) { console.log(msg); } };

// 4. ES Module syntax (with "type": "module" in package.json)
export const add = (a, b) => a + b;
export default function greet() {}
```

---

**Q20. What will happen if you do not export the module?**

If you don't use `module.exports`, the module's contents are **private** to that file. Other files that `require()` it will get an **empty object `{}`**.

```js
// math.js — not exported
function add(a, b) { return a + b; }

// app.js
const math = require('./math');
console.log(math.add); // undefined
```

---

**Q21. How to import single or multiple functions from a module?**

```js
// Export
module.exports = { add, subtract, multiply };

// Import all
const math = require('./math');
math.add(2, 3);

// Destructure specific functions
const { add, subtract } = require('./math');
add(2, 3);

// ES Module (ESM)
import { add, subtract } from './math.js';
```

---

**Q22. What is the module wrapper function?**

Before executing a module, Node.js wraps it in a **function wrapper**:

```js
(function(exports, require, module, __filename, __dirname) {
  // Your module code runs here
});
```

This is why:
- `exports`, `require`, `module`, `__filename`, `__dirname` are available in every file.
- Each module has its **own scope** (variables don't leak globally).

---

**Q23. What are the types of modules in Node?**

1. **Core (Built-in) Modules** — ship with Node.js: `fs`, `http`, `path`, `os`, `events`, `crypto`, `stream`.
2. **Third-party Modules** — from NPM: `express`, `mongoose`, `axios`, `dotenv`.
3. **Local/Custom Modules** — files you write: `require('./utils')`.

---

## Top Built-in Modules

---

**Q24. What are the top five built-in modules commonly used in Node?**

1. **`fs`** — File system operations.
2. **`http`** — HTTP server and client.
3. **`path`** — File path manipulation.
4. **`os`** — Operating system info.
5. **`events`** — EventEmitter for custom events.

---

**Q25. Explain the role of the `fs` module.**

The `fs` (File System) module lets you interact with the file system.

```js
const fs = require('fs');

// Read (async)
fs.readFile('data.txt', 'utf8', (err, data) => console.log(data));

// Write
fs.writeFile('output.txt', 'Hello!', (err) => console.log('Written'));

// Append
fs.appendFile('log.txt', 'New line\n', () => {});

// Delete
fs.unlink('temp.txt', () => {});

// Check if file exists
fs.existsSync('data.txt'); // true/false

// Read directory
fs.readdir('./', (err, files) => console.log(files));
```

---

**Q26. Explain the role of the `path` module.**

The `path` module handles file and directory path operations in a cross-platform way.

```js
const path = require('path');

path.join('/users', 'alice', 'docs');       // '/users/alice/docs'
path.resolve('src', 'index.js');            // absolute path
path.basename('/users/alice/file.txt');     // 'file.txt'
path.dirname('/users/alice/file.txt');      // '/users/alice'
path.extname('index.html');                 // '.html'
path.parse('/users/alice/file.txt');        // { root, dir, base, ext, name }
```

Use `path.join()` instead of string concatenation to avoid OS-specific issues.

---

**Q27. Explain the role of the OS module.**

The `os` module provides information about the operating system.

```js
const os = require('os');

os.platform();     // 'linux', 'win32', 'darwin'
os.arch();         // 'x64', 'arm64'
os.hostname();     // computer name
os.homedir();      // '/home/alice'
os.tmpdir();       // '/tmp'
os.totalmem();     // total RAM in bytes
os.freemem();      // free RAM in bytes
os.cpus();         // array of CPU core info
os.uptime();       // system uptime in seconds
```

---

**Q28. Explain the role of the `events` module. How to handle events in Node?**

The `events` module provides the **EventEmitter** class — the foundation of Node's async, event-driven architecture.

```js
const EventEmitter = require('events');
const emitter = new EventEmitter();

// Register listener
emitter.on('userLogin', (user) => {
  console.log(`${user.name} logged in`);
});

// One-time listener
emitter.once('startup', () => console.log('Server started'));

// Emit event
emitter.emit('userLogin', { name: 'Alice' });

// Remove listener
emitter.removeListener('userLogin', handler);
emitter.removeAllListeners('userLogin');
```

---

**Q29. What are event arguments?**

Event arguments are **data passed along when an event is emitted**. They are received as parameters in the listener function.

```js
emitter.on('purchase', (item, price, qty) => {
  console.log(`Bought ${qty}x ${item} for $${price * qty}`);
});

emitter.emit('purchase', 'Laptop', 999, 2); // Bought 2x Laptop for $1998
```

---

**Q30. What is the difference between a function and an event?**

| Function | Event |
|---|---|
| Called **directly** by name | **Emitted** and listened to |
| Synchronous (called inline) | Can be async — listener runs when event fires |
| Tightly coupled to caller | Decoupled — emitter doesn't know who's listening |
| `add(2, 3)` | `emitter.emit('add', 2, 3)` |

Events enable **loose coupling** — the emitter doesn't need to know about the listeners.

---

**Q31. What is the role of the HTTP module?**

The `http` module allows Node.js to create HTTP servers and make HTTP requests — the foundation of web development in Node.

```js
const http = require('http');

// Make a GET request
http.get('http://jsonplaceholder.typicode.com/todos/1', (res) => {
  let data = '';
  res.on('data', chunk => data += chunk);
  res.on('end', () => console.log(JSON.parse(data)));
});
```

---

**Q32. What is the role of the `createServer` method of the HTTP module?**

`http.createServer()` creates an HTTP server that listens for requests and sends responses.

```js
const http = require('http');

const server = http.createServer((req, res) => {
  const { url, method } = req;

  if (url === '/' && method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>Home Page</h1>');
  } else {
    res.writeHead(404);
    res.end('Not Found');
  }
});

server.listen(3000, () => console.log('Listening on port 3000'));
```

---

## Express Framework - Basics

---

**Q33. What are the advantages of using Express with Node?**

1. **Minimal & flexible** — lightweight, unopinionated.
2. **Routing** — clean route definition for all HTTP methods.
3. **Middleware support** — plug-in architecture for auth, logging, parsing.
4. **Template engines** — EJS, Pug, Handlebars for server-side rendering.
5. **Error handling** — centralized error middleware.
6. **Large ecosystem** — thousands of compatible NPM packages.
7. **REST API** — perfect for building RESTful APIs.

---

**Q34. How do you install Express in a Node project?**

```bash
npm init -y
npm install express

# Optional but recommended:
npm install --save-dev nodemon
```

```js
// index.js
const express = require('express');
const app = express();
app.listen(3000, () => console.log('Running on port 3000'));
```

---

**Q35. How to create an HTTP server using Express.js?**

```js
const express = require('express');
const app = express();

app.use(express.json()); // parse JSON request bodies

app.get('/', (req, res) => res.send('Hello World'));
app.get('/about', (req, res) => res.send('About page'));

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

**Q36. How do you create and start the Express application?**

```js
const express = require('express');

// 1. Create the app instance
const app = express();

// 2. Configure middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// 3. Define routes
app.get('/api/health', (req, res) => res.json({ status: 'OK' }));

// 4. Start listening
app.listen(3000, () => console.log('Express app started on port 3000'));
```

---

## Express Framework - Middleware

---

**Q37. What is middleware in Express.js and when to use it?**

Middleware are functions that execute **during the request-response cycle**, with access to `req`, `res`, and `next`.

Use middleware for:
- Logging requests.
- Authenticating users.
- Parsing request bodies (JSON, form data).
- Handling CORS.
- Compressing responses.
- Error handling.

```js
function logger(req, res, next) {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next(); // MUST call next() or the request hangs
}
app.use(logger);
```

---

**Q38. How to implement middleware in Express?**

```js
// Global middleware (applies to all routes)
app.use(express.json());
app.use(cors());

// Route-specific middleware
app.get('/dashboard', authMiddleware, (req, res) => {
  res.json({ data: 'Protected data' });
});

// Inline middleware
app.use((req, res, next) => {
  req.requestTime = Date.now();
  next();
});
```

---

**Q39. What is the purpose of the `app.use` function in Express?**

`app.use()` **mounts middleware** — it registers a function to run for every request (or for requests to a specific path).

```js
app.use(express.json());                  // all routes
app.use('/api', apiRouter);               // only routes starting with /api
app.use('/static', express.static('public')); // serve static files

// Order matters — middleware runs top to bottom
app.use(logger);      // runs first
app.use(authCheck);   // runs second
app.use(router);      // runs third
```

---

**Q40. What is the purpose of the `next` parameter in Express.js?**

`next` is a function that **passes control to the next middleware** in the stack.

```js
function authMiddleware(req, res, next) {
  const token = req.headers.authorization;
  if (!token) return res.status(401).json({ error: 'Unauthorized' }); // stops here
  req.user = verifyToken(token);
  next(); // continues to the next middleware/route
}

// Passing an error to next() triggers error-handling middleware
function riskyMiddleware(req, res, next) {
  try {
    doSomething();
    next();
  } catch (err) {
    next(err); // skips to error handler
  }
}
```

---

**Q41. How to use middleware globally for a specific route?**

```js
// Apply to all routes under /api
app.use('/api', express.json());
app.use('/api', authMiddleware);

// Apply to a specific HTTP method and route
app.post('/api/users', validateBody, createUser);

// Router-level — apply to all routes in a router
const router = express.Router();
router.use(authMiddleware); // applies to all routes in this router
router.get('/profile', getProfile);
```

---

**Q42. What is the request pipeline in Express?**

The request pipeline is the **ordered sequence of middleware and route handlers** a request passes through before a response is sent.

```
Request → [Logger] → [CORS] → [Body Parser] → [Auth] → [Route Handler] → Response
                                                                     ↓ (on error)
                                                          [Error Handler] → Response
```

Each middleware can:
- Process the request.
- Modify `req` or `res`.
- Call `next()` to continue.
- Send a response to end the cycle.

---

## Express Framework - Types of Middleware

---

**Q43. What are the types of middleware in Express.js?**

1. **Application-level** — `app.use()` / `app.get()` etc.
2. **Router-level** — `router.use()` on an `express.Router()` instance.
3. **Error-handling** — 4 arguments: `(err, req, res, next)`.
4. **Built-in** — `express.json()`, `express.static()`, `express.urlencoded()`.
5. **Third-party** — `cors`, `morgan`, `helmet`, `compression`.

---

**Q44. What is the difference between application-level and router-level middleware?**

| Application-level | Router-level |
|---|---|
| Bound to `app` instance | Bound to `express.Router()` instance |
| `app.use(fn)` | `router.use(fn)` |
| Applies to the whole app | Applies only to routes in that router |
| Defined in main `app.js` | Defined in route files |

```js
// Application-level
app.use(morgan('dev'));

// Router-level
const userRouter = express.Router();
userRouter.use(authMiddleware); // only for user routes
```

---

**Q45. What is error-handling middleware and how to implement it?**

Error-handling middleware has **four parameters**: `(err, req, res, next)`. It must be defined **last** in the middleware stack.

```js
// Must have all 4 params — Express identifies it as error handler
app.use((err, req, res, next) => {
  console.error(err.stack);
  const statusCode = err.statusCode || 500;
  res.status(statusCode).json({
    error: err.message || 'Internal Server Error'
  });
});
```

Triggered by: `next(err)` in any middleware or route handler.

---

**Q46. If you have five middlewares, in which one will you do error handling?**

The **last** middleware should be the error handler. It should be defined **after all routes and other middleware**.

```js
app.use(morgan('dev'));          // 1. Logging
app.use(express.json());         // 2. Body parsing
app.use(authMiddleware);         // 3. Authentication
app.use('/api', routes);         // 4. Routes
app.use(errorHandler);           // 5. Error handling — ALWAYS LAST
```

Any unhandled error passed via `next(err)` from middlewares 1–4 will be caught here.

---

**Q47. What is built-in middleware? How to serve static files from Express.js?**

Express has three built-in middleware functions:

```js
app.use(express.json());                         // parse JSON bodies
app.use(express.urlencoded({ extended: true })); // parse form data
app.use(express.static('public'));               // serve static files

// Static files: files in ./public are served at root
// http://localhost:3000/images/logo.png → ./public/images/logo.png
app.use('/static', express.static(path.join(__dirname, 'public')));
```

---

**Q48. What are third-party middleware? Give some examples.**

Third-party middleware is installed via npm and plugged into Express.

| Package | Purpose |
|---|---|
| `morgan` | HTTP request logging |
| `cors` | Enable CORS headers |
| `helmet` | Security headers |
| `compression` | Gzip response compression |
| `express-rate-limit` | Rate limiting |
| `multer` | File upload handling |
| `cookie-parser` | Parse cookie headers |

```js
const morgan = require('morgan');
const cors = require('cors');
const helmet = require('helmet');

app.use(morgan('dev'));
app.use(cors());
app.use(helmet());
```

---

**Q49. Can you summarize all the types of middleware?**

| Type | How to apply | Purpose |
|---|---|---|
| Application-level | `app.use(fn)` | Runs for all routes in the app |
| Router-level | `router.use(fn)` | Runs for routes in that router |
| Error-handling | `app.use((err,req,res,next)=>{})` | Catches errors — always last |
| Built-in | `express.json()`, `express.static()` | Parsing, serving static files |
| Third-party | npm install + `app.use()` | Logging, security, CORS, etc. |

---

**Q50. What are the advantages of using middleware in Express.js?**

1. **Separation of concerns** — each middleware does one thing.
2. **Reusability** — write once, apply anywhere.
3. **Composability** — chain multiple middleware for complex pipelines.
4. **Clean code** — routes stay focused on business logic.
5. **Centralized error handling** — one place to handle all errors.
6. **Extensibility** — plug in third-party middleware easily.

---

## Express Framework - Routing

---

**Q51. What is routing in Express?**

Routing determines how an application responds to a client request at a particular **URL** (endpoint) and **HTTP method**.

```js
app.get('/users', handler);       // GET /users
app.post('/users', handler);      // POST /users
app.put('/users/:id', handler);   // PUT /users/123
app.delete('/users/:id', handler);// DELETE /users/123
```

---

**Q52. What is the difference between middleware and routing in Express?**

| Middleware | Routing |
|---|---|
| Runs for matching paths (or all) | Runs for a specific path + HTTP method |
| Calls `next()` to continue | Sends a response to end the cycle |
| For cross-cutting concerns | For request handling logic |
| `app.use('/api', fn)` | `app.get('/api/users', fn)` |

---

**Q53. How to implement routing in Express?**

```js
const express = require('express');
const app = express();
app.use(express.json());

// Basic routes
app.get('/', (req, res) => res.send('Home'));
app.get('/about', (req, res) => res.send('About'));
app.post('/submit', (req, res) => res.json({ received: req.body }));

// Route with parameters
app.get('/users/:id', (req, res) => res.json({ id: req.params.id }));

// Route with query string
app.get('/search', (req, res) => res.json({ query: req.query.q }));
// GET /search?q=nodejs → { query: "nodejs" }

app.listen(3000);
```

---

**Q54. How to handle routing in real Express applications?**

Split routes into separate files using `express.Router()`:

```js
// routes/users.js
const router = require('express').Router();
router.get('/', getAllUsers);
router.post('/', createUser);
router.get('/:id', getUserById);
router.put('/:id', updateUser);
router.delete('/:id', deleteUser);
module.exports = router;

// app.js
const userRoutes = require('./routes/users');
app.use('/api/users', userRoutes);
```

---

**Q55. What are route handlers?**

Route handlers are the **callback functions** that execute when a route matches.

```js
// Single handler
app.get('/user', (req, res) => res.send('User'));

// Multiple handlers (middleware chain)
app.get('/user', validateAuth, checkPermission, (req, res) => {
  res.json({ user: req.user });
});

// Array of handlers
const handlers = [validateAuth, checkPermission, getUser];
app.get('/user', handlers);
```

---

**Q56. What are route parameters in Express?**

Route parameters are **named URL segments** prefixed with `:` that capture values from the URL.

```js
app.get('/users/:id', (req, res) => {
  const { id } = req.params;
  res.json({ userId: id });
});
// GET /users/42 → { userId: "42" }

// Multiple parameters
app.get('/users/:userId/posts/:postId', (req, res) => {
  const { userId, postId } = req.params;
  res.json({ userId, postId });
});
// GET /users/1/posts/5 → { userId: "1", postId: "5" }
```

---

## Express Framework - Additional Routing Questions

---

**Q57. What are the router object and router methods?**

`express.Router()` creates a **mini Express application** — a modular, mountable route handler.

```js
const router = express.Router();

// Router methods mirror app methods
router.get('/', handler);
router.post('/', handler);
router.put('/:id', handler);
router.delete('/:id', handler);
router.use(middleware); // router-level middleware

module.exports = router;
```

---

**Q58. What are the types of router methods?**

All HTTP verbs are supported:

```js
router.get(path, handler)      // Read
router.post(path, handler)     // Create
router.put(path, handler)      // Full update
router.patch(path, handler)    // Partial update
router.delete(path, handler)   // Delete
router.all(path, handler)      // Any HTTP method
router.use(path, middleware)   // Middleware for path
```

---

**Q59. What is the difference between `app.get` and `router.get` methods?**

| `app.get` | `router.get` |
|---|---|
| Defined on the main app | Defined on a Router instance |
| Global scope | Scoped to the router's mount path |
| Used in main app file | Used in route module files |
| `app.get('/users', fn)` → `/users` | `router.get('/', fn)` + `app.use('/users', router)` → `/users` |

---

**Q60. What is `Express.Router` in Express.js?**

`Express.Router` is a class that creates **isolated, modular route handlers** — like mini-apps that can be mounted on the main app at a specific path.

```js
// Feature-based routing
const authRouter = require('./routes/auth');
const productRouter = require('./routes/products');
const orderRouter = require('./routes/orders');

app.use('/api/auth', authRouter);
app.use('/api/products', productRouter);
app.use('/api/orders', orderRouter);
```

---

**Q61. Share a real application use of routing.**

**E-commerce REST API structure:**

```
GET    /api/products          — list all products
GET    /api/products/:id      — get one product
POST   /api/products          — create product (admin)
PUT    /api/products/:id      — update product (admin)
DELETE /api/products/:id      — delete product (admin)

POST   /api/auth/register     — register user
POST   /api/auth/login        — login, get JWT

GET    /api/orders            — get user's orders (auth required)
POST   /api/orders            — place an order (auth required)
```

---

**Q62. What is route chaining in Express?**

Route chaining uses `.route()` to define multiple HTTP methods on the same path without repeating the path.

```js
app.route('/users/:id')
  .get((req, res) => res.json({ user: 'fetched' }))
  .put((req, res) => res.json({ user: 'updated' }))
  .delete((req, res) => res.json({ user: 'deleted' }));
```

---

**Q63. What is route nesting in Express?**

Route nesting is mounting a router inside another router, creating hierarchical URL structures.

```js
// /api/users/:userId/posts
const postRouter = express.Router({ mergeParams: true });
postRouter.get('/', (req, res) => {
  res.json({ userId: req.params.userId, posts: [] });
});

const userRouter = express.Router();
userRouter.use('/:userId/posts', postRouter); // nest post router

app.use('/api/users', userRouter);
```

---

**Q64. How to implement route nesting in Express?**

```js
// routes/posts.js
const router = express.Router({ mergeParams: true }); // inherit parent params
router.get('/', async (req, res) => {
  const posts = await Post.find({ userId: req.params.userId });
  res.json(posts);
});
module.exports = router;

// routes/users.js
const postRouter = require('./posts');
const router = express.Router();
router.use('/:userId/posts', postRouter);
module.exports = router;

// app.js
app.use('/api/users', require('./routes/users'));
// Result: GET /api/users/123/posts
```

---

## Express Framework - Template Engines

---

**Q65. What are template engines in Express?**

Template engines allow you to generate dynamic HTML on the server by combining HTML templates with data.

```js
app.set('view engine', 'ejs');
app.set('views', './views');

app.get('/profile', (req, res) => {
  res.render('profile', { name: 'Alice', age: 30 }); // renders views/profile.ejs
});
```

---

**Q66. Name some template engine libraries.**

| Engine | Syntax style |
|---|---|
| **EJS** | `<%= variable %>` — closest to HTML |
| **Pug (Jade)** | Indentation-based, no closing tags |
| **Handlebars** | `{{ variable }}` — logic-less |
| **Mustache** | `{{ variable }}` — minimal logic |
| **Nunjucks** | Django/Jinja2-like syntax |

---

**Q67. How to implement the EJS templating engine?**

```bash
npm install ejs
```

```js
// app.js
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

app.get('/users', async (req, res) => {
  const users = await User.find();
  res.render('users', { users, title: 'User List' });
});
```

```html
<!-- views/users.ejs -->
<h1><%= title %></h1>
<ul>
  <% users.forEach(user => { %>
    <li><%= user.name %> — <%= user.email %></li>
  <% }) %>
</ul>
```

---

## REST API - Basics

---

**Q68. What are REST and RESTful API?**

**REST (Representational State Transfer)** is an architectural style for designing networked applications using standard HTTP.

**RESTful API** — an API that follows REST constraints:
- **Stateless** — each request contains all info needed.
- **Client-Server** — separated concerns.
- **Uniform Interface** — consistent URL patterns and HTTP methods.
- **Cacheable** — responses can be cached.
- **Layered System** — client doesn't know about backend layers.

---

**Q69. What are the HTTP request and response structure?**

**HTTP Request:**
- **Method** — GET, POST, PUT, DELETE, PATCH
- **URL** — `/api/users/1`
- **Headers** — `Authorization`, `Content-Type`, `Accept`
- **Body** — JSON payload (POST/PUT/PATCH)
- **Query params** — `/search?q=node&limit=10`

**HTTP Response:**
- **Status code** — 200, 201, 400, 401, 404, 500
- **Headers** — `Content-Type: application/json`
- **Body** — JSON data

---

**Q70. What are the top five REST guidelines and their advantages?**

1. **Statelessness** — server doesn't store session state → scalable.
2. **Uniform Interface** — consistent URLs and methods → predictable, easy to use.
3. **Client-Server separation** — frontend and backend can evolve independently.
4. **Cacheability** — responses marked as cacheable → better performance.
5. **Layered System** — client talks to one endpoint, unaware of backend complexity.

---

**Q71. What is the difference between REST API and SOAP API?**

| REST | SOAP |
|---|---|
| Architectural style | Protocol |
| JSON, XML, HTML | XML only |
| Stateless | Can be stateful |
| Uses standard HTTP | Has own specification (WSDL) |
| Lightweight, fast | Heavy, verbose |
| Modern web/mobile APIs | Enterprise, banking, legacy |

---

## REST API - HTTP Methods and Status Codes

---

**Q72. What are HTTP verbs and HTTP methods?**

HTTP methods (verbs) define the **action** being performed on a resource:

| Method | Action | Body? |
|---|---|---|
| GET | Retrieve data | No |
| POST | Create new resource | Yes |
| PUT | Replace entire resource | Yes |
| PATCH | Partial update | Yes |
| DELETE | Remove resource | No |
| HEAD | Like GET but no response body | No |
| OPTIONS | Describe allowed methods (CORS preflight) | No |

---

**Q73. What are the GET, POST, PUT, and DELETE HTTP methods?**

```js
// GET — read (no body)
app.get('/users', (req, res) => res.json(users));

// POST — create (sends body)
app.post('/users', (req, res) => {
  const user = createUser(req.body);
  res.status(201).json(user);
});

// PUT — full replace
app.put('/users/:id', (req, res) => {
  const user = replaceUser(req.params.id, req.body);
  res.json(user);
});

// DELETE — remove
app.delete('/users/:id', (req, res) => {
  deleteUser(req.params.id);
  res.status(204).send();
});
```

---

**Q74. What is the difference between the PUT and PATCH methods?**

| PUT | PATCH |
|---|---|
| **Replaces** the entire resource | **Partially updates** the resource |
| Must send all fields | Send only changed fields |
| Idempotent | Idempotent (usually) |

```js
// PUT — must send full user object
// PUT /users/1 → { name: "Alice", email: "alice@example.com", age: 30 }

// PATCH — send only what changes
// PATCH /users/1 → { age: 31 }
```

---

**Q75. Explain the concept of idempotent in RESTful API.**

An operation is **idempotent** if making the same request multiple times produces the same result.

| Method | Idempotent |
|---|---|
| GET | Yes |
| PUT | Yes |
| DELETE | Yes |
| POST | **No** — creates new resource each time |
| PATCH | Usually yes |

Example: `DELETE /users/1` called 10 times — user is deleted on first call; subsequent calls return 404, but the state (user doesn't exist) is the same.

---

**Q76. What is the role of status codes in RESTful APIs?**

Status codes communicate the **result of an HTTP request**.

| Range | Category | Examples |
|---|---|---|
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirection | 301 Moved Permanently, 304 Not Modified |
| 4xx | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| 5xx | Server Error | 500 Internal Server Error, 503 Service Unavailable |

---

## REST API - CORS, Serialization, and Other Topics

---

**Q77. What is CORS in RESTful APIs?**

**CORS (Cross-Origin Resource Sharing)** is a browser security mechanism that restricts HTTP requests made from a **different origin** (domain, port, or protocol) than the server.

A browser blocks requests from `http://frontend.com` to `http://api.com` unless the server explicitly allows it via CORS headers.

---

**Q78. How to remove CORS restriction on RESTful API?**

```bash
npm install cors
```

```js
const cors = require('cors');

// Allow all origins (development only)
app.use(cors());

// Allow specific origin (production)
app.use(cors({
  origin: 'https://myfrontend.com',
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true
}));
```

---

**Q79. What are serialization and deserialization?**

- **Serialization:** Converting a JavaScript object → string/bytes for transmission (e.g., JSON.stringify).
- **Deserialization:** Converting string/bytes back → JavaScript object (e.g., JSON.parse).

```js
const obj = { name: 'Alice', age: 30 };
const json = JSON.stringify(obj);   // serialization → '{"name":"Alice","age":30}'
const parsed = JSON.parse(json);    // deserialization → { name: 'Alice', age: 30 }
```

---

**Q80. What are the types of serialization?**

| Format | Use case |
|---|---|
| **JSON** | Web APIs — human-readable, universal |
| **XML** | SOAP, legacy systems |
| **Binary (Protocol Buffers/MessagePack)** | High performance, compact |
| **CSV** | Spreadsheets, data export |
| **YAML** | Config files |

JSON is the standard for modern REST APIs.

---

**Q81. How to serialize and deserialize in Node.js?**

```js
// JSON Serialization
const user = { id: 1, name: 'Alice' };
const jsonString = JSON.stringify(user);         // '{"id":1,"name":"Alice"}'
const parsedUser = JSON.parse(jsonString);        // { id: 1, name: 'Alice' }

// Pretty print
const pretty = JSON.stringify(user, null, 2);

// Express auto-serializes with res.json()
app.get('/user', (req, res) => res.json(user)); // auto JSON.stringify

// Express auto-deserializes with express.json() middleware
app.use(express.json()); // req.body is already parsed object
```

---

**Q82. Explain the concept of versioning in RESTful APIs.**

API versioning allows you to **evolve your API** without breaking existing clients.

**Strategies:**

1. **URI versioning** (most common):
```
GET /api/v1/users
GET /api/v2/users
```

2. **Query parameter:**
```
GET /api/users?version=2
```

3. **Header versioning:**
```
Accept: application/vnd.api+json;version=2
```

```js
app.use('/api/v1', v1Routes);
app.use('/api/v2', v2Routes);
```

---

**Q83. What is an API document? What are the popular documentation formats?**

An API document describes **how to use an API** — endpoints, methods, parameters, request/response examples, auth.

**Popular formats:**
| Tool | Description |
|---|---|
| **Swagger / OpenAPI** | Most popular — YAML/JSON spec, interactive UI |
| **Postman** | API testing + documentation |
| **Redoc** | Beautiful OpenAPI documentation |
| **API Blueprint** | Markdown-based |

```yaml
# OpenAPI example
paths:
  /users:
    get:
      summary: Get all users
      responses:
        '200':
          description: List of users
```

---

**Q84. What is the typical structure of a REST API project in Node?**

```
my-api/
├── src/
│   ├── config/          # DB, env config
│   ├── controllers/     # request/response handlers
│   ├── middleware/      # auth, logging, validation
│   ├── models/          # Mongoose/DB schemas
│   ├── routes/          # Express Router files
│   ├── services/        # business logic
│   └── utils/           # helper functions
├── .env
├── app.js               # Express app setup
├── server.js            # server entry point (listen)
└── package.json
```

---

## REST API - Authentication and Authorization

---

**Q85. What are authentication and authorization?**

| Authentication | Authorization |
|---|---|
| **Who are you?** — verify identity | **What can you do?** — verify permissions |
| Login with email/password | Check if user can access a resource |
| Issues a token/session | Uses token/role to allow or deny |
| "Prove you're Alice" | "Alice can view, but not delete" |

---

**Q86. What are the types of authentication in Node?**

1. **Basic Auth** — Base64 encoded username:password in header.
2. **Session-based** — Server stores session; client holds session ID in cookie.
3. **Token-based (JWT)** — Stateless; server issues signed token; client sends it with every request.
4. **API Key** — Static key issued to clients.
5. **OAuth 2.0** — Third-party auth (Google, GitHub login).
6. **Passport.js** — middleware supporting all above strategies.

---

**Q87. What is basic authentication?**

Basic auth sends credentials as a **Base64-encoded** string in the `Authorization` header.

```
Authorization: Basic YWxpY2U6cGFzc3dvcmQ=
// decodes to: alice:password
```

```js
app.use((req, res, next) => {
  const authHeader = req.headers.authorization;
  if (!authHeader?.startsWith('Basic ')) return res.status(401).send('Unauthorized');
  const [username, password] = Buffer.from(authHeader.slice(6), 'base64')
    .toString()
    .split(':');
  if (username !== 'admin' || password !== 'secret') return res.status(401).send('Unauthorized');
  next();
});
```

**Not recommended for production** — use JWT or OAuth instead. Always use over HTTPS.

---

**Q88. What are the security risks of storing passwords as plain text?**

1. **Database breach** — attacker gets all passwords immediately readable.
2. **Credential stuffing** — same passwords used on other sites.
3. **Insider threat** — employees can see user passwords.
4. **Compliance violations** — GDPR, HIPAA, PCI-DSS require secure storage.

**Never store plain text passwords.** Always hash with a slow algorithm like **bcrypt**.

---

**Q89. What is the role of hashing and salt in securing passwords?**

- **Hashing:** One-way transformation of a password into a fixed-length string. Cannot be reversed.
- **Salt:** A random string added to the password BEFORE hashing — ensures identical passwords produce different hashes (defeats rainbow table attacks).

```
password: "secret123"
salt: "x7k2m"
hash: bcrypt("secret123" + "x7k2m") → "$2b$10$..."
```

bcrypt automatically generates and stores the salt with the hash.

---

**Q90. How to create hashed passwords in Node.js?**

```bash
npm install bcrypt
```

```js
const bcrypt = require('bcrypt');

// Hash password on registration
async function hashPassword(plainPassword) {
  const saltRounds = 12; // higher = slower = more secure
  const hash = await bcrypt.hash(plainPassword, saltRounds);
  return hash; // store this in the database
}

// Verify password on login
async function verifyPassword(plainPassword, storedHash) {
  const isMatch = await bcrypt.compare(plainPassword, storedHash);
  return isMatch; // true or false
}

// Usage
const hash = await hashPassword('myPassword123');
const valid = await verifyPassword('myPassword123', hash); // true
```

---

**Q91. What is API key authentication?**

An **API key** is a unique, static token issued to a client that identifies and authenticates the client on every request.

```js
// Client sends key in header or query param
// Header: X-API-Key: abc123secret
// Query: GET /api/data?apiKey=abc123secret

function apiKeyAuth(req, res, next) {
  const apiKey = req.headers['x-api-key'] || req.query.apiKey;
  if (!apiKey || !isValidKey(apiKey)) {
    return res.status(401).json({ error: 'Invalid API Key' });
  }
  next();
}

app.use(apiKeyAuth);
```

Used by: public APIs (Google Maps, Stripe, OpenAI).

---

**Q92. What is token-based authentication and JWT authentication?**

**Token-based auth:** Client receives a token on login and sends it with every subsequent request. Server validates the token — no session storage needed (stateless).

**JWT (JSON Web Token):** A self-contained, signed token encoding user data.

```js
const jwt = require('jsonwebtoken');

// Login — issue token
app.post('/login', async (req, res) => {
  const user = await User.findOne({ email: req.body.email });
  const valid = await bcrypt.compare(req.body.password, user.password);
  if (!valid) return res.status(401).json({ error: 'Invalid credentials' });

  const token = jwt.sign(
    { userId: user._id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '1h' }
  );
  res.json({ token });
});

// Protected route
function auth(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ error: 'No token' });
  req.user = jwt.verify(token, process.env.JWT_SECRET);
  next();
}
```

---

**Q93. What are the parts of the JWT token?**

A JWT has three Base64URL-encoded parts separated by dots:

```
header.payload.signature
eyJhbGci...  .eyJ1c2VySWQi...  .SflKxwRJSMeK...
```

1. **Header** — algorithm used to sign: `{ "alg": "HS256", "typ": "JWT" }`
2. **Payload** — claims (user data): `{ "userId": "123", "role": "admin", "exp": 1700000000 }`
3. **Signature** — `HMACSHA256(base64(header) + "." + base64(payload), secret)` — verifies integrity.

The payload is **encoded, not encrypted** — don't store sensitive data in it.

---

**Q94. Where does the JWT token reside in the request?**

The JWT is typically sent in the **`Authorization` header** using the **Bearer** scheme:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

```js
// Extract in middleware
const token = req.headers.authorization?.split(' ')[1]; // "Bearer <token>"
```

**Alternative locations (less common):**
- `Cookie` header (httpOnly cookie — more secure against XSS).
- Query parameter (not recommended — logs and URLs expose tokens).

---

## Random Topics - Error Handling and Debugging

---

**Q95. What is error handling? In how many ways can you implement it in Node?**

Error handling is the process of **anticipating, detecting, and recovering from errors** to prevent app crashes.

**4 ways to implement in Node:**
1. `try-catch-finally` — synchronous code.
2. Error-first callbacks — `(err, data) => {}`.
3. Promise `.catch()` — async promise chains.
4. `async/await` with `try-catch` — cleanest approach.
5. Express error-handling middleware — centralized HTTP error handling.

---

**Q96. How to handle errors in synchronous operations using try-catch-finally?**

```js
function parseJSON(str) {
  try {
    const result = JSON.parse(str);
    return result;
  } catch (err) {
    console.error('Invalid JSON:', err.message);
    throw new Error('Parsing failed'); // re-throw if caller needs to handle it
  } finally {
    console.log('Parsing attempt complete'); // always runs
  }
}

// Usage
try {
  const data = parseJSON('{ invalid }');
} catch (err) {
  console.error(err.message); // 'Parsing failed'
}
```

---

**Q97. What are error-first callbacks?**

A Node.js convention where the **first argument of a callback is reserved for an error** object. If no error, it's `null`.

```js
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err.message);
    return; // stop execution
  }
  console.log('File contents:', data);
});

// Custom error-first callback function
function divide(a, b, callback) {
  if (b === 0) return callback(new Error('Division by zero'));
  callback(null, a / b); // null = no error
}

divide(10, 2, (err, result) => {
  if (err) return console.error(err.message);
  console.log(result); // 5
});
```

---

**Q98. How to handle errors using Promises?**

```js
// .catch() on a promise chain
fetchUser(id)
  .then(user => fetchPosts(user.id))
  .then(posts => processPosts(posts))
  .catch(err => {
    console.error('Something failed:', err.message);
  })
  .finally(() => console.log('Done'));

// Promise.reject() to create a failed promise
function validateAge(age) {
  if (age < 0) return Promise.reject(new Error('Invalid age'));
  return Promise.resolve(age);
}

validateAge(-1).catch(err => console.error(err.message)); // 'Invalid age'
```

---

**Q99. How to handle errors using async/await?**

```js
// Wrap in try-catch
async function getUser(id) {
  try {
    const user = await User.findById(id);
    if (!user) throw new Error('User not found');
    return user;
  } catch (err) {
    throw err; // re-throw for caller to handle
  }
}

// In Express with async routes — always catch errors
app.get('/users/:id', async (req, res, next) => {
  try {
    const user = await getUser(req.params.id);
    res.json(user);
  } catch (err) {
    next(err); // pass to error-handling middleware
  }
});

// Or use a wrapper utility
const asyncHandler = (fn) => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get('/users/:id', asyncHandler(async (req, res) => {
  const user = await getUser(req.params.id);
  res.json(user);
}));
```

---

**Q100. How can you debug a Node.js application?**

**1. Console logging (basic):**
```js
console.log('Value:', variable);
console.error('Error:', err);
console.table(arrayOfObjects);
```

**2. Node.js built-in debugger:**
```bash
node --inspect index.js
# Open chrome://inspect in Chrome
```

**3. VS Code debugger:**
- Add `"type": "node"` launch config in `.vscode/launch.json`.
- Set breakpoints in the editor.

**4. `debug` npm package:**
```bash
npm install debug
```
```js
const debug = require('debug')('app:server');
debug('Server started on port %d', 3000);
// Run: DEBUG=app:* node index.js
```

**5. Logging with Winston/Morgan:**
```js
const morgan = require('morgan');
app.use(morgan('dev')); // logs every HTTP request
```

**Best practices:**
- Use structured logging (JSON) in production.
- Never leave `console.log` in production code.
- Use environment-based logging levels (error/warn/info/debug).

---

## Additional Topics

---

**Q101. What is the Buffer class in Node.js and when is it used?**

A **Buffer** is a temporary in-memory storage for raw binary data — used when dealing with streams, file I/O, and network packets before the data can be processed.

```js
// Create a buffer from a string
const buf = Buffer.from('Hello', 'utf8');
console.log(buf);            // <Buffer 48 65 6c 6c 6f>
console.log(buf.toString()); // 'Hello'

// Allocate an empty buffer of fixed size
const buf2 = Buffer.alloc(10); // 10 bytes, zero-filled

// Convert between encodings
const base64 = Buffer.from('Hello').toString('base64'); // 'SGVsbG8='
const decoded = Buffer.from('SGVsbG8=', 'base64').toString('utf8'); // 'Hello'
```

Use cases: reading image/audio files as binary, handling network protocol data, cryptographic operations.

---

**Q102. What is the difference between `setImmediate()` and `setTimeout()`?**

Both schedule a callback, but they run at **different phases of the event loop**.

| | `setTimeout(fn, 0)` | `setImmediate(fn)` |
|---|---|---|
| Event loop phase | **Timers** phase | **Check** phase (after I/O) |
| Order vs each other | Non-deterministic in main module | Always runs before `setTimeout` inside I/O callbacks |

```js
// In the main module — order is non-deterministic
setTimeout(() => console.log('setTimeout'), 0);
setImmediate(() => console.log('setImmediate'));

// Inside an I/O callback — setImmediate ALWAYS runs first
const fs = require('fs');
fs.readFile(__filename, () => {
  setTimeout(() => console.log('setTimeout'), 0);
  setImmediate(() => console.log('setImmediate')); // always first here
});
```

---

**Q103. What are the types of streams in Node.js?**

Streams process data **chunk by chunk** instead of loading it all into memory — ideal for large files or real-time data.

| Type | Description | Example |
|---|---|---|
| **Readable** | Read data from a source | `fs.createReadStream()` |
| **Writable** | Write data to a destination | `fs.createWriteStream()` |
| **Duplex** | Both readable and writable | TCP socket |
| **Transform** | Duplex that transforms data | `zlib.createGzip()` (compression) |

```js
const fs = require('fs');
const zlib = require('zlib');

// Pipe: read → compress → write (memory-efficient)
fs.createReadStream('large-file.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('large-file.txt.gz'));

// Stream events
const readable = fs.createReadStream('file.txt', 'utf8');
readable.on('data', chunk => console.log('Chunk:', chunk));
readable.on('end', () => console.log('Done'));
readable.on('error', err => console.error(err));
```
