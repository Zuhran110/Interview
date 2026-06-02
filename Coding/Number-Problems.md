# Coding — Number Problems

---

## 1. Calculate factorial (recursion)
```js
function factorial(n) {
  if (n < 0) throw new Error('Negative numbers not allowed');
  if (n === 0 || n === 1) return 1;
  return n * factorial(n - 1);
}
console.log(factorial(5)); // 120
```

---

## 2. Reverse a number
```js
function reverseNumber(num) {
  const isNegative = num < 0;
  const reversed = Math.abs(num).toString().split('').reverse().join('');
  return isNegative ? -Number(reversed) : Number(reversed);
}
console.log(reverseNumber(12345)); // 54321
```

---
