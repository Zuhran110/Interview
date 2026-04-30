# JavaScript Interview Questions & Answers

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

## Operators & Conditions

---

**Q14: What is operator precedence?**

Operator precedence determines the **order in which operators are evaluated** in an expression. Higher precedence operators execute first.

```js
2 + 3 * 4    // 14 — multiplication before addition
(2 + 3) * 4  // 20 — parentheses override precedence

// Precedence (high → low, simplified):
// () > ** > * / % > + - > < > <= >= > == != === !== > && > || > =
```

---

**Q15: What is the difference between unary, binary, and ternary operators?**

| Type | Operands | Example |
|---|---|---|
| **Unary** | 1 | `typeof x`, `-x`, `!true`, `++i` |
| **Binary** | 2 | `a + b`, `x === y`, `a && b` |
| **Ternary** | 3 | `condition ? valueIfTrue : valueIfFalse` |

```js
// Unary
console.log(typeof 42); // "number"
console.log(!true);     // false

// Binary
console.log(5 + 3);     // 8

// Ternary
const age = 20;
const status = age >= 18 ? 'adult' : 'minor'; // 'adult'
```

---

**Q16: What is short-circuit evaluation in JS?**

Short-circuit evaluation means JavaScript **stops evaluating** as soon as the result is determined.

```js
// && (AND) — stops at first falsy value
false && doSomething()   // doSomething() never called
true && 'hello'          // 'hello' (returns last truthy)
null && 'hello'          // null (returns first falsy)

// || (OR) — stops at first truthy value
true || doSomething()    // doSomething() never called
false || 'default'       // 'default' (returns first truthy)
null || 'default'        // 'default'

// Practical use: default values
const name = user?.name || 'Guest';

// ?? (Nullish coalescing) — only null/undefined, not 0 or ''
const count = value ?? 0;
```

---

**Q17: What are the types of conditional statements in JS?**

```js
// 1. if / else if / else
if (score >= 90) {
  grade = 'A';
} else if (score >= 80) {
  grade = 'B';
} else {
  grade = 'C';
}

// 2. switch — for multiple discrete values
switch (day) {
  case 'Monday': console.log('Start of week'); break;
  case 'Friday': console.log('TGIF'); break;
  default: console.log('Midweek');
}

// 3. Ternary operator
const msg = isLoggedIn ? 'Welcome back' : 'Please log in';

// 4. Nullish coalescing (??)
const username = input ?? 'Anonymous';

// 5. Optional chaining (?.)
const city = user?.address?.city; // undefined if user or address is null
```

---

**Q18: What is the difference between `==` and `===`?**

| `==` (Loose equality) | `===` (Strict equality) |
|---|---|
| Compares values after type coercion | Compares value AND type — no coercion |
| `'5' == 5` → `true` | `'5' === 5` → `false` |

```js
0 == false      // true  (coercion)
0 === false     // false (different types)
null == undefined // true
null === undefined // false
'' == false     // true
'' === false    // false
```

**Always use `===`** unless you specifically need type coercion.

---

**Q19: What is the difference between Spread and Rest operator in JS?**

Both use `...` syntax but serve opposite purposes.

**Spread (`...`)** — **expands** an iterable into individual elements.
```js
const arr = [1, 2, 3];
console.log(...arr);          // 1 2 3
const arr2 = [...arr, 4, 5]; // [1, 2, 3, 4, 5]

const obj = { a: 1 };
const obj2 = { ...obj, b: 2 }; // { a: 1, b: 2 }

Math.max(...arr); // 3
```

**Rest (`...`)** — **collects** remaining arguments into an array.
```js
function sum(...numbers) { // collects all args into array
  return numbers.reduce((total, n) => total + n, 0);
}
sum(1, 2, 3, 4); // 10

const [first, ...rest] = [1, 2, 3, 4];
// first = 1, rest = [2, 3, 4]
```

---

## Arrays

---

**Q20: What are Arrays in JS? How to get, add & remove elements?**

An array is an ordered, zero-indexed collection of values.

```js
const fruits = ['apple', 'banana', 'cherry'];

// Get
fruits[0];         // 'apple'
fruits.at(-1);     // 'cherry' (last element)

// Add
fruits.push('date');          // add to end → ['apple','banana','cherry','date']
fruits.unshift('avocado');    // add to start

// Remove
fruits.pop();                 // remove from end
fruits.shift();               // remove from start
fruits.splice(1, 1);          // remove 1 element at index 1
```

---

**Q21: What is `indexOf()` method of an Array?**

Returns the **first index** of a specified element, or `-1` if not found.

```js
const arr = ['a', 'b', 'c', 'b'];
arr.indexOf('b');     // 1 (first occurrence)
arr.indexOf('b', 2);  // 3 (search from index 2)
arr.indexOf('z');     // -1 (not found)

// Use case: check if element exists
if (arr.indexOf('b') !== -1) { /* exists */ }
// Prefer includes() for simple existence check:
arr.includes('b');    // true
```

---

**Q22: What is the difference between `find()` and `filter()` method of an array?**

| `find()` | `filter()` |
|---|---|
| Returns the **first** matching element | Returns **all** matching elements |
| Returns the value or `undefined` | Returns a new array (empty if no match) |
| Stops at first match | Iterates the whole array |

