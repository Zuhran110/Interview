# JavaScript — Operators & Conditions

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
