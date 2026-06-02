# React — Advanced, Code Splitting & Optimization

---

## Code Splitting — Part 14

---

**Q86. What is Code Splitting in React?**

**Code splitting** is a technique to break your app's JavaScript bundle into **smaller chunks** that are loaded **on demand**, rather than loading everything upfront. This improves initial load time.

**Use case:** A large admin dashboard — you don't load the Reports page code until the user navigates to it.

---

**Q87. How to implement Code Splitting in React?**

Using **dynamic `import()`** with `React.lazy()`:

```jsx
import React, { lazy, Suspense } from 'react';

const LazyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

Webpack automatically splits the bundle when it encounters dynamic `import()`.

---

**Q88. What is the role of Lazy and Suspense in React?**

- **`React.lazy()`** — lets you define a component that is loaded **dynamically** (only when first rendered).
- **`<Suspense>`** — wraps lazy components and shows a **fallback UI** (like a spinner) while the component's code is being loaded.

They always work together — `lazy` without `Suspense` will throw an error.

---

**Q89. What are the Pros and Cons of Code Splitting?**

**Pros:**
- Faster initial page load
- Only load code when needed
- Better performance on slow networks
- Smaller initial bundle size

**Cons:**
- Added complexity in routing/setup
- Delay on first load of a lazy chunk
- Error handling needed for failed chunk loads
- Over-splitting can cause too many network requests

---

**Q90. What is the role of the import() function in Code Splitting?**

`import()` is a **dynamic import** — it's a Promise-based function that loads a module asynchronously at runtime instead of at build time.

```jsx
// Static import (loads at start)
import HeavyChart from './HeavyChart';

// Dynamic import (loads when needed)
const HeavyChart = lazy(() => import('./HeavyChart'));
```

Webpack detects `import()` and creates a **separate chunk** file for it.

---

**Q91. What is the purpose of the fallback prop in Suspense?**

The `fallback` prop defines what to **display while the lazy component is loading** (downloading its chunk).

```jsx
<Suspense fallback={<Spinner />}>
  <LazyPage />
</Suspense>
```

Common fallback UIs: loading spinners, skeleton screens, "Loading..." text.

---

## Others — Part 15

---

**Q94. What is a Higher-Order Component (HOC) in React?**

A **Higher-Order Component** is a function that takes a component and returns a **new enhanced component**. It's a pattern for reusing component logic.

```jsx
// HOC that adds loading logic
const withLoader = (WrappedComponent) => {
  return function WithLoaderComponent({ isLoading, ...props }) {
    if (isLoading) return <Spinner />;
    return <WrappedComponent {...props} />;
  };
};

const UserListWithLoader = withLoader(UserList);

// Usage
<UserListWithLoader isLoading={true} users={users} />
```

**Use cases:** Authentication guards, logging, theming, data fetching wrappers.

---

**Q95. What are the 5 ways to style React components? Explain inline styles.**

1. **Inline styles** — style object passed directly to element.
2. **CSS Modules** — scoped CSS files (`*.module.css`).
3. **Styled Components** — CSS-in-JS library.
4. **Tailwind CSS** — utility-first class names.
5. **Regular CSS / SASS** — global stylesheets imported in components.

**Inline styles:**
```jsx
<div style={{ color: 'blue', fontSize: '16px', marginTop: '10px' }}>
  Hello
</div>
```
Note: Properties are camelCase, values are strings (except unitless numbers).

---

**Q96. What are the differences between React and React Native?**

| React | React Native |
|---|---|
| Builds **web apps** | Builds **mobile apps** (iOS & Android) |
| Renders to browser DOM | Renders to native mobile components |
| Uses HTML elements (`div`, `p`) | Uses native components (`View`, `Text`) |
| CSS for styling | StyleSheet API |
| Runs in browser | Runs on mobile device |

**Same:** JSX syntax, component model, hooks, state management — same mental model.

---

**Q98. What are the top 3 ways to achieve state management? When to use what?**

| Solution | When to use |
|---|---|
| **useState / useReducer** | Local component state — simple forms, toggles, counters |
| **Context API** | Global state for small-medium apps — auth, theme, locale |
| **Redux / Zustand** | Large apps with complex, shared, frequently-changing state |

**Rule of thumb:** Start with `useState` → lift to Context if needed → reach for Redux only when Context becomes a performance bottleneck.

---

**Q99. How can you implement authentication in a React application?**

1. **Login form** → send credentials to API → receive JWT token.
2. **Store token** in `localStorage` or an httpOnly cookie.
3. **Create AuthContext** to share auth state (user, token) app-wide.
4. **Protected Routes** — redirect to login if not authenticated.

```jsx
const ProtectedRoute = ({ children }) => {
  const { isAuthenticated } = useContext(AuthContext);
  return isAuthenticated ? children : <Navigate to="/login" />;
};

