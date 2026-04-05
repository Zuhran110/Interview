# React Interview Preparation — 106 Questions & Answers

---

## React Basics — Part 1

---

**Q1. What is React? What is the role of React in software development?**

React is an open-source JavaScript **library** built by Facebook (Meta) for building **user interfaces**, specifically for single-page applications.

**Role:** React handles the **View layer** of an application (the "V" in MVC). It lets you build reusable UI components and efficiently update the DOM when data changes.

**Use case:** You're building a dashboard where data updates in real time (e.g., stock prices). React re-renders only the changed parts, not the entire page.

---

**Q2. What are the key features of React?**

| Feature | What it means |
|---|---|
| **Virtual DOM** | Fast UI updates without touching real DOM directly |
| **Component-Based** | UI split into reusable, independent pieces |
| **JSX** | Write HTML-like syntax inside JavaScript |
| **One-way Data Flow** | Data flows parent → child via props |
| **Hooks** | Use state and lifecycle in functional components |
| **React Ecosystem** | Works with React Router, Redux, Next.js, etc. |

---

**Q3. What is the DOM? What is the difference between HTML and DOM?**

- **HTML** is the static markup code you write in a `.html` file — it's just text.
- **DOM (Document Object Model)** is the live, in-memory tree representation of that HTML that the browser creates. JavaScript can read and manipulate the DOM.

**Analogy:** HTML is the blueprint; DOM is the actual house built from it.

---

**Q4. What is the Virtual DOM? Difference between DOM and Virtual DOM?**

The **Virtual DOM** is a lightweight, in-memory copy of the real DOM maintained by React.

**How it works:**
1. State changes → React creates a new Virtual DOM tree.
2. React **diffs** (compares) the new and old Virtual DOM.
3. Only the changed nodes are updated in the real DOM (**reconciliation**).

| Real DOM | Virtual DOM |
|---|---|
| Slow to update | Fast (in-memory JS object) |
| Full re-render on changes | Only updates changed parts |
| Browser manages it | React manages it |

**Use case:** Clicking a "Like" button — only the like counter re-renders, not the whole page.

---

**Q5. What are React Components? What are the main elements of it?**

A **component** is a self-contained, reusable piece of UI — like a Lego block.

**Main elements:**
- **JSX** — defines the UI structure
- **Props** — input data passed from parent
- **State** — internal data that can change
- **Event handlers** — functions that respond to user actions
- **Lifecycle/Hooks** — control behavior at different stages

**Use case:** A `Button` component can be reused across the entire app with different labels and colors passed as props.

---

**Q6. What is SPA (Single Page Application)?**

An SPA loads a **single HTML page** and dynamically updates content as the user interacts — no full page reloads.

**Use cases:** Gmail, Facebook, Twitter, Google Maps.

**How React enables it:** React Router swaps components in/out without refreshing the browser, giving a smooth app-like feel.

---

**Q7. What are the 5 advantages of React?**

1. **Fast rendering** — Virtual DOM minimizes real DOM updates.
2. **Reusable components** — Build once, use everywhere.
3. **Large ecosystem** — Huge community, libraries, and tools.
4. **SEO-friendly** — With Next.js (Server Side Rendering).
5. **One-way data flow** — Easier to debug and trace data changes.

---

**Q8. What are the disadvantages of React?**

1. **Only the View layer** — You need extra libraries for routing, state management, etc.
2. **JSX learning curve** — Mixing HTML and JS feels unfamiliar at first.
3. **Frequent updates** — The ecosystem changes fast; keeping up can be hard.
4. **Boilerplate** — Setting up a large app requires many decisions (Redux, Router, etc.).

---

**Q9. What is the role of JSX in React?**

**JSX (JavaScript XML)** lets you write HTML-like code inside JavaScript. It makes the UI code more readable and intuitive.

```jsx
// JSX
const element = <h1>Hello, World!</h1>;

// What Babel converts it to
const element = React.createElement('h1', null, 'Hello, World!');
```

Without JSX, you'd write `React.createElement()` calls everywhere — JSX is just syntactic sugar for that.

---

**Q10. What is the difference between Declarative and Imperative syntax?**

| Declarative (React) | Imperative (Vanilla JS) |
|---|---|
| You describe **what** the UI should look like | You describe **how** to change the UI step by step |
| `<button disabled={isLoading}>Submit</button>` | `if(isLoading) btn.setAttribute('disabled', true)` |
| React figures out the DOM updates | You manually update the DOM |

