# React Advanced Interview Questions & Answers

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
