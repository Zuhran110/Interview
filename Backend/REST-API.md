# REST API

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