**Interview answer:** React is declarative — you define the desired state of the UI, and React handles the DOM manipulation for you.

---

## React Basics — Part 2

---

**Q11. What is Arrow Function Expression in JSX?**

Arrow functions are used in JSX to define **inline event handlers** and **component functions** concisely, and they **don't have their own `this`**, which prevents binding issues.

```jsx
// Arrow function as event handler
<button onClick={() => console.log('clicked')}>Click me</button>

// Functional component with arrow function
const Greeting = () => <h1>Hello!</h1>;
```

---

**Q12. How to set up a React first project?**

```bash
npx create-react-app my-app
cd my-app
npm start
```

Or with Vite (modern, faster):
```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

---

**Q13. What are the main files in a React project?**

| File/Folder | Role |
|---|---|
| `public/index.html` | The single HTML shell |
| `src/index.js` | Entry point — mounts React to the DOM |
| `src/App.js` | Root component |
| `package.json` | Project dependencies and scripts |
| `node_modules/` | Installed packages |

---

**Q14. How does a React app load and display components in the browser?**

1. Browser loads `public/index.html` — contains `<div id="root">`.
2. `src/index.js` runs — calls `ReactDOM.render(<App />, document.getElementById('root'))`.
3. React renders the `App` component and all its children into that `#root` div.
4. Webpack/Babel bundle and transpile the code for the browser.

---

**Q15. What is the difference between React and Angular?**

| React | Angular |
|---|---|
| Library (View only) | Full framework (MVC) |
| JavaScript + JSX | TypeScript by default |
| Virtual DOM | Real DOM with Change Detection |
| Flexible, choose your tools | Opinionated, built-in everything |
| Lighter, faster to start | More structure out of the box |
| Facebook | Google |

---

**Q17. Is React a Framework or Library? What is the difference?**

**React is a Library**, not a framework.

| Library | Framework |
|---|---|
| You call the library | Framework calls your code |
| Handles one concern (UI/View) | Handles the whole application |
| React | Angular, Ember |

**Analogy:** A library is like buying individual tools; a framework is like buying a complete toolbox with instructions on how to use them together.

---

**Q18. How does React provide Reusability and Composition?**

- **Reusability:** A component like `<Button>` is written once and used in 100 places with different props.
- **Composition:** Build complex UIs by composing small components together.

```jsx
// Composition example
const Card = ({ title, children }) => (
  <div className="card">
    <h2>{title}</h2>
    {children}
  </div>
);

// Reuse with different content
<Card title="Profile"><Avatar /></Card>
<Card title="Stats"><Chart /></Card>
```

---

**Q19. What are State, Stateless, Stateful, and State Management?**

- **State:** Data that can change over time and causes the component to re-render.
- **Stateful component:** Has its own state (e.g., a counter, form input).
- **Stateless component:** Just receives props and renders UI — no internal state (e.g., a pure display card).
- **State management:** How you manage and share state across components — locally with `useState`, or globally with Context API / Redux.

---

**Q20. What are Props in JSX?**

**Props (Properties)** are how you pass data from a **parent** component to a **child** component. They are **read-only** — a child cannot modify its props.

```jsx
// Parent passes props
<UserCard name="Alice" age={30} />

// Child receives props
const UserCard = ({ name, age }) => (
  <p>{name} is {age} years old</p>
);
```

---

## React Basics — Part 3

---

**Q21. What is NPM? What is the role of the node_modules folder?**

- **NPM (Node Package Manager):** A tool to install, manage, and share JavaScript packages/libraries.
- **node_modules:** The folder where all installed packages live. It is auto-generated by `npm install` and should never be committed to git (add to `.gitignore`).

---

**Q22. What is the role of the public folder in React?**

The `public/` folder contains **static assets** that are served as-is, without being processed by Webpack:
- `index.html` — the app shell
- Favicon, images, manifest.json

Files here are accessible via `/` in the browser. The `%PUBLIC_URL%` variable in `index.html` references this folder.

---

**Q23. What is the role of the src folder in React?**

`src/` contains all your **application source code** — components, hooks, styles, and utilities. Everything here gets processed and bundled by Webpack/Babel.

---

**Q24. What is the role of index.html in React?**

It's the **single HTML page** that the browser loads. It has one key element:

```html
<div id="root"></div>
```

React mounts the entire application inside this `#root` div. Everything else is rendered dynamically by JavaScript.

---

**Q25. What is the role of index.js and ReactDOM in React?**

