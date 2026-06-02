# Authentication & Security

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