```js
const users = [
  { id: 1, name: 'Alice', active: true },
  { id: 2, name: 'Bob', active: false },
  { id: 3, name: 'Carol', active: true },
];

users.find(u => u.active);    // { id: 1, name: 'Alice', active: true }
users.filter(u => u.active);  // [{ id: 1... }, { id: 3... }]
```

---

**Q23: What is the `slice()` method of an Array?**

`slice(start, end)` returns a **shallow copy** of a portion of an array. It does NOT modify the original.

```js
const arr = [1, 2, 3, 4, 5];
arr.slice(1, 3);  // [2, 3] — from index 1 up to (not including) index 3
arr.slice(2);     // [3, 4, 5] — from index 2 to end
arr.slice(-2);    // [4, 5] — last 2 elements
arr.slice();      // [1, 2, 3, 4, 5] — shallow copy of entire array
// original arr is unchanged
```

---

**Q24: What is the difference between `push()` and `concat()` methods?**

| `push()` | `concat()` |
|---|---|
| **Mutates** the original array | Returns a **new** array — doesn't mutate |
| Adds elements to the end | Joins arrays/values into a new one |
| Returns new length | Returns new array |

```js
const arr = [1, 2, 3];
arr.push(4);              // arr is now [1, 2, 3, 4] — mutated
const arr2 = arr.concat([5, 6]); // arr unchanged, arr2 = [1, 2, 3, 4, 5, 6]
```

---

**Q25: What is the difference between `pop()` and `shift()` methods?**

| `pop()` | `shift()` |
|---|---|
| Removes the **last** element | Removes the **first** element |
| Returns the removed element | Returns the removed element |
| Mutates the original array | Mutates the original array |

```js
const arr = [1, 2, 3, 4];
arr.pop();    // returns 4, arr = [1, 2, 3]
arr.shift();  // returns 1, arr = [2, 3]
```

---

**Q26: What is the `splice()` method of an Array?**

`splice(start, deleteCount, ...items)` **modifies** the array in place — can remove, replace, or insert elements.

```js
const arr = [1, 2, 3, 4, 5];

// Remove 2 elements starting at index 1
arr.splice(1, 2);        // returns [2, 3], arr = [1, 4, 5]

// Insert without removing
arr.splice(1, 0, 99, 88); // arr = [1, 99, 88, 4, 5]

// Replace: remove 1 and insert 2 elements
arr.splice(2, 1, 'a', 'b'); // replaces index 2
```

---

**Q27: What is the difference between `slice()` and `splice()`?**

| `slice()` | `splice()` |
|---|---|
| Does NOT mutate the original | **Mutates** the original array |
| Returns a shallow copy | Returns removed elements |
| Read-only operation | Can remove, insert, replace |
| `slice(start, end)` | `splice(start, deleteCount, ...items)` |

---

**Q28: What is the difference between `map()` and `forEach()`?**

| `map()` | `forEach()` |
|---|---|
| Returns a **new array** | Returns `undefined` |
| Use when you need a transformed result | Use for side effects only |
| Does not mutate original | Does not mutate original |

```js
const nums = [1, 2, 3];

const doubled = nums.map(n => n * 2);  // [2, 4, 6] — new array
nums.forEach(n => console.log(n));     // logs each, returns undefined

// Use map() when transforming data:
const names = users.map(u => u.name);

// Use forEach() when side effects (logging, DOM updates):
items.forEach(item => item.classList.add('active'));
```

---

**Q29: How do you sort and reverse an array?**

```js
const nums = [3, 1, 4, 1, 5, 9];

// Reverse (mutates)
nums.reverse(); // [9, 5, 1, 4, 1, 3]

// Sort — default is lexicographic (string comparison)
[10, 2, 1].sort();           // [1, 10, 2] — WRONG for numbers!
[10, 2, 1].sort((a, b) => a - b); // [1, 2, 10] — correct ascending
[10, 2, 1].sort((a, b) => b - a); // [10, 2, 1] — descending

// Sort strings
['banana', 'apple', 'cherry'].sort(); // ['apple', 'banana', 'cherry']
```

---

**Q30: What is Array Destructuring in JS?**

Destructuring lets you **unpack** array values into variables in a single statement.

```js
const [a, b, c] = [1, 2, 3];
console.log(a, b, c); // 1 2 3

// Skip elements
const [first, , third] = [1, 2, 3];

// Default values
const [x = 0, y = 0] = [5];
console.log(x, y); // 5 0

// Swap variables
let p = 1, q = 2;
[p, q] = [q, p];
console.log(p, q); // 2 1

// Rest
const [head, ...tail] = [1, 2, 3, 4];
// head = 1, tail = [2, 3, 4]
```

---

**Q31: What are array-like objects in JS?**

Array-like objects have **indexed elements and a `length` property** but are not actual arrays — they don't have array methods like `map`, `filter`, `push`.

Examples: `arguments`, `NodeList` (from `querySelectorAll`), `HTMLCollection`, strings.

```js
function test() {
  console.log(arguments);      // array-like: { 0: 1, 1: 2, length: 2 }
  arguments.map(x => x);       // TypeError — not an array!
}
test(1, 2);
```

---

**Q32: How to convert an array-like object into an array?**

