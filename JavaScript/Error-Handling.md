# JavaScript — Error Handling

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
