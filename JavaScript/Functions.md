# JavaScript — Functions

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
