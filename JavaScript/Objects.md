# JavaScript — Objects, Sets & Maps

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
