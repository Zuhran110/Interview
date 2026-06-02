# Coding — Array Problems

---

## 1. Sum all numbers in an array
```js
function sumArray(arr) {
  return arr.reduce((total, num) => total + num, 0);
}
console.log(sumArray([1, 2, 3, 4, 5])); // 15
```

---

## 2. Filter numeric values from mixed array
```js
function filterNumeric(arr) {
  return arr.filter(item => typeof item === 'number' && !isNaN(item));
}
console.log(filterNumeric([1, 'a', 2, null, 3, true, NaN, 4])); // [1, 2, 3, 4]
```

---

## 3. Remove objects from array by id
```js
const users = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
  { id: 3, name: 'Charlie' },
];

const filtered = users.filter(user => user.id !== 2);
console.log(filtered);
```

---

## 4. Union of two arrays
```js
function union(arr1, arr2) {
  return [...new Set([...arr1, ...arr2])];
}
console.log(union([1, 2, 3], [2, 3, 4, 5])); // [1, 2, 3, 4, 5]
```

---

## 5. Return first and last n elements
```js
function firstN(arr, n) {
  return arr.slice(0, n);
}

function lastN(arr, n) {
  return arr.slice(-n);
}

console.log(firstN([1, 2, 3, 4, 5], 3)); // [1, 2, 3]
console.log(lastN([1, 2, 3, 4, 5], 2));  // [4, 5]
```

---
