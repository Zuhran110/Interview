# Error Handling & Advanced Topics

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
