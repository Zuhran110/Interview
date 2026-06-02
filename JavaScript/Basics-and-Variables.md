# JavaScript — Basics, Variables & Datatypes

---

## Basics

---

**Q1: What is DOM? What is the difference between HTML and DOM?**

- **HTML** is the static markup you write in a `.html` file — it's just text/code.
- **DOM (Document Object Model)** is the live, in-memory tree structure the browser builds from that HTML. JavaScript reads and manipulates the DOM.

```html
<!-- HTML (static text) -->
<p id="msg">Hello</p>
```
```js
// DOM (live object)
document.getElementById('msg').textContent = 'World'; // modifies the live DOM
```

**Analogy:** HTML is the blueprint; DOM is the actual building constructed from it.

---

**Q2: What is JavaScript? What is the role of the JS engine?**

**JavaScript** is a high-level, interpreted, single-threaded programming language primarily used to add interactivity to web pages.

**JS Engine:** A program that parses, compiles (JIT), and executes JavaScript code.
- **V8** — Chrome, Node.js
- **SpiderMonkey** — Firefox
- **JavaScriptCore** — Safari

The engine:
1. Parses JS code into an AST (Abstract Syntax Tree).
2. Compiles it to machine code (JIT compilation).
3. Executes it.

---

**Q3: What are Client-side and Server-side?**

| Client-side | Server-side |
|---|---|
| Runs in the **browser** | Runs on the **server** |
| JavaScript, HTML, CSS | Node.js, Python, PHP, Java |
| Handles UI, user events | Handles DB, auth, business logic |
| Code is visible to the user | Code is hidden |

---

**Q4: What is Scope in JS?**

Scope defines **where a variable is accessible** in code.

- **Global scope** — accessible everywhere.
- **Function scope** — `var` declared inside a function; only accessible inside it.
- **Block scope** — `let`/`const` declared inside `{}` (if, for, etc.); only accessible inside the block.
- **Lexical (Closure) scope** — inner functions access outer function's variables.

```js
let x = 'global';

function outer() {
  let y = 'function scope';
  if (true) {
    let z = 'block scope';
    console.log(x, y, z); // all accessible
  }
  // console.log(z); // ReferenceError — z not accessible here
}
```

---

**Q5: What is the type of a variable declared without `var`, `let`, or `const`?**

It becomes an **implicit global variable** — attached to the `window` object (browser) or `global` (Node.js). This is a bug source and is forbidden in strict mode.

```js
function test() {
  x = 42; // no keyword — becomes global!
}
test();
console.log(x); // 42 — bad practice

'use strict';
function test2() {
  y = 42; // ReferenceError in strict mode
}
```

---

**Q6: What is Hoisting in JavaScript?**

Hoisting is JavaScript's behavior of **moving declarations to the top of their scope** before code executes.

- **`var`** — hoisted and initialized as `undefined`.
- **Function declarations** — fully hoisted (can be called before declaration).
- **`let`/`const`** — hoisted but NOT initialized (Temporal Dead Zone → ReferenceError).

```js
console.log(a);  // undefined (hoisted)
var a = 5;

greet();         // works — function hoisted
function greet() { console.log('Hi'); }

console.log(b);  // ReferenceError
let b = 10;
```

---

**Q7: What is JSON?**

**JSON (JavaScript Object Notation)** is a lightweight, text-based data format for storing and exchanging data. It is language-independent but derived from JS object syntax.

```json
{
  "name": "Alice",
  "age": 30,
  "skills": ["JavaScript", "React"],
  "address": { "city": "London" }
}
```

```js
// Convert JS object → JSON string
const json = JSON.stringify({ name: 'Alice', age: 30 });

// Convert JSON string → JS object
const obj = JSON.parse('{"name":"Alice","age":30}');
```

---

## Variables and Datatypes

---

**Q8: What are variables? What is the difference between `var`, `let`, and `const`?**

A variable is a **named container for storing data values**.

| Feature | `var` | `let` | `const` |
|---|---|---|---|
| Scope | Function | Block | Block |
| Hoisting | Yes (undefined) | Yes (TDZ) | Yes (TDZ) |
| Re-declare | Yes | No | No |
| Re-assign | Yes | Yes | No |

```js
var x = 1;    // function-scoped, can redeclare
let y = 2;    // block-scoped, can reassign
const z = 3;  // block-scoped, cannot reassign

// const with objects/arrays — reference is fixed, content is mutable
const arr = [1, 2, 3];
arr.push(4);  // OK — mutating content
arr = [];     // TypeError — can't reassign the reference
```

---

**Q9: What are data types in JS?**

**Primitive (7 types):** `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`

**Non-Primitive (Reference types):** `object`, `array`, `function`

```js
typeof "hello"       // "string"
typeof 42            // "number"
typeof true          // "boolean"
typeof undefined     // "undefined"
typeof null          // "object" (historical bug)
typeof Symbol()      // "symbol"
typeof 9n            // "bigint"
typeof {}            // "object"
typeof []            // "object"
typeof function(){}  // "function"
```

---

**Q10: What is the difference between primitive and non-primitive data types?**

| Primitive | Non-Primitive |
|---|---|
| Stored by **value** | Stored by **reference** |
| Immutable | Mutable |
| Copied on assignment | Reference copied on assignment |
| `string, number, boolean, null, undefined, symbol, bigint` | `object, array, function` |

```js
// Primitive — copy by value
let a = 5;
let b = a;
b = 10;
console.log(a); // 5 — unchanged

// Non-primitive — copy by reference
let obj1 = { name: 'Alice' };
let obj2 = obj1;
obj2.name = 'Bob';
console.log(obj1.name); // 'Bob' — both point to same object
```

---

**Q11: What is the difference between `null` and `undefined` in JS?**

| `null` | `undefined` |
|---|---|
| Intentional absence of value | Variable declared but not assigned |
| Set explicitly by developer | Default value by JavaScript |
| `typeof null === 'object'` (bug) | `typeof undefined === 'undefined'` |

```js
let a;
console.log(a); // undefined — declared, not assigned

let b = null;
console.log(b); // null — explicitly set to "nothing"

null == undefined   // true (loose equality)
null === undefined  // false (strict equality)
```

---

**Q12: What is the use of the `typeof` operator?**

`typeof` returns a **string** describing the type of a value.

```js
typeof 42          // "number"
typeof "hello"     // "string"
typeof true        // "boolean"
typeof undefined   // "undefined"
typeof null        // "object" — known bug
typeof {}          // "object"
typeof []          // "object" — use Array.isArray() for arrays
typeof function(){} // "function"
typeof Symbol()    // "symbol"
```

---

**Q13: What is type coercion in JS?**

Type coercion is JavaScript's **automatic conversion** of one data type to another.

**Implicit coercion (automatic):**
```js
'5' + 3        // '53' — number coerced to string
'5' - 3        // 2 — string coerced to number
true + 1       // 2 — boolean coerced to number
false + 'x'    // 'falsex'
null + 1       // 1
undefined + 1  // NaN
```

**Explicit coercion (manual):**
```js
Number('42')   // 42
String(42)     // '42'
Boolean(0)     // false
parseInt('3px') // 3
```

Use `===` (strict equality) to avoid unexpected coercion bugs.

---