`index.js` is the **entry point** of the app. It uses `ReactDOM` to mount the root component into the HTML.

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

**ReactDOM** is the bridge between React's Virtual DOM and the browser's real DOM.

---

**Q26. What is the role of App.js in React?**

`App.js` is the **root component** — the top-level component that contains all other components. Think of it as the main container of your UI tree.

---

**Q27. What is the role of function and return inside App.js?**

- The **function** defines the component and contains all the logic (state, handlers, etc.).
- The **return** statement outputs the JSX — what actually gets rendered to the screen.

```jsx
function App() {
  const title = "Hello"; // logic
  return <h1>{title}</h1>; // UI output
}
```

---

**Q28. Can we have a function without a return inside App.js?**

Yes — if the component renders nothing, it can return `null`. But a component **must always have a return statement** (even if it returns `null`). A function with no return implicitly returns `undefined`, which React will throw an error for.

```jsx
function App() {
  return null; // valid — renders nothing
}
```

---

**Q29. What is the role of export default inside App.js?**

`export default` makes the component available to be **imported** in other files. Without it, `import App from './App'` would fail.

```jsx
export default App; // allows: import App from './App'
```

---

**Q30. Does the file name and component name have to be the same in React?**

**No, it's not required** — but it's a **strong convention**. A component named `UserProfile` should live in `UserProfile.js`. This makes the codebase predictable and easier to navigate.

---

## JSX — Part 4

---

**Q31. What is the role of JSX in React?**

JSX allows you to write **HTML-like UI code inside JavaScript**. It makes component code more readable and combines the UI structure with its logic in one place.

```jsx
const App = () => (
  <div>
    <h1>Welcome</h1>
    <p>This is JSX</p>
  </div>
);
```

Babel compiles JSX to `React.createElement()` calls before the browser runs it.

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

## Components — Functional & Class — Part 5

---

**Q41. What are React Components? What are the main elements?**

A component is a **reusable, independent piece of UI**. Main elements:
- **JSX** (UI structure)
- **Props** (input from parent)
- **State** (internal data)
- **Hooks / Lifecycle methods** (behavior control)
- **Event handlers** (user interaction)

---

**Q42. What are the types of React Components? What are Functional Components?**

**Types:**
1. **Functional Components** — JavaScript functions that return JSX. Modern standard.
2. **Class Components** — ES6 classes that extend `React.Component`. Legacy.

**Functional Component:**
```jsx
const Hello = ({ name }) => <h1>Hello, {name}!</h1>;
```
Simple, concise, uses hooks for state and lifecycle.

---

**Q43. How do you pass data between functional components?**

Via **props** from parent to child:
```jsx
// Parent
<UserCard name="Alice" />

// Child
const UserCard = ({ name }) => <p>{name}</p>;
```

For sibling or deeply nested sharing, use **Context API** or **state management** (Redux).

---

**Q44. What is Prop Drilling in React?**

**Prop Drilling** is when you pass props through multiple layers of components just to reach a deeply nested child — even if intermediate components don't need that data.

```
App → Header → Nav → UserAvatar (needs user data)
```
`user` prop gets passed through `App → Header → Nav → UserAvatar` even though only `UserAvatar` needs it.

---

**Q45. Why avoid Prop Drilling? How many ways to avoid it?**

**Why avoid:** It creates unnecessary coupling — intermediate components must know about data they don't use, making refactoring painful.

**Ways to avoid:**
1. **Context API** — share data globally without passing props
2. **Redux / Zustand** — global state management
3. **Component Composition** — pass components as children/props instead of data
4. **Custom Hooks** — encapsulate and share logic
5. **React Query / SWR** — for server state

---

**Q46. What are Class Components in React?**

Class components are ES6 classes that extend `React.Component` and have a `render()` method returning JSX. They were the original way to use state and lifecycle methods.

```jsx
class Counter extends React.Component {
  state = { count: 0 };

  render() {
    return <h1>{this.state.count}</h1>;
  }
}
```

Today, **functional components + hooks** are preferred.

---

**Q47. How to pass data between class components?**

Same as functional — via **props** for parent-to-child, and **callbacks** for child-to-parent.

```jsx
// Parent
<Child message="Hello" onUpdate={this.handleUpdate} />

// Child
this.props.message
this.props.onUpdate(newData)
```

---

**Q48. What is the role of the `this` keyword in class components?**