```js
// 1. Array.from()
const nodeList = document.querySelectorAll('div');
const arr = Array.from(nodeList); // real array
Array.from('hello');              // ['h', 'e', 'l', 'l', 'o']
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]

// 2. Spread operator
const args = [...arguments];

// 3. Array.prototype.slice.call (old way)
const arr2 = Array.prototype.slice.call(arguments);
```

---

## Loops

---

**Q33: What are loops? What are the types of loops in JS?**

Loops repeat a block of code multiple times.

| Loop | Best for |
|---|---|
| `for` | Known number of iterations |
| `while` | Unknown iterations, condition-based |
| `do...while` | At least one iteration guaranteed |
| `for...of` | Iterating values of iterables (arrays, strings) |
| `for...in` | Iterating keys of objects |
| `forEach` | Array-specific iteration |

---

**Q34: What is the difference between `for` and `while` loops?**

```js
// for — when you know the count upfront
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// while — when condition is checked before each iteration
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}
```

Use `for` when you know the range; use `while` when the end condition depends on runtime values.

---

**Q35: What is the difference between `while` and `do...while` loops?**

| `while` | `do...while` |
|---|---|
| Condition checked **before** first iteration | Condition checked **after** first iteration |
| May run **zero** times | Always runs **at least once** |

```js
// while — may never run
let x = 10;
while (x < 5) { console.log(x); } // never runs

// do...while — always runs at least once
let y = 10;
do {
  console.log(y); // runs once even though condition is false
} while (y < 5);
```

---

**Q36: What is the difference between `break` and `continue` statements?**

- **`break`** — exits the loop entirely.
- **`continue`** — skips the current iteration and moves to the next.

```js
for (let i = 0; i < 10; i++) {
  if (i === 3) continue; // skip 3
  if (i === 7) break;    // stop at 7
  console.log(i);        // prints: 0, 1, 2, 4, 5, 6
}
```

---

**Q37: What is the difference between `for` and `for...of` loops?**

| `for` | `for...of` |
|---|---|
| Index-based | Value-based |
| Manual index management | Cleaner, no index needed |
| Works on any iterable with index | Works on any iterable (array, string, Map, Set) |

```js
const arr = ['a', 'b', 'c'];

for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // index-based
}

for (const value of arr) {
  console.log(value); // direct value — cleaner
}
```

---

**Q38: What is the difference between `for...of` and `for...in` loops?**

| `for...of` | `for...in` |
|---|---|
| Iterates **values** of iterables | Iterates **keys** of objects |
| Arrays, strings, Maps, Sets | Objects (own + inherited enumerable props) |
| Cannot iterate plain objects | Can iterate arrays (but not recommended) |

```js
const arr = ['a', 'b', 'c'];
for (const val of arr) console.log(val);   // 'a', 'b', 'c'
for (const key in arr) console.log(key);   // '0', '1', '2'

const obj = { x: 1, y: 2 };
for (const key in obj) console.log(key);   // 'x', 'y'
// for...of on plain object throws TypeError
```

---

**Q39: What is `forEach` loop? Compare it with `for...of` and `for...in`.**