<Route path="/dashboard" element={<ProtectedRoute><Dashboard /></ProtectedRoute>} />
```

---

**Q100. What is the use of React Profiler?**

The **React Profiler** is a DevTools feature (and `<Profiler>` API) that measures how often components render and how long each render takes — helping you identify **performance bottlenecks**.

```jsx
<Profiler id="UserList" onRender={(id, phase, actualDuration) => {
  console.log(`${id} took ${actualDuration}ms`);
}}>
  <UserList />
</Profiler>
```

**Use in:** React DevTools → Profiler tab → record interactions → see which components re-render unnecessarily.

---

**Q101. What is the difference between Fetch and Axios for API calls?**

| | Fetch | Axios |
|---|---|---|
| **Built-in** | Yes (browser native) | No (install needed) |
| **JSON auto-parse** | No (need `.json()`) | Yes (automatic) |
| **Error handling** | Non-2xx doesn't throw | Throws on non-2xx |
| **Request cancellation** | Via AbortController | Built-in `cancelToken` |
| **Interceptors** | No | Yes (request/response) |
| **Browser support** | Modern browsers | All (via polyfill) |

**Use case:** For simple apps, Fetch is fine. For production apps with interceptors and global error handling, Axios is more convenient.

---

**Q102. What are the popular Testing Libraries for React?**

1. **Jest** — Test runner + assertion library (default with CRA).
2. **React Testing Library (RTL)** — Tests components from user's perspective (DOM-based).
3. **Vitest** — Faster alternative to Jest, works well with Vite.
4. **Cypress** — End-to-end testing in a real browser.
5. **Playwright** — E2E testing across multiple browsers.

**Most common combo:** Jest + React Testing Library for unit/integration tests.

---

**Q103. How can you optimize performance in a React application?**

1. **React.memo** — Prevent re-renders of functional components when props haven't changed.
2. **useMemo** — Memoize expensive computed values.
3. **useCallback** — Memoize functions to prevent child re-renders.
4. **Code splitting** — Lazy load routes and heavy components.
5. **Virtualization** — Render only visible list items (`react-window`, `react-virtual`).
6. **Avoid anonymous functions in JSX** — creates new references every render.
7. **Keys in lists** — use stable, unique IDs (not array index).

---

**Q104. Explain Reactive Programming with an example.**

**Reactive programming** is a paradigm focused on **data streams and propagation of changes** — when data changes, all dependent UI updates automatically.

**Example:** A search box that filters a list in real time.
```jsx
const [query, setQuery] = useState('');
const filtered = users.filter(u => u.name.includes(query));

<input value={query} onChange={e => setQuery(e.target.value)} />
<UserList users={filtered} />
```

When `query` changes → `filtered` recomputes → `UserList` re-renders. That's reactive — the UI **reacts** to state changes automatically.

---

**Q105. In how many ways can we implement Reactive Programming in React?**

1. **useState + derived values** — most common pattern above.
2. **useEffect** — react to state/prop changes with side effects.
3. **Context API** — reactive global state propagation.
4. **Redux** — reactive store subscriptions.
5. **RxJS** — explicit reactive streams (observables) integrated via hooks.
6. **React Query / SWR** — reactive server state (auto-refetch, caching).
7. **MobX** — observable state that automatically tracks dependencies.

---

**Q106. How to pass data from child component to parent component in React?**

Pass a **callback function** from parent to child as a prop. The child calls it with the data.

```jsx
// Parent — defines callback and passes it
const Parent = () => {
  const handleData = (data) => console.log('From child:', data);
  return <Child onSend={handleData} />;
};