`this` refers to the **current instance** of the class. You use it to access:
- `this.state` — the component's state
- `this.props` — the component's props
- `this.setState()` — update state
- `this.handleClick` — reference class methods

Arrow functions in class fields avoid binding issues since they capture `this` lexically.

---

**Q49. What are the 5 differences between Functional and Class Components?**

| | Functional | Class |
|---|---|---|
| **Syntax** | Plain function | ES6 class |
| **State** | `useState()` hook | `this.state` |
| **Lifecycle** | `useEffect()` hook | `componentDidMount()`, etc. |
| **`this` keyword** | Not needed | Required everywhere |
| **Performance** | Slightly better | Slightly more overhead |

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

**Q54. What is the role of the Switch component in React Routing?**

**`<Switch>`** (React Router v5) renders **only the first matching route**, preventing multiple routes from rendering simultaneously.

In **React Router v6**, `<Switch>` was replaced by `<Routes>`, which does the same thing automatically.

---

**Q55. What is the role of the `exact` prop in React Routing?**

In **React Router v5**, `exact` ensures the route only matches if the path is an **exact** match.

```jsx
// Without exact: '/' matches '/', '/about', '/users', etc.
<Route exact path="/" component={Home} />
// With exact: '/' only matches '/'
```

In **React Router v6**, this is no longer needed — all routes match exactly by default.

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

**Q57. What are State, Stateless, Stateful, and State Management?**

- **State:** Internal data of a component that can change and triggers re-renders.
- **Stateful:** Component that owns and manages state (e.g., a form).
- **Stateless:** Component that only receives props and renders UI (e.g., a pure card).
- **State Management:** Strategy for managing state — local (`useState`), global (Context, Redux), or server state (React Query).

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

## Component Lifecycle — Part 11

---

**Q70. What are component lifecycle phases?**

Every React component goes through 3 phases:

1. **Mounting** — Component is created and inserted into the DOM.
2. **Updating** — Component re-renders due to state or prop changes.
3. **Unmounting** — Component is removed from the DOM.

---

**Q71. What are component lifecycle methods?**

| Phase | Method | Hook Equivalent |
|---|---|---|
| Mounting | `constructor()` | `useState` init |
| Mounting | `render()` | return JSX |
| Mounting | `componentDidMount()` | `useEffect(() => {}, [])` |
| Updating | `componentDidUpdate()` | `useEffect(() => {}, [dep])` |
| Unmounting | `componentWillUnmount()` | `useEffect` cleanup return |

---

**Q73. What are constructors in class components? When to use them?**

The `constructor` runs **first** when a class component is created. Use it to:
1. Initialize `this.state`
2. Bind event handler methods to `this`

```jsx
constructor(props) {
  super(props);
  this.state = { count: 0 };
  this.handleClick = this.handleClick.bind(this);
}
```

**Note:** With modern class fields syntax, constructors are rarely needed.

---

**Q74. What is the role of the super keyword in constructor?**

`super(props)` calls the **parent class constructor** (`React.Component`). It **must** be called before you can use `this` in the constructor. Without it, `this.props` would be undefined.

```jsx
constructor(props) {
  super(props); // must call this first
  this.state = { name: '' };
}
```

---

**Q75. What is the role of render() method in component lifecycle?**

`render()` is the **only required method** in a class component. It reads `this.state` and `this.props` and returns JSX to display. It must be a **pure function** — no side effects, no `setState` calls.

It runs on:
- Initial mount
- Every state/prop update

---

**Q76. How can state be maintained in a class component?**

State is initialized in the constructor and updated with `setState()`.

```jsx
class Counter extends React.Component {
  state = { count: 0 }; // initialize

  increment = () => {
    this.setState({ count: this.state.count + 1 }); // update
  };

  render() {
    return <button onClick={this.increment}>{this.state.count}</button>;
  }
}
```

**Important:** Never mutate state directly (`this.state.count = 1`). Always use `setState`.

---

**Q77. What is the role of componentDidMount()?**

`componentDidMount()` runs **once** after the component is first rendered to the DOM. It's the ideal place to:
- Fetch data from APIs
- Set up subscriptions or event listeners
- Initialize third-party libraries

```jsx
componentDidMount() {
  fetch('/api/data').then(res => res.json()).then(data => this.setState({ data }));
}
```

**Hook equivalent:** `useEffect(() => { ... }, [])`

---

## Controlled & Uncontrolled Components — Part 13

---

**Q78. What are Controlled Components in React?**