| | `forEach` | `for...of` | `for...in` |
|---|---|---|---|
| Works on | Arrays only | Any iterable | Objects (keys) |
| Returns | `undefined` | — | — |
| `break`/`continue` | Not supported | Supported | Supported |
| Async-friendly | No (can't await inside) | Yes | Yes |

```js
const arr = [1, 2, 3];
arr.forEach((val, index) => console.log(index, val));

// Cannot break out of forEach:
arr.forEach(val => {
  if (val === 2) return; // only skips current iteration (like continue)
});

// Use for...of when you need break or await:
for (const val of arr) {
  if (val === 2) break;
}
```

---

**Q40: When to use `for...of` loop vs `forEach` method?**

Use **`for...of`** when:
- You need `break` or `continue`.
- You use `async/await` inside the loop.
- Iterating non-array iterables (Map, Set, strings).

Use **`forEach`** when:
- Simpler array iteration with index access.
- No need to break or use async.

---

## Functions

---

**Q41: What are Functions in JS? What are the types of functions?**

A function is a **reusable block of code** that performs a specific task.

```js
// 1. Function declaration (hoisted)
function greet(name) { return `Hello, ${name}`; }

// 2. Function expression (not hoisted)
const greet = function(name) { return `Hello, ${name}`; };

// 3. Arrow function
const greet = (name) => `Hello, ${name}`;

// 4. Anonymous function
setTimeout(function() { console.log('done'); }, 1000);

// 5. IIFE (Immediately Invoked Function Expression)
(function() { console.log('runs immediately'); })();

// 6. Generator function
function* gen() { yield 1; yield 2; }

// 7. Async function
async function fetchData() { return await getData(); }
```

---

**Q42: What is the difference between named and anonymous functions?**

| Named function | Anonymous function |
|---|---|
| Has an identifier (`function foo()`) | No name (`function() {}`) |
| Hoisted (if declaration) | Not hoisted |
| Shows name in stack traces | Shows as `anonymous` in errors |
| Can reference itself (recursion) | Cannot reference itself easily |

```js
// Named — better for debugging and recursion
function factorial(n) {
  return n <= 1 ? 1 : n * factorial(n - 1);
}

// Anonymous — common in callbacks
arr.map(function(x) { return x * 2; });
arr.map(x => x * 2); // arrow function (also anonymous by nature)
```

---

**Q43: What is a function expression in JS?**

A function expression assigns a function to a variable. Unlike function declarations, they are **not hoisted**.

```js
const add = function(a, b) {
  return a + b;
};

add(2, 3); // 5

// Named function expression (has name for recursion/debugging)
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1);
};
```

---

**Q44: What are Arrow Functions in JS? What is their use?**

Arrow functions are a **concise syntax** for function expressions. Key difference: they do **not have their own `this`** — they inherit `this` from the enclosing scope.

```js
// Regular function
const add = function(a, b) { return a + b; };

// Arrow function
const add = (a, b) => a + b;
const square = x => x * x;        // single param: no parens needed
const hello = () => 'Hello!';     // no params

// this binding difference
const obj = {
  name: 'Alice',
  greet: function() {
    setTimeout(function() {
      console.log(this.name); // undefined — 'this' is window
    }, 100);

    setTimeout(() => {
      console.log(this.name); // 'Alice' — 'this' inherited from greet
    }, 100);
  }
};
```

---

**Q45: What is a callback function in JavaScript?**

A callback is a **function passed as an argument to another function**, to be called at a later time.

```js
function greet(name, callback) {
  console.log('Hello, ' + name);
  callback(); // call the passed function
}

greet('Alice', function() {
  console.log('Callback executed!');
});

// Common examples
[1, 2, 3].map(x => x * 2);              // map's callback
setTimeout(() => console.log('done'), 1000); // timer callback
button.addEventListener('click', handler);   // event callback
```

---

**Q46: What is a Higher-Order Function in JS?**

A Higher-Order Function (HOF) is a function that **takes another function as an argument** or **returns a function**.

```js
// Takes a function as argument
function repeat(n, action) {
  for (let i = 0; i < n; i++) action(i);
}
repeat(3, console.log); // 0, 1, 2

// Returns a function
function multiplier(factor) {
  return (number) => number * factor;
}
const double = multiplier(2);
const triple = multiplier(3);
double(5); // 10
triple(5); // 15

// Built-in HOFs: map, filter, reduce, forEach, sort
[1, 2, 3].map(x => x * 2);
[1, 2, 3].filter(x => x > 1);
[1, 2, 3].reduce((sum, x) => sum + x, 0);
```

---

**Q: What is the difference between arguments and parameters?**

- **Parameters** — variables listed in the function definition.
- **Arguments** — actual values passed when calling the function.

```js
function add(a, b) { // a, b are PARAMETERS
  return a + b;
}
add(5, 3); // 5, 3 are ARGUMENTS
```

---

**Q47: In how many ways can we pass parameters in functions in JS?**

```js
// 1. Positional
function greet(name, age) {}
greet('Alice', 30);

// 2. Default parameters
function greet(name = 'Guest', age = 0) {}
greet(); // uses defaults

// 3. Rest parameters
function sum(...nums) { return nums.reduce((a, b) => a + b, 0); }
sum(1, 2, 3, 4); // 10

// 4. Destructuring
function display({ name, age }) { console.log(name, age); }
display({ name: 'Alice', age: 30 });

// 5. Arguments object (old, non-arrow functions)
function old() { console.log(arguments); }
```

---

**Q48: What are Default Parameters in functions?**

Default parameters let you assign **fallback values** if an argument is `undefined` or not passed.

```js
function greet(name = 'Guest', greeting = 'Hello') {
  return `${greeting}, ${name}!`;
}
greet();                   // 'Hello, Guest!'
greet('Alice');            // 'Hello, Alice!'
greet('Bob', 'Hi');        // 'Hi, Bob!'
greet(undefined, 'Hey');   // 'Hey, Guest!' — undefined triggers default
greet(null, 'Hey');        // 'Hey, null' — null does NOT trigger default
```

---

**Q49: What is the use of event handling in JS?**

Event handling lets you **respond to user actions** (clicks, input, scroll, etc.) or browser events.

```js
// addEventListener (preferred)
const btn = document.getElementById('myBtn');
btn.addEventListener('click', function(event) {
  console.log('Button clicked!', event.target);
});

// Common events
element.addEventListener('click', handler);
element.addEventListener('keydown', handler);
element.addEventListener('submit', handler);
element.addEventListener('mouseover', handler);
element.addEventListener('change', handler);
element.addEventListener('load', handler);

// Remove event listener
element.removeEventListener('click', handler);
```

---

**Q50: What are First-Class Functions in JS?**

JS treats functions as **first-class citizens** — they can be:
- Assigned to variables.
- Passed as arguments.
- Returned from other functions.
- Stored in data structures.

```js
const greet = () => 'Hello';  // assigned to variable
arr.map(greet);                // passed as argument
function makeGreeter() { return () => 'Hi'; } // returned
const funcs = [greet, makeGreeter()]; // stored in array
```

---

**Q51: What are Pure and Impure Functions in JS?**

**Pure function:**
- Same input **always** returns the same output.
- No **side effects** (no mutation of external state).

```js
// Pure
const add = (a, b) => a + b; // always same result, no side effects

// Impure — depends on external state
let count = 0;
function increment() { count++; } // modifies external variable

// Impure — different results each call
function getRandom() { return Math.random(); }
```

---

**Q52: What is Function Currying in JS?**

Currying transforms a function with **multiple arguments** into a **sequence of functions**, each taking one argument.

```js
// Normal function
const add = (a, b, c) => a + b + c;
add(1, 2, 3); // 6

// Curried version
const curriedAdd = a => b => c => a + b + c;
curriedAdd(1)(2)(3); // 6
const add1 = curriedAdd(1);    // partial application
const add1and2 = add1(2);      // partial application
add1and2(3); // 6

// Practical use
const multiply = factor => number => number * factor;
const double = multiply(2);
const triple = multiply(3);
[1, 2, 3].map(double); // [2, 4, 6]
```

---

**Q53: `call()`, `apply()`, and `bind()` methods in JS?**

All three explicitly set the `this` context of a function.

```js
function introduce(greeting, punctuation) {
  return `${greeting}, I'm ${this.name}${punctuation}`;
}
const alice = { name: 'Alice' };

