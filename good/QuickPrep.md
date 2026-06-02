# ⚡ Last-Minute Quick Prep — Must-Know Interview Questions

> The highest-frequency questions only — review these if you're short on time.
> Full answers & more questions: [JavaScript.md](JavaScript.md) · [Backend.md](Backend.md) · [React.md](React.md) · [MERN.md](MERN.md) · [Coding.md](Coding.md)

---

## JavaScript

**1. `var` vs `let` vs `const`?**
`var` is function-scoped & hoisted as `undefined`; `let`/`const` are block-scoped with a Temporal Dead Zone. `const` can't be reassigned (but objects/arrays it points to are still mutable).

**2. `==` vs `===`?**
`==` compares after type coercion (`'5' == 5` → true); `===` compares value **and** type (`'5' === 5` → false). Always prefer `===`.

**3. What is hoisting?**
JS moves declarations to the top of their scope. `var` → hoisted as `undefined`; function declarations → fully hoisted; `let`/`const` → hoisted but in the TDZ (throws if accessed early).

**4. What is a closure?**
A function that "remembers" variables from its outer (lexical) scope even after that outer function has returned. Basis for data privacy, currying, and `useState`-style state.
```js
function counter() { let c = 0; return () => ++c; }
const next = counter(); next(); // 1  next(); // 2
```

**5. `null` vs `undefined`?**
`undefined` = declared but not assigned (JS default). `null` = intentional "no value" set by the developer. `null == undefined` is true, but `null === undefined` is false.

**6. Arrow functions & `this`?**
Arrow functions have **no own `this`** — they inherit it from the enclosing scope. Great for callbacks; not for object methods or where you need a dynamic `this`.

**7. `map()` vs `forEach()`?**
`map` returns a **new transformed array**; `forEach` returns `undefined` and is for side effects only. Use `map` when you need the result.

**8. Spread vs rest (`...`)?**
**Spread** expands an iterable (`[...arr, 4]`, `{...obj}`); **rest** collects remaining items into an array (`function sum(...nums)`).

**9. Promises & `async/await`?**
A Promise represents a future value (`pending → fulfilled/rejected`). `async/await` is syntactic sugar over promises; wrap `await` in `try/catch` for errors.
```js
async function getUser(id) {
  try { return await fetch(`/users/${id}`).then(r => r.json()); }
  catch (e) { console.error(e); }
}
```

