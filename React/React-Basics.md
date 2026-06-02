# React — Basics

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