// call() — invokes immediately, args passed individually
introduce.call(alice, 'Hi', '!');  // "Hi, I'm Alice!"

// apply() — invokes immediately, args passed as array
introduce.apply(alice, ['Hello', '.']); // "Hello, I'm Alice."

// bind() — returns a NEW function with 'this' bound, doesn't invoke
const aliceIntro = introduce.bind(alice, 'Hey');
aliceIntro('?'); // "Hey, I'm Alice?"
```

---

## Strings

---

**Q54: What is a string?**

A string is a sequence of characters enclosed in single quotes, double quotes, or backticks. Strings are **immutable** — you cannot change individual characters.

```js
const name = 'Alice';
const city = "London";
const greeting = `Hello, ${name}!`; // template literal
```

---

**Q55: Template Literals and String Interpolation**

Template literals (backticks) allow **embedded expressions**, multi-line strings, and cleaner concatenation.

```js
const name = 'Alice';
const age = 30;

// Old way
const msg = 'Hello, ' + name + '! You are ' + age + '.';

// Template literal
const msg = `Hello, ${name}! You are ${age}.`;

// Multi-line
const html = `
  <div>
    <h1>${name}</h1>
  </div>
`;

// Expression inside ${}
console.log(`${2 + 2}`);         // '4'
console.log(`${age >= 18 ? 'Adult' : 'Minor'}`);
```

---

**Q56: Single quotes vs double quotes vs backticks**

| | Single `'` | Double `"` | Backtick `` ` `` |
|---|---|---|---|
| Interpolation | No | No | Yes (`${}`) |
| Multi-line | No | No | Yes |
| Escape inner | `\'` | `\"` | Rarely needed |

All are equivalent for basic strings. Backticks are preferred for dynamic/multi-line strings.

---

**Q57: String Operations in JS**

```js
const str = 'Hello, World!';

str.length;               // 13
str.toUpperCase();        // 'HELLO, WORLD!'
str.toLowerCase();        // 'hello, world!'
str.includes('World');    // true
str.startsWith('Hello');  // true
str.endsWith('!');        // true
str.indexOf('o');         // 4
str.slice(7, 12);         // 'World'
str.replace('World', 'JS'); // 'Hello, JS!'
str.split(', ');          // ['Hello', 'World!']
str.trim();               // removes leading/trailing whitespace
str.padStart(15, '*');    // '**Hello, World!'
str.repeat(2);            // 'Hello, World!Hello, World!'
```

---

**Q58: What is string immutability?**

Strings cannot be **changed in place** — any string method returns a **new string**.

```js
let str = 'hello';
str[0] = 'H';        // silently fails — no error, no change
console.log(str);    // 'hello' — unchanged

// String methods return NEW strings
const newStr = str.toUpperCase(); // 'HELLO'
console.log(str);    // 'hello' — original unchanged
```

---

**Q59: Different ways to concatenate strings in JS**

```js
const a = 'Hello';
const b = 'World';

// 1. + operator
const s1 = a + ', ' + b + '!';

// 2. Template literal (preferred)
const s2 = `${a}, ${b}!`;

// 3. concat() method
const s3 = a.concat(', ', b, '!');

// 4. join() for arrays
const s4 = [a, b].join(', ');

// 5. Array + join for building strings
const parts = [];
parts.push(a);
parts.push(b);
const s5 = parts.join(', ');
```

---

## DOM

---

**Q60: How do you select, modify, or remove DOM elements?**

```js
// Select
document.getElementById('myId');
document.querySelector('.myClass');        // first match
document.querySelectorAll('p');            // all matches (NodeList)
document.getElementsByClassName('active'); // HTMLCollection

// Modify
const el = document.getElementById('title');
el.textContent = 'New Title';
el.innerHTML = '<strong>Bold</strong>';
el.setAttribute('class', 'active');
el.style.color = 'red';

// Remove
el.remove();                          // remove element
el.parentNode.removeChild(el);        // old way
```

---

**Q61: Selectors in JS DOM**

```js
// By ID
document.getElementById('header');

// By class
document.getElementsByClassName('btn');

// By tag
document.getElementsByTagName('p');

// CSS selectors (modern, preferred)
document.querySelector('#header');        // first match
document.querySelector('.btn.active');    // compound selector
document.querySelectorAll('ul > li');     // all matching
```

---

**Q62: Difference between `getElementById`, `getElementsByClassName`, and `getElementsByTagName`?**