**10. `call`, `apply`, `bind`?**
All set `this`. `call(this, a, b)` invokes now with args listed; `apply(this, [a,b])` invokes now with an args array; `bind` returns a **new function** with `this` fixed (doesn't invoke).

**11. What is type coercion?**
Automatic type conversion: `'5' + 3` → `'53'` (to string), `'5' - 3` → `2` (to number). Avoid surprises by using `===`.

**12. Higher-order function?**
A function that takes a function as an argument and/or returns a function — e.g. `map`, `filter`, `reduce`, or a `multiplier(factor)` that returns a function.

**13. Event delegation, bubbling & capturing?**
Events bubble from target → root by default (capturing is root → target). **Delegation** = one listener on a parent that uses `event.target` to handle many children (works for dynamically added elements).

**14. Deep vs shallow copy?**
Shallow (`{...obj}`, `Object.assign`) copies only the top level — nested objects stay shared. Deep (`structuredClone(obj)`) copies every level independently.

**15. Debounce vs throttle?** *(very common)*
**Debounce** = run only after activity stops (e.g. search input). **Throttle** = run at most once per interval (e.g. scroll/resize). Both limit how often a function fires.

**16. `Set` vs `Map`?**
`Set` = collection of **unique values** (`[...new Set(arr)]` dedupes). `Map` = key-value pairs where **keys can be any type** and insertion order is preserved.

---

## Node.js & Express

**17. What is Node.js?**
A JavaScript runtime built on Chrome's V8 engine that runs JS on the server, using an **event-driven, non-blocking I/O** model — ideal for scalable, I/O-heavy apps.

**18. Why is Node single-threaded? How does it handle concurrency?**
JS runs on one main thread; the **event loop** + libuv thread pool handle async I/O in the background, so thousands of connections are served without blocking. Use **Worker Threads** for CPU-heavy work.

**19. Synchronous vs asynchronous?**
Sync blocks the thread until each operation finishes (`fs.readFileSync`); async initiates and continues, running a callback/promise later (`fs.readFile`). Async keeps the server responsive.

**20. What is middleware in Express?**
Functions that run during the request-response cycle with access to `(req, res, next)`. Used for logging, auth, body parsing, CORS, error handling. Must call `next()` or send a response.

**21. How does error-handling middleware work?**
It has **four** params `(err, req, res, next)` and must be defined **last**. Any `next(err)` jumps straight to it.
```js
app.use((err, req, res, next) =>
  res.status(err.statusCode || 500).json({ error: err.message }));
```

**22. What is REST? Key principles?**
An architectural style over HTTP that is **stateless**, has a **uniform interface** (consistent URLs + HTTP verbs), is **client-server**, **cacheable**, and **layered**.

**23. PUT vs PATCH?**
`PUT` **replaces** the whole resource (send all fields); `PATCH` **partially updates** it (send only changed fields). Both are idempotent.

**24. What is idempotency?**
Same request made repeatedly → same end state. GET, PUT, DELETE are idempotent; **POST is not** (creates a new resource each call).

**25. Authentication vs authorization?**
Authentication = **who are you?** (verify identity / login). Authorization = **what can you do?** (verify permissions / roles).

**26. What is JWT & how does token auth work?**
On login the server issues a signed token (`header.payload.signature`); the client sends it as `Authorization: Bearer <token>` on each request. Stateless — no server session needed. Payload is **encoded, not encrypted**.

**27. How do you store passwords securely?**
Never plain text. **Hash** with a slow algorithm like **bcrypt** plus a per-password **salt** (defeats rainbow tables). Compare with `bcrypt.compare`.

**28. What is CORS?**
A browser security mechanism that blocks requests to a different origin unless the server allows it via CORS headers. In Express: `app.use(cors({ origin: '...' }))`.

**29. What are streams?**
Process data **chunk by chunk** instead of loading it all in memory. Types: Readable, Writable, Duplex, Transform. Great for large files (`readStream.pipe(writeStream)`).

---

## React

**30. What is React & the Virtual DOM?**
A JS library for building component-based UIs. The Virtual DOM is an in-memory copy React **diffs** against the previous version, then updates only the changed real-DOM nodes (reconciliation).

**31. What is JSX?**
HTML-like syntax inside JS that Babel compiles to `React.createElement()` calls. Lets you describe UI declaratively.

**32. Props vs state?**
**Props** are read-only inputs passed parent → child. **State** is internal, mutable data that triggers a re-render when changed via its setter.

**33. How does `useState` work?**
`const [val, setVal] = useState(initial)` holds local state; calling `setVal` re-renders the component with the new value (state persists across renders).

**34. `useEffect` & the dependency array?**
Runs side effects after render. `[]` → once on mount; `[dep]` → on mount + when `dep` changes; no array → after every render. Return a function for cleanup (unmount).

**35. Controlled vs uncontrolled components?**
Controlled: input value is driven by React state (`value` + `onChange`) — single source of truth. Uncontrolled: the DOM holds the value, read via a `ref` (good for file inputs).

**36. Why are `key` props important in lists?**
Keys give each item a stable identity so React can tell what changed, was added, or removed. Use a **stable unique id**, not the array index.

**37. `useMemo` vs `useCallback`?**
`useMemo` memoizes a computed **value**; `useCallback` memoizes a **function reference**. Use for expensive work or stable props to memoized children — don't over-use them.

**38. What is prop drilling & how do you avoid it?**
Passing props through many intermediate layers that don't need them. Avoid with **Context API**, state libraries (Redux/Zustand), or component composition.

**39. What is the Context API (`useContext`)?**
Shares values (auth, theme, locale) across the tree without prop drilling. A `Provider` supplies the value; `useContext(MyContext)` consumes it.

**40. How do you optimize React performance?**
`React.memo` (skip re-renders on equal props), `useMemo`/`useCallback`, code-splitting with `lazy`/`Suspense`, list virtualization (`react-window`), stable keys, and avoiding inline functions in hot paths.

**41. What is a Higher-Order Component (HOC)?**
A function that takes a component and returns an enhanced one — for reusing logic like auth guards, loaders, or logging. `withAuth(Component)`.

**42. `useRef` vs `useState`?**
`useRef` holds a mutable `.current` that persists across renders **without** re-rendering (DOM refs, timer IDs, previous values). `useState` re-renders on change.

**43. Code splitting with `lazy` + `Suspense`?**
`const X = lazy(() => import('./X'))` loads a component on demand; wrap it in `<Suspense fallback={<Spinner/>}>` to show a loader while its chunk downloads. Improves initial load.

---

## MongoDB / Web Architecture

**44. SQL vs NoSQL (MongoDB)?**
SQL = fixed schema, tables/rows, native JOINs, vertical scaling. MongoDB = flexible JSON-like documents, dynamic schema, horizontal scaling (sharding). NoSQL suits variable/large-scale data.

**45. Document vs Collection?**
A **document** is one record (BSON, ≈ a row); a **collection** is a group of documents (≈ a table).

**46. REST vs GraphQL?**
REST has fixed endpoints and can over/under-fetch. GraphQL lets the client request **exactly the fields it needs** in one query — good for complex UIs and mobile.

**47. What is the aggregation pipeline?**
Processes documents through stages: `$match` (filter), `$group` (aggregate), `$project` (reshape), `$sort`, `$lookup` (join). Used for analytics/reporting.

**48. Replica set vs sharding?**
**Replica set** = copies of the same data (Primary + Secondaries) for redundancy & failover. **Sharding** = partitions data across servers by a shard key for horizontal scaling.

---

## Coding (practice these — answers in [Coding.md](Coding.md))

- Reverse a string
- Check if a string is a palindrome
- Factorial (recursion)
- Count character frequency in a string
- Sum / find max in an array
- Remove duplicates (`[...new Set(arr)]`)
- FizzBuzz

---

*Tip: For each answer above, be ready to give one concrete example or trade-off — interviewers probe the "why," not just the definition.*
