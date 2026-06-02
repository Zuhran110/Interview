# React — JSX

---

## JSX — Part 4

---

**Q32. What are the 5 advantages of JSX?**

1. **Readable** — UI structure is clear and HTML-like.
2. **Type-safe** — Catches errors at compile time (with TypeScript).
3. **Powerful expressions** — Embed any JS logic with `{}`.
4. **Prevents injection attacks** — React escapes values in JSX by default.
5. **Better tooling** — IDEs provide autocomplete and syntax highlighting.

---

**Q33. What is Babel?**

**Babel** is a JavaScript **transpiler** that converts modern JavaScript (ES6+, JSX) into older JavaScript that browsers can understand.

**Use case:** You write `const arrow = () => {}` and JSX — Babel converts it to ES5 `function` syntax and `React.createElement()` calls.

---

**Q34. What is the role of Fragment in JSX?**

A **Fragment** lets you return multiple elements from a component **without adding an extra DOM node**.

```jsx
// Without Fragment — adds an extra <div>
return <div><h1>Title</h1><p>Desc</p></div>;

// With Fragment — no extra DOM node
return (
  <>
    <h1>Title</h1>
    <p>Desc</p>
  </>
);
```

**Use case:** When styling would break if a wrapper `<div>` is added (e.g., inside a `<table>`).

---

**Q35. What is the Spread Operator in JSX?**

The spread operator (`...`) lets you pass all properties of an object as props to a component at once.

```jsx
const buttonProps = { type: 'submit', disabled: false, className: 'btn' };

// Without spread
<button type={buttonProps.type} disabled={buttonProps.disabled}>Submit</button>

// With spread
<button {...buttonProps}>Submit</button>
```

---

**Q36. What are the types of Conditional Rendering in JSX?**

1. **Ternary operator** — `{isLoggedIn ? <Dashboard /> : <Login />}`
2. **Logical AND (&&)** — `{isLoading && <Spinner />}`
3. **if/else** — outside the return statement
4. **Switch statement** — for multiple conditions
5. **Immediately Invoked Function (IIFE)** — `{(() => { ... })()}`

---

**Q37. How do you iterate over a list in JSX? What is the map() method?**

Use the JavaScript `map()` array method to render lists in JSX. Each item must have a unique **key** prop.

```jsx
const fruits = ['Apple', 'Banana', 'Mango'];

return (
  <ul>
    {fruits.map((fruit, index) => (
      <li key={index}>{fruit}</li>
    ))}
  </ul>
);
```

**Key prop:** Helps React identify which items changed, were added, or removed.

---

**Q38. Can a browser read a JSX file?**

**No.** Browsers only understand plain HTML, CSS, and JavaScript. JSX must be **transpiled** by Babel into standard `React.createElement()` JavaScript calls before the browser can run it.

---

**Q39. What is a Transpiler? What is the difference between Compiler and Transpiler?**

| Compiler | Transpiler |
|---|---|
| Converts code to a lower-level language (e.g., C → machine code) | Converts code from one high-level language to another |
| Output is fundamentally different | Output is the same level (e.g., JSX → JS, ES6 → ES5) |

**Babel is a transpiler** — it converts JSX/ES6 → ES5 JavaScript.

---

**Q40. Is it possible to use JSX without React?**

**Yes**, since React 17. React introduced a new JSX transform where you no longer need `import React from 'react'` at the top — the transform auto-imports what's needed. However, you still need a tool like Babel to process JSX.

---