| Method | Returns | Live? |
|---|---|---|
| `getElementById` | Single Element or `null` | No |
| `getElementsByClassName` | HTMLCollection | Yes (live) |
| `getElementsByTagName` | HTMLCollection | Yes (live) |
| `querySelector` | First matching Element | No |
| `querySelectorAll` | Static NodeList | No |

A **live** collection updates automatically when the DOM changes.

---

**Q63: What is the difference between `querySelector()` and `querySelectorAll()`?**

```js
// querySelector — returns FIRST matching element (or null)
const first = document.querySelector('.card'); // one element

// querySelectorAll — returns ALL matching elements as a static NodeList
const all = document.querySelectorAll('.card'); // NodeList
all.forEach(card => card.classList.add('active'));
```

---

**Q64: Methods to manipulate elements, properties and attributes of JS DOM**

```js
const el = document.querySelector('#box');

// Attributes
el.getAttribute('href');
el.setAttribute('href', 'https://example.com');
el.removeAttribute('disabled');
el.hasAttribute('class');

// Properties
el.id;
el.className;
el.classList.add('active');
el.classList.remove('hidden');
el.classList.toggle('open');
el.classList.contains('active'); // true/false

// Content
el.textContent = 'text only';
el.innerHTML = '<span>html content</span>';

// Style
el.style.backgroundColor = 'blue';
el.style.display = 'none';
```

---

**Q65: What is the difference between `innerHTML` and `textContent`?**

| `innerHTML` | `textContent` |
|---|---|
| Parses and renders HTML | Plain text only — no HTML parsing |
| Risk of XSS injection | Safe — escapes HTML |
| Slower (HTML parsing) | Faster |

```js
el.innerHTML = '<b>Bold</b>';    // renders as bold text
el.textContent = '<b>Bold</b>';  // shows literal string "<b>Bold</b>"

// Always use textContent when inserting user-provided text!
el.textContent = userInput; // safe from XSS
el.innerHTML = userInput;   // DANGEROUS if userInput contains scripts
```

---

**Q66: How to add or remove properties of HTML elements from the DOM?**

```js
const input = document.querySelector('input');

// Add property
input.disabled = true;
input.value = 'Hello';

// Remove property
input.disabled = false;
delete input.dataset.myProp;

// classList for CSS classes
el.classList.add('active');
el.classList.remove('active');
el.classList.toggle('active');
```

---

**Q67: How to add or remove style of HTML elements in the DOM?**

```js
const el = document.querySelector('.box');

// Inline styles
el.style.color = 'red';
el.style.backgroundColor = 'blue';
el.style.display = 'none';

// Remove inline style
el.style.color = ''; // resets to stylesheet value

// Add/remove CSS classes (preferred approach)
el.classList.add('hidden');
el.classList.remove('hidden');
el.classList.toggle('dark-mode');

// getComputedStyle — get actual applied styles
const styles = window.getComputedStyle(el);
console.log(styles.fontSize);
```

---

**Q68: How to create new elements in DOM? Difference between `createElement()` and `cloneNode()`?**

```js
// createElement — creates a brand new element
const div = document.createElement('div');
div.textContent = 'New element';
div.classList.add('card');
document.body.appendChild(div);

// cloneNode — copies an existing element
const original = document.querySelector('.card');
const clone = original.cloneNode(true); // true = deep clone (with children)
const shallowClone = original.cloneNode(false); // false = element only, no children
document.body.appendChild(clone);
```

| `createElement()` | `cloneNode()` |
|---|---|
| Creates fresh element | Copies existing element |
| No event listeners | Copies structure, NOT event listeners |
| More control | Faster for complex structures |

---

## Error Handling in JS

---

**Q69: What is Error Handling in JS?**

Error handling is anticipating and gracefully managing runtime errors so the program doesn't crash.

```js
try {
  // code that might throw
  const data = JSON.parse('{ invalid }');
} catch (err) {
  // handle the error
  console.error('Error:', err.message);
} finally {
  // always runs
  console.log('Cleanup done');
}
```

---

**Q70: What is the role of the `finally` block in JS?**

`finally` always executes — whether an error was thrown or not, and even if there's a `return` in `try` or `catch`.

```js
function readFile() {
  try {
    return processFile();
  } catch (err) {
    console.error(err);
    return null;
  } finally {
    closeFileHandle(); // ALWAYS runs — great for cleanup
  }
}
```

Use `finally` for: closing database connections, releasing resources, clearing timers.

---

**Q71: What is the purpose of the `throw` statement in JS?**

`throw` lets you **manually create and throw an error** with a custom message or type.

```js
function validateAge(age) {
  if (typeof age !== 'number') throw new TypeError('Age must be a number');
  if (age < 0 || age > 150) throw new RangeError('Age out of valid range');
  return age;
}

try {
  validateAge(-5);
} catch (err) {
  console.error(err.name, err.message); // RangeError Age out of valid range
}

// Throw custom error objects
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ValidationError';
  }
}
throw new ValidationError('Invalid input');
```

---

**Q72: What is Error Propagation in JS?**

When an error is thrown and not caught in the current function, it **propagates up the call stack** until it's caught by a `try-catch` or crashes the program.

```js
function c() { throw new Error('Error in c'); }
function b() { c(); }  // error propagates from c to b
function a() { b(); }  // error propagates from b to a

try {
  a(); // caught here
} catch (err) {
  console.error(err.message); // 'Error in c'
}
```