// Child — calls the callback with data
const Child = ({ onSend }) => (
  <button onClick={() => onSend('Hello Parent!')}>Send</button>
);
```

**Use case:** A form component (child) sends form data to a parent on submit. The parent decides what to do with it.

---

*End of React Interview Q&A*

## Advanced — useRef, Large Lists, useMemo vs useCallback

---

**Q1. How does `useRef` differ from `useState`?**

| Feature | `useRef` | `useState` |
|---|---|---|
| Triggers re-render | **No** | **Yes** |
| Persists across renders | Yes | Yes |
| Mutable | Yes (`ref.current`) | No (use setter) |
| Main use case | Access DOM nodes, store mutable values | UI state that should update the screen |

**`useRef`** holds a mutable `.current` value that persists between renders without causing re-renders.

```jsx
// useRef — accessing a DOM element
const inputRef = useRef(null);
const focusInput = () => inputRef.current.focus();
return <input ref={inputRef} />;

// useRef — storing a mutable value without re-render
const countRef = useRef(0);
countRef.current += 1; // does NOT trigger re-render

// useState — triggers re-render
const [count, setCount] = useState(0);
setCount(count + 1); // triggers re-render
```

**When to use `useRef`:**
- Accessing/focusing DOM elements.
- Storing previous values.
- Storing timer IDs (`setInterval` / `setTimeout`).
- Any mutable value that shouldn't cause a re-render.

---

**Q2. How do you optimize large lists in React?**

Rendering thousands of DOM nodes is slow. Use **virtualization** — only render items currently visible in the viewport.

**Option 1: `react-window` (recommended, lightweight)**

```bash
npm install react-window
```

```jsx
import { FixedSizeList } from 'react-window';

const Row = ({ index, style }) => (
  <div style={style}>Item {index}</div>
);

const BigList = () => (
  <FixedSizeList
    height={600}       // visible area height
    itemCount={10000}  // total items
    itemSize={50}      // height of each row
    width="100%"
  >
    {Row}
  </FixedSizeList>
);
```

**Option 2: `react-virtual` (TanStack)**

```bash
npm install @tanstack/react-virtual
```

**Other optimizations for large lists:**
- `React.memo` — prevent child re-renders when props haven't changed.
- `useCallback` — stable function references passed as props.
- Pagination or infinite scroll — load data in chunks.
- Unique stable `key` props — help React reconcile efficiently.

```jsx
// React.memo to prevent unnecessary re-renders
const ListItem = React.memo(({ item }) => (
  <div>{item.name}</div>
));
```

---

**Q3. What is the difference between `useMemo` and `useCallback`, and when should you NOT use them?**

| Hook | Memoizes | Returns |
|---|---|---|
| `useMemo` | The **result** of a function | A value |
| `useCallback` | The **function itself** | A function |

```jsx
// useMemo — memoize an expensive computed value
const sortedList = useMemo(() => {
  return items.sort((a, b) => a.price - b.price);
}, [items]); // only recomputes when `items` changes

// useCallback — memoize a function reference
const handleDelete = useCallback((id) => {
  setItems(prev => prev.filter(item => item.id !== id));
}, []); // stable reference across renders
```

**When to USE them:**
- `useMemo`: Expensive calculations (sorting/filtering large arrays, complex math).
- `useCallback`: Passing callbacks to memoized children (`React.memo`) or as dependency in `useEffect`.

**When NOT to use them (premature optimization):**
- Simple values or functions that are cheap to recreate.
- Components that don't re-render often anyway.
- When the memoization overhead exceeds the performance gain.

```jsx
// BAD — useMemo for something trivially cheap
const double = useMemo(() => count * 2, [count]); // overkill, just write: count * 2

// GOOD — useMemo for genuinely expensive work
const filtered = useMemo(() =>
  largeDataset.filter(item => item.active && item.category === cat),
  [largeDataset, cat]
);
```

**Rule of thumb:** Profile first with React DevTools, then optimize. Don't add `useMemo`/`useCallback` everywhere by default — they add complexity and their own overhead.
