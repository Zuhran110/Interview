# React — Components (Functional & Class)

---

## Components — Functional & Class — Part 5

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