---

**Q73: Error Handling Best Practices**

1. Always catch errors at the appropriate level.
2. Use specific error types (`TypeError`, `RangeError`, custom errors).
3. Never swallow errors silently: `catch(err) {}` is a red flag.
4. Use `finally` for cleanup.
5. Log errors with context (timestamp, request ID).
6. In async code, always use `try/catch` with `await`.
7. Use error-handling middleware in Express.
8. Don't expose internal error details to clients.

---

**Q74: What are the different types of errors in JS?**

| Error Type | Cause |
|---|---|
| `SyntaxError` | Invalid code syntax (parse time) |
| `ReferenceError` | Accessing undefined variable |
| `TypeError` | Wrong type operation (call non-function) |
| `RangeError` | Value out of allowed range |
| `URIError` | Bad URI encoding/decoding |
| `EvalError` | Issue with `eval()` |
| Custom Error | User-defined via `extends Error` |

```js
null.property;          // TypeError
undeclaredVar;          // ReferenceError
new Array(-1);          // RangeError
eval('{ invalid');      // SyntaxError
```

---

## Objects

---

**Q75: What are objects in JS?**

An object is a **collection of key-value pairs** (properties and methods). It's the most fundamental data structure in JavaScript.

```js
const user = {
  name: 'Alice',
  age: 30,
  isActive: true,
  address: { city: 'London' },   // nested object
  greet() { return `Hi, I'm ${this.name}`; }  // method
};
user.greet(); // "Hi, I'm Alice"
```

---

**Q76: In how many ways can we create an object?**

```js
// 1. Object literal (most common)
const obj = { name: 'Alice' };

// 2. Object constructor
const obj = new Object();
obj.name = 'Alice';

// 3. Constructor function
function Person(name) { this.name = name; }
const alice = new Person('Alice');

// 4. ES6 Class
class Person {
  constructor(name) { this.name = name; }
}
const alice = new Person('Alice');

// 5. Object.create() — sets prototype
const proto = { greet() { return 'Hi'; } };
const obj = Object.create(proto);

// 6. Factory function
function createPerson(name) { return { name, greet() { return 'Hi'; } }; }
```

---

**Q77: What is the difference between an array and an object?**

| Array | Object |
|---|---|
| Ordered, indexed by numbers | Unordered, indexed by string keys |
| `typeof [] === 'object'` | `typeof {} === 'object'` |
| `Array.isArray([])` → `true` | Use for key-value data |
| Use for lists/sequences | Use for named data |

```js
const arr = ['apple', 'banana']; // indexed by 0, 1
const obj = { fruit1: 'apple', fruit2: 'banana' }; // named keys
```

---

**Q78: How to manipulate Objects in JS?**

```js
const user = { name: 'Alice', age: 30 };

// Add property
user.email = 'alice@example.com';

// Update property
user.age = 31;

// Delete property
delete user.age;

// Check if property exists
'name' in user;           // true
user.hasOwnProperty('name'); // true

// Get all keys/values/entries
Object.keys(user);         // ['name', 'email']
Object.values(user);       // ['Alice', 'alice@example.com']
Object.entries(user);      // [['name', 'Alice'], ['email', '...']]
```

---

**Q79: Dot Notation vs Bracket Notation**

```js
const user = { name: 'Alice', 'first name': 'Alice' };

// Dot notation — simple, clean
user.name;              // 'Alice'
user.name = 'Bob';

// Bracket notation — required for:
user['first name'];     // keys with spaces
user['name'];           // same as user.name

const key = 'name';
user[key];              // dynamic key access
user['first' + ' ' + 'name']; // computed key
```

---

**Q80: How to iterate through Objects in JS?**

```js
const obj = { a: 1, b: 2, c: 3 };

// for...in (own + inherited enumerable props)
for (const key in obj) {
  if (obj.hasOwnProperty(key)) console.log(key, obj[key]);
}

// Object.keys() — only own enumerable keys
Object.keys(obj).forEach(key => console.log(key, obj[key]));

// Object.entries() — key-value pairs
for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}

// Object.values()
Object.values(obj).forEach(val => console.log(val));
```

---

**Q81: How to check if a property exists?**

```js
const obj = { name: 'Alice', age: undefined };

// 'in' operator — checks own AND inherited properties
'name' in obj;           // true
'toString' in obj;       // true (inherited)

// hasOwnProperty — only OWN properties
obj.hasOwnProperty('name');    // true
obj.hasOwnProperty('toString'); // false

// Optional chaining — safe access
obj?.address?.city;      // undefined (no error)

// Direct check (careful with falsy values)
obj.name !== undefined;  // true
// BUT: obj.age !== undefined → false even though age exists (it's undefined)
// Use 'in' or hasOwnProperty for reliable checks
```

---

**Q82: How to clone an object?**

```js
const original = { name: 'Alice', scores: [1, 2, 3] };

// Shallow clone (nested objects still shared)
const clone1 = Object.assign({}, original);
const clone2 = { ...original };