A **controlled component** is one where React **controls the form element's value** through state. The input value is always driven by `state`, and every change goes through `setState`.

```jsx
const [name, setName] = useState('');

<input
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

**Single source of truth** — React state is always in sync with the input.

---

**Q79. What are the differences between Controlled and Uncontrolled Components?**

| Controlled | Uncontrolled |
|---|---|
| Value managed by React state | Value managed by the DOM |
| Use `value` + `onChange` | Use `ref` to read value |
| Instant access to input value | Read value only on submit |
| More code, more control | Less code, simpler for basic forms |
| Good for validation | Good for file inputs, simple forms |

---

**Q80. What are the characteristics of Controlled Components?**

1. Input value is **tied to state** via `value` prop.
2. Changes are handled via **onChange** event.
3. React is the **single source of truth**.
4. Easy to perform **real-time validation**.
5. Easy to **reset or pre-fill** form data.

---

**Q81. What are the advantages of Controlled Components in React forms?**

1. **Instant validation** — validate on every keystroke.
2. **Conditional disabling** — disable submit until form is valid.
3. **Programmatic control** — easily reset or pre-populate forms.
4. **Consistency** — state always matches the displayed value.
5. **Easy testing** — just test state changes.

---

**Q82. How to handle forms in React?**

```jsx
const [formData, setFormData] = useState({ email: '', password: '' });

const handleSubmit = (e) => {
  e.preventDefault(); // prevent page reload
  console.log(formData);
};

<form onSubmit={handleSubmit}>
  <input value={formData.email} onChange={e => setFormData({...formData, email: e.target.value})} />
  <button type="submit">Submit</button>
</form>
```

---

**Q83. How can you handle multiple input fields in a controlled form?**

Use a single state object and the input's `name` attribute to dynamically update the right field:

```jsx
const [form, setForm] = useState({ name: '', email: '' });

const handleChange = (e) => {
  setForm({ ...form, [e.target.name]: e.target.value });
};

<input name="name" value={form.name} onChange={handleChange} />
<input name="email" value={form.email} onChange={handleChange} />
```

---

**Q84. How do you handle form validation in a controlled component?**

```jsx
const [email, setEmail] = useState('');
const [error, setError] = useState('');

const validate = (value) => {
  if (!value.includes('@')) setError('Invalid email');
  else setError('');
};

<input
  value={email}
  onChange={(e) => { setEmail(e.target.value); validate(e.target.value); }}
/>
{error && <span style={{ color: 'red' }}>{error}</span>}
```

For complex forms, use libraries like **React Hook Form** or **Formik**.

---

**Q85. When might using Uncontrolled Components be advantageous?**

1. **File inputs** — `<input type="file">` is always uncontrolled; you access it via ref.
2. **Integrating with non-React code** — third-party DOM libraries.
3. **Simple forms with no validation** — just need the value on submit.
4. **Performance** — avoids re-renders on every keystroke.

```jsx
const inputRef = useRef();
const handleSubmit = () => console.log(inputRef.current.value);

<input ref={inputRef} />
```

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

**Q92. Can you dynamically load CSS files using Code Splitting in React?**

**Yes.** When Webpack creates a chunk for a lazy component, it also bundles any CSS imported within that component into a **separate CSS chunk** that loads alongside the JS chunk.

You can also use:
- CSS Modules (scoped per component)
- Libraries like `loadable-components` for more control
- Dynamic `import()` for CSS-in-JS solutions

---

**Q93. How do you inspect and analyze generated chunks in a React application?**

1. **Webpack Bundle Analyzer:**
```bash
npm install --save-dev webpack-bundle-analyzer
```
Generates an interactive treemap of all chunks and their sizes.

2. **Source Map Explorer:**
```bash
npm run build && npx source-map-explorer 'build/static/js/*.js'
```

3. **Browser DevTools** → Network tab → filter by JS to see chunk loading.

4. **Vite's built-in `rollup-plugin-visualizer`** for Vite projects.

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

**Q97. What is GraphQL?**

**GraphQL** is a query language for APIs developed by Facebook. Unlike REST (which has fixed endpoints), GraphQL lets clients **request exactly the data they need** in a single request.

```graphql
# REST: GET /users/1 returns everything
# GraphQL: ask for only what you need
query {
  user(id: 1) {
    name
    email
  }
}
```

**In React:** Use with Apollo Client or React Query to fetch, cache, and update GraphQL data.

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

*End of React Interview Q&A — 106 Questions*
