# React — Component Lifecycle

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