// Deep clone (fully independent copy)
const deepClone = JSON.parse(JSON.stringify(original)); // simple, but loses functions/dates
const deepClone2 = structuredClone(original);           // modern, handles more types
```

---

**Q83: What is the difference between deep copy and shallow copy in JS?**

| Shallow Copy | Deep Copy |
|---|---|
| Copies only the top-level properties | Copies all levels recursively |
| Nested objects are still **shared** | Nested objects are fully **independent** |
| `Object.assign`, spread `...` | `structuredClone`, `JSON.parse(JSON.stringify(...))` |

```js
const obj = { a: 1, nested: { b: 2 } };

// Shallow copy
const shallow = { ...obj };
shallow.nested.b = 99;
console.log(obj.nested.b); // 99 — SHARED reference!

// Deep copy
const deep = structuredClone(obj);
deep.nested.b = 99;
console.log(obj.nested.b); // 2 — independent!
```

---

**Q84: Sets in JS**

A `Set` is a collection of **unique values** — no duplicates allowed.

```js
const set = new Set([1, 2, 3, 2, 1]);
console.log(set); // Set {1, 2, 3} — duplicates removed

set.add(4);
set.delete(2);
set.has(3);       // true
set.size;         // 3
set.clear();

// Iterate
for (const val of set) console.log(val);
[...set];         // convert to array

// Common use: remove duplicates from array
const unique = [...new Set([1, 2, 2, 3, 3])]; // [1, 2, 3]
```

---

**Q84: Map Object in JS**

A `Map` is a collection of **key-value pairs** where keys can be ANY type (unlike plain objects where keys are strings/symbols).

```js
const map = new Map();

// Set key-value pairs
map.set('name', 'Alice');
map.set(42, 'answer');
map.set({ id: 1 }, 'user object key');

// Access
map.get('name');    // 'Alice'
map.has(42);        // true
map.delete(42);
map.size;           // 2

// Iterate
for (const [key, value] of map) console.log(key, value);
map.forEach((value, key) => console.log(key, value));
[...map.entries()]; // [[key, val], ...]

// Map vs Object
// Map: any key type, maintains insertion order, has .size
// Object: string/symbol keys only, faster for simple lookups
```

---

## Events in JS

---

**Q85: Events in JS**

An event is something that **happens in the browser** — user interaction or browser action.

```js
// addEventListener (preferred)
element.addEventListener('click', handler);
element.addEventListener('keydown', (e) => console.log(e.key));
element.addEventListener('submit', (e) => { e.preventDefault(); });

// Common events
// click, dblclick, mouseover, mouseout, mouseenter, mouseleave
// keydown, keyup, keypress
// submit, change, input, focus, blur
// load, DOMContentLoaded, resize, scroll
```

---

**Q86: Event Delegation in JS**

Instead of adding listeners to each child, add **one listener to the parent** and use `event.target` to identify which child was clicked.

```js
// Bad — a listener for each item
document.querySelectorAll('li').forEach(li => {
  li.addEventListener('click', handler);
});

// Good — one listener on the parent (event delegation)
document.querySelector('ul').addEventListener('click', (e) => {
  if (e.target.tagName === 'LI') {
    console.log('Clicked:', e.target.textContent);
  }
});
```

**Benefits:** Works for dynamically added elements, fewer event listeners, better performance.

---

**Q87: Event Bubbling and Event Capturing in JS**

Events travel in 3 phases:
1. **Capturing** — from root → target (top-down).
2. **Target** — at the element.
3. **Bubbling** — from target → root (bottom-up).

By default, handlers use **bubbling**.

```js
// Bubbling (default — false/omitted)
parent.addEventListener('click', () => console.log('Parent'), false);
child.addEventListener('click', () => console.log('Child'), false);
// Click child → logs: 'Child', then 'Parent'

// Capturing (true)
parent.addEventListener('click', () => console.log('Parent'), true);
// Click child → logs: 'Parent', then 'Child'

// Stop propagation
child.addEventListener('click', (e) => {
  e.stopPropagation(); // prevents bubbling up
});
```

---

**Q88: `event.preventDefault()` method in JS**

Prevents the **default browser behavior** for an event (form submission, link navigation, etc.).

```js
// Prevent form from reloading page
form.addEventListener('submit', (e) => {
  e.preventDefault();
  // handle form data manually
});

// Prevent link navigation
link.addEventListener('click', (e) => {
  e.preventDefault();
  // do custom routing instead
});

// Prevent right-click context menu
document.addEventListener('contextmenu', (e) => e.preventDefault());
```

---

**Q: The use of `this` keyword in the context of event handling**

In a regular function event handler, `this` refers to the element the listener is attached to.

```js
button.addEventListener('click', function() {
  console.log(this); // the button element
  this.classList.toggle('active');
});

// Arrow function — 'this' is the enclosing context, NOT the element
button.addEventListener('click', () => {
  console.log(this); // window (or undefined in strict mode)
});
```

Use regular functions when you need `this` to refer to the element.

---

**Q89: How to remove (unattach) an event handler from an element?**

```js
// Must reference the same function — anonymous functions cannot be removed
function handleClick() {
  console.log('clicked');
}

element.addEventListener('click', handleClick);
element.removeEventListener('click', handleClick); // removes it

// AbortController — modern way to remove multiple listeners
const controller = new AbortController();
element.addEventListener('click', handler, { signal: controller.signal });
element.addEventListener('scroll', handler, { signal: controller.signal });
controller.abort(); // removes all listeners added with this signal
```
