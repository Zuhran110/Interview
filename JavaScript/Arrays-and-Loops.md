# JavaScript — Arrays & Loops

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
