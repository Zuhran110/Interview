# Express — Basics, Middleware & Routing

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
