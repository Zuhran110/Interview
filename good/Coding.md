# JavaScript Coding Exercises — Interview-Focused

---

## 1. Reverse a string
```js
function reverseString(str) {
  return str.split('').reverse().join('');
}
console.log(reverseString('hello')); // 'olleh'
```

---

## 2. Check if a string is a palindrome
```js
function isPalindrome(str) {
  const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  return cleaned === cleaned.split('').reverse().join('');
}
console.log(isPalindrome('racecar')); // true
```

---

## 3. Calculate factorial (recursion)
```js
function factorial(n) {
  if (n < 0) throw new Error('Negative numbers not allowed');
  if (n === 0 || n === 1) return 1;
  return n * factorial(n - 1);
}
console.log(factorial(5)); // 120
```

---

## 4. Count character frequency in a string
```js
function countCharacters(str) {
  const count = {};
  for (const char of str) {
    count[char] = (count[char] || 0) + 1;
  }
  return count;
}
console.log(countCharacters('banana')); // { b: 1, a: 3, n: 2 }
```

---

## 5. Sum all numbers in an array
```js
function sumArray(arr) {
  return arr.reduce((total, num) => total + num, 0);
}
console.log(sumArray([1, 2, 3, 4, 5])); // 15
```

---

## 6. Filter numeric values from mixed array
```js
function filterNumeric(arr) {
  return arr.filter(item => typeof item === 'number' && !isNaN(item));
}
console.log(filterNumeric([1, 'a', 2, null, 3, true, NaN, 4])); // [1, 2, 3, 4]
```

---

## 7. Reverse a number
```js
function reverseNumber(num) {
  const isNegative = num < 0;
  const reversed = Math.abs(num).toString().split('').reverse().join('');
  return isNegative ? -Number(reversed) : Number(reversed);
}
console.log(reverseNumber(12345)); // 54321
```

---

## 8. Title case a sentence
```js
function titleCase(str) {
  return str
    .split(' ')
    .map(word => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
    .join(' ');
}
console.log(titleCase('the quick BROWN fox')); // 'The Quick Brown Fox'
```

---

## 9. Remove objects from array by id
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

## 10. Bubble sort
```js
function bubbleSort(arr) {
  const a = [...arr];
  for (let i = 0; i < a.length - 1; i++) {
    for (let j = 0; j < a.length - 1 - i; j++) {
      if (a[j] > a[j + 1]) {
        [a[j], a[j + 1]] = [a[j + 1], a[j]];
      }
    }
  }
  return a;
}
console.log(bubbleSort([5, 3, 8, 1, 2])); // [1, 2, 3, 5, 8]
```

---

## 11. Shuffle an array (Fisher-Yates)
```js
function shuffleArray(arr) {
  const shuffled = [...arr];
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
  }
  return shuffled;
}
console.log(shuffleArray([1, 2, 3, 4, 5]));
```

---

## 12. Union of two arrays
```js
function union(arr1, arr2) {
  return [...new Set([...arr1, ...arr2])];
}
console.log(union([1, 2, 3], [2, 3, 4, 5])); // [1, 2, 3, 4, 5]
```

---

## 13. Return first and last n elements
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

## 14. Reverse each word in a sentence
```js
function reverseEachWord(sentence) {
  return sentence
    .split(' ')
    .map(word => word.split('').reverse().join(''))
    .join(' ');
}
console.log(reverseEachWord('I love coding')); // 'I evol gnidoc'
```