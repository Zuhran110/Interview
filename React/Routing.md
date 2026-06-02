# React — Routing

---

## Routing — Part 6

---

**Q50. What is Routing and Router in React?**

**Routing** is the mechanism of navigating between different views/pages in a React app **without a full page reload**.

**React Router** is the standard library for routing (`react-router-dom`). It maps URL paths to components.

```
/ → <Home />
/about → <About />
/users/:id → <UserProfile />
```

---

**Q51. How to implement Routing in React?**

```bash
npm install react-router-dom
```

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

---

**Q52. What are the roles of `<Routes>` and `<Route>` components?**

- **`<Routes>`** — Container for all routes; renders the **first matching** `<Route>`.
- **`<Route>`** — Defines a mapping between a URL `path` and a component (`element`).

```jsx
<Routes>
  <Route path="/home" element={<Home />} />
  <Route path="/about" element={<About />} />
</Routes>
```

---

**Q53. What are Route Parameters in React Routing?**

Route parameters are **dynamic segments** in a URL, defined with `:paramName`. They let you render different content based on the URL.

```jsx
<Route path="/user/:id" element={<UserProfile />} />

// Access in component
import { useParams } from 'react-router-dom';
const { id } = useParams();
```

**Use case:** `/user/42` → renders profile for user with ID 42.

---
