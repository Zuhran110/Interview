# JavaScript — Strings

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
