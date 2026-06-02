# React — Hooks

---

## Hooks — useState / useEffect — Part 7

---

**Q56. What are React Hooks? What are the top React Hooks?**

**Hooks** are functions that let you use React features (state, lifecycle, context) inside **functional components**. Introduced in React 16.8.

**Top Hooks:**
| Hook | Purpose |
|---|---|
| `useState` | Local state management |
| `useEffect` | Side effects (API calls, timers) |
| `useContext` | Access Context values |
| `useReducer` | Complex state logic |
| `useRef` | DOM references, persist values |
| `useMemo` | Memoize computed values |
| `useCallback` | Memoize functions |
| `useNavigate` | Programmatic routing |

---

**Q58. What is the role of useState() and how does it work?**

`useState` lets functional components hold **local state**.

```jsx
const [count, setCount] = useState(0);
//     state  setter    initial value
```

**How it works:**
1. `useState(0)` initializes `count` to `0`.
2. When you call `setCount(newValue)`, React updates the state and **re-renders** the component.
3. React preserves state between renders.

```jsx
<button onClick={() => setCount(count + 1)}>+1</button>
```

---

**Q59. What is the role of useEffect() and how does it work?**

`useEffect` lets you run **side effects** after a component renders — things like:
- Fetching data from an API
- Setting up event listeners
- Updating the document title
- Timers / subscriptions

```jsx
useEffect(() => {
  fetch('/api/users').then(res => res.json()).then(setUsers);
}, []); // runs once after initial render
```

**Structure:** `useEffect(callback, dependencyArray)`

---

**Q60. What is the Dependency Array in useEffect()?**

The **dependency array** controls **when** the effect re-runs:

| Dependency Array | When it runs |
|---|---|
| Not provided | After **every** render |
| `[]` (empty) | Only **once** after initial mount |
| `[value]` | After mount + whenever `value` changes |

```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]); // re-runs only when count changes
```

---

**Q61. What is the meaning of the empty array `[]` in useEffect()?**

An empty dependency array `[]` means: **"run this effect only once"** — after the component mounts for the first time. It's equivalent to `componentDidMount` in class components.

**Use case:** Fetching initial data when a page loads.

```jsx
useEffect(() => {
  fetchInitialData();
}, []); // runs once on mount
```

---

## Hooks — useContext / useReducer — Part 8

---

**Q62. What is the role of the useContext() hook?**

`useContext` lets you **consume** a Context value in any functional component **without prop drilling**. It gives direct access to the nearest Context Provider's value.

```jsx
const theme = useContext(ThemeContext);
```

**Use case:** A `ThemeContext` provides dark/light mode to every component in the app without passing it as props.

---

**Q63. What is createContext()? What are Provider and Consumer properties?**

```jsx
const ThemeContext = createContext('light'); // default value
```

- **`createContext()`** — creates the Context object.
- **`Provider`** — wraps the component tree and **supplies** the value.
- **`Consumer`** — older API to **receive** the value (replaced by `useContext`).

```jsx
// Provider (supply value)
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>

// Consumer (receive value)
const theme = useContext(ThemeContext); // "dark"
```

---

**Q64. When to use useContext instead of props in a real application?**

Use `useContext` when:
- Data needs to be **shared across many components** at different nesting levels.
- Passing via props would cause **prop drilling** (3+ levels deep).

**Real-world use cases:**
- Auth state (current user)
- Theme (dark/light mode)
- Language/locale settings
- Shopping cart data

---

**Q65. What are the similarities between useState() and useReducer()?**

1. Both manage **local component state**.
2. Both trigger a **re-render** when state changes.
3. Both return the **current state** value.
4. Both are React Hooks — only usable in functional components.
5. Initial state is passed as an **argument**.

---

**Q66. What is useReducer()? When to use useState vs useReducer?**

`useReducer` is a hook for managing **complex state logic** — similar to how Redux works, but local.

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

| Use `useState` when | Use `useReducer` when |
|---|---|
| Simple, independent values | Multiple related state values |
| `count`, `isOpen`, `name` | Shopping cart, form with many fields |
| State logic is straightforward | State transitions follow clear rules |

---

**Q67. What are the differences between useState() and useReducer()?**

| | useState | useReducer |
|---|---|---|
| **Complexity** | Simple state | Complex state logic |
| **Update method** | `setState(newValue)` | `dispatch({ type: 'ACTION' })` |
| **Logic location** | Inline in component | Centralized in `reducer` function |
| **Predictability** | Lower | Higher (pure reducer function) |
| **Best for** | Single values | Multiple related values |

---

**Q68. What are dispatch and reducer functions in useReducer?**

- **`reducer(state, action)`** — a **pure function** that takes current state + an action, and returns the **new state**. Contains all the state transition logic.
- **`dispatch(action)`** — triggers the reducer. You send an **action object** (usually with `type` and optionally `payload`).

```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT': return { count: state.count + 1 };
    case 'RESET': return { count: 0 };
    default: return state;
  }
};

dispatch({ type: 'INCREMENT' });
```

---

**Q69. What is the purpose of passing initial state as an object in useReducer?**

Passing an **object** as initial state lets you group multiple related values together and update them atomically — making it easy to manage complex forms or UI states.

```jsx
const initialState = { name: '', email: '', isSubmitting: false };
const [state, dispatch] = useReducer(reducer, initialState);
```

This keeps all related state in one place and makes state transitions more predictable.

---
