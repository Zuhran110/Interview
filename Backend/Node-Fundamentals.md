# Node.js — Fundamentals

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
