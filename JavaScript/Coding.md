# JavaScript Coding Exercises — Solutions

---

## Part 1: Basic Exercises

---

**1. Get the full year**
```js
const year = new Date().getFullYear();
console.log(year); // e.g., 2026
```

---

**2. Concatenate and log first and last name**
```js
const firstName = 'John';
const lastName = 'Doe';
console.log(firstName + ' ' + lastName);
// or:
console.log(`${firstName} ${lastName}`);
```

---

**3. Track a variable's value**
```js
let score = 10;
console.log('Current score:', score);
score = 20;
console.log('Updated score:', score);
```

---

**4. Log an error message**
```js
console.error('Something went wrong: file not found');
```

---

**5. Log the square of 12**
```js
console.log(12 ** 2); // 144
// or:
console.log(Math.pow(12, 2)); // 144
```

---

**6. Print the type of a boolean variable**
```js
const isActive = true;
console.log(typeof isActive); // "boolean"
```

---

**7. Log if age is greater than 18**
```js
const age = 20;
console.log(age > 18); // true
```

---

**8. Print infinity**
```js
console.log(1 / 0);       // Infinity
console.log(Infinity);    // Infinity
console.log(Number.POSITIVE_INFINITY); // Infinity
```

---

**9. Declare a variable using `let` and log it**
```js
let city = 'London';
console.log(city); // 'London'
```

---

**10. Store the value of PI in a constant**
```js
const PI = Math.PI;
console.log(PI); // 3.141592653589793
```

---

**11. Reassign a variable declared with `let`**
```js
let temperature = 25;
console.log(temperature); // 25
temperature = 30;
console.log(temperature); // 30
```

---

**12. Check the type of `null`**
```js
console.log(typeof null); // "object" — this is a known JS quirk/bug
```

---

**13. Check the type of a boolean variable**
```js
const flag = false;
console.log(typeof flag); // "boolean"
```

---

**14. Create three variables (string, number, boolean) and log them**
```js
const name = 'Alice';
const age = 30;
const isStudent = false;
console.log(name, age, isStudent); // Alice 30 false
```

---

**15. Declare a variable without assigning a value and log its type**
```js
let x;
console.log(typeof x); // "undefined"
```

---

**16. Create a variable with `undefined` and log its type**
```js
let value = undefined;
console.log(typeof value); // "undefined"
```

---

**17. Use a constant to create an array and try to reassign it**
```js
const arr = [1, 2, 3];
arr.push(4);   // OK — mutating the array contents is allowed
console.log(arr); // [1, 2, 3, 4]

// arr = [5, 6]; // TypeError: Assignment to constant variable
```

---

**18. For loop to print 1 to 50**
```js
for (let i = 1; i <= 50; i++) {
  console.log(i);
}
```

---

**19. While loop to sum numbers from 1 to 10**
```js
let sum = 0;
let i = 1;
while (i <= 10) {
  sum += i;
  i++;
}
console.log(sum); // 55
```

---

**20. For...of loop to log each character of a string**
```js
const str = 'Hello';
for (const char of str) {
  console.log(char); // H, e, l, l, o
}
```

---

**21. For loop to skip even numbers between 1 and 20**
```js
for (let i = 1; i <= 20; i++) {
  if (i % 2 === 0) continue;
  console.log(i); // 1, 3, 5, 7, 9, 11, 13, 15, 17, 19
}
```

---

**22. Array reversal (manual logic)**
```js
function reverseArray(arr) {
  const reversed = [];
  for (let i = arr.length - 1; i >= 0; i--) {
    reversed.push(arr[i]);
  }
  return reversed;
}
console.log(reverseArray([1, 2, 3, 4, 5])); // [5, 4, 3, 2, 1]
```

---

**23. While loop to log numbers from 1 to 100 divisible by 5**
```js
let i = 1;
while (i <= 100) {
  if (i % 5 === 0) console.log(i);
  i++;
}
// 5, 10, 15, 20, ..., 100
```

---

**24. For...in loop to log object keys**
```js
const person = { name: 'Alice', age: 30, city: 'London' };
for (const key in person) {
  console.log(key, ':', person[key]);
}
// name : Alice
// age : 30
// city : London
```

---

**25. Create an array of 5 favorite movies and use `forEach`**
```js
const movies = ['Inception', 'Interstellar', 'The Matrix', 'Dark Knight', 'Parasite'];
movies.forEach((movie, index) => {
  console.log(`${index + 1}. ${movie}`);
});
```

---

**26. Find and log the second element of an array**
```js
const arr = [10, 20, 30, 40, 50];
console.log(arr[1]); // 20
```

---

**27. Add two elements to the start of an array using `unshift`**
```js
const arr = [3, 4, 5];
arr.unshift(1, 2);
console.log(arr); // [1, 2, 3, 4, 5]
```

---

**28. Remove the last element of an array using `pop`**
```js
const arr = [1, 2, 3, 4];
const removed = arr.pop();
console.log(removed); // 4
console.log(arr);     // [1, 2, 3]
```

---

**29. Extract first three elements using `slice`**
```js
const arr = [10, 20, 30, 40, 50];
const first3 = arr.slice(0, 3);
console.log(first3); // [10, 20, 30]
```

---

**30. Find the index of a specific element using `indexOf`**
```js
const fruits = ['apple', 'banana', 'cherry', 'banana'];
console.log(fruits.indexOf('banana'));  // 1 (first occurrence)
console.log(fruits.indexOf('grape'));   // -1 (not found)
```

---

**31. Check if an element exists in an array using `includes`**
```js
const nums = [1, 2, 3, 4, 5];
console.log(nums.includes(3)); // true
console.log(nums.includes(9)); // false
```

---

**32. Combine two arrays using `concat`**
```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = arr1.concat(arr2);
console.log(combined); // [1, 2, 3, 4, 5, 6]
// or with spread:
const combined2 = [...arr1, ...arr2];
```

---

**33. Sort an array of numbers (Bubble Sort)**
```js
function bubbleSort(arr) {
  const a = [...arr]; // don't mutate original
  for (let i = 0; i < a.length - 1; i++) {
    for (let j = 0; j < a.length - 1 - i; j++) {
      if (a[j] > a[j + 1]) {
        [a[j], a[j + 1]] = [a[j + 1], a[j]]; // swap
      }
    }
  }
  return a;
}
console.log(bubbleSort([5, 3, 8, 1, 2])); // [1, 2, 3, 5, 8]
```

---

**34. Create a copy of an array without mutating the original**
```js
const original = [1, 2, 3, 4];

const copy1 = [...original];             // spread
const copy2 = original.slice();          // slice
const copy3 = Array.from(original);      // Array.from
const copy4 = Object.assign([], original); // Object.assign

copy1.push(99);
console.log(original); // [1, 2, 3, 4] — unchanged
console.log(copy1);    // [1, 2, 3, 4, 99]
```

---

**35. Calculate factorial using a function**
```js
function factorial(n) {
  if (n < 0) throw new Error('Negative numbers not allowed');
  if (n === 0 || n === 1) return 1;
  return n * factorial(n - 1);
}
console.log(factorial(5)); // 120
console.log(factorial(0)); // 1
```

---

**36. Reverse a string using a function**
```js
function reverseString(str) {
  return str.split('').reverse().join('');
}
console.log(reverseString('hello'));  // 'olleh'
console.log(reverseString('abcde'));  // 'edcba'
```

---

**37. Replace spaces in a string with a hyphen**
```js
function replaceSpaces(str) {
  return str.split(' ').join('-');
  // or: return str.replace(/ /g, '-');
  // or: return str.replaceAll(' ', '-');
}
console.log(replaceSpaces('hello world foo')); // 'hello-world-foo'
```

---

**38. Create a function that logs 'Hello World'**
```js
function sayHello() {
  console.log('Hello World');
}
sayHello(); // Hello World
```

---

## Part 2: Practice Questions

---

**1. Reverse each word in a sentence**
```js
function reverseEachWord(sentence) {
  return sentence
    .split(' ')
    .map(word => word.split('').reverse().join(''))
    .join(' ');
}
console.log(reverseEachWord('Hello World'));    // 'olleH dlroW'
console.log(reverseEachWord('I love coding')); // 'I evol gnidoc'
```

---

**2. Check if an element is an array or not**
```js
function isArray(val) {
  return Array.isArray(val);
}
console.log(isArray([1, 2, 3]));  // true
console.log(isArray('hello'));     // false
console.log(isArray({ a: 1 }));   // false
console.log(isArray(null));        // false
```

---

**3. Empty an array in JavaScript**
```js
const arr = [1, 2, 3, 4, 5];

// Method 1: Set length to 0 (empties in place — all references see the change)
arr.length = 0;

// Method 2: Reassign (original reference lost)
let arr2 = [1, 2, 3];
arr2 = [];

// Method 3: splice
arr.splice(0, arr.length);

console.log(arr); // []
```

---

**4. Check if a number is an integer**
```js
function isInteger(num) {
  return Number.isInteger(num);
}
console.log(isInteger(4));     // true
console.log(isInteger(4.5));   // false
console.log(isInteger(-3));    // true
console.log(isInteger('4'));   // false — only checks numbers
```

---

**5. Duplicate an array (making a copy)**
```js
const original = [1, 2, 3, 'hello', true];

const copy = [...original];
// or:
const copy2 = original.slice();
// or:
const copy3 = Array.from(original);

console.log(copy);                      // [1, 2, 3, 'hello', true]
console.log(copy === original);         // false — different reference
```

---

**6. Reverse a number**
```js
function reverseNumber(num) {
  const isNegative = num < 0;
  const reversed = Math.abs(num)
    .toString()
    .split('')
    .reverse()
    .join('');
  return isNegative ? -Number(reversed) : Number(reversed);
}
console.log(reverseNumber(12345));  // 54321
console.log(reverseNumber(-987));   // -789
console.log(reverseNumber(1200));   // 21
```

---

**7. Check if a string is a palindrome**
```js
function isPalindrome(str) {
  const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, ''); // remove non-alphanumeric
  const reversed = cleaned.split('').reverse().join('');
  return cleaned === reversed;
}
console.log(isPalindrome('racecar'));         // true
console.log(isPalindrome('A man a plan a canal Panama')); // true
console.log(isPalindrome('hello'));           // false
```

---

**8. Return a string in alphabetical order**
```js
function alphabeticalOrder(str) {
  return str.split('').sort().join('');
}
console.log(alphabeticalOrder('dcba'));    // 'abcd'
console.log(alphabeticalOrder('hello'));   // 'ehllo'
console.log(alphabeticalOrder('javascript')); // 'aacijprstv'
```

---

**9. Convert the first letter of every word to uppercase**
```js
function titleCase(str) {
  return str
    .split(' ')
    .map(word => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
    .join(' ');
}
console.log(titleCase('hello world'));       // 'Hello World'
console.log(titleCase('the quick BROWN fox')); // 'The Quick Brown Fox'
```

---

**10. Count the occurrences of characters in a string**
```js
function countCharacters(str) {
  const count = {};
  for (const char of str) {
    count[char] = (count[char] || 0) + 1;
  }
  return count;
}
console.log(countCharacters('hello'));
// { h: 1, e: 1, l: 2, o: 1 }

console.log(countCharacters('banana'));
// { b: 1, a: 3, n: 2 }
```

---

**11. Loop an array and add all members**
```js
function sumArray(arr) {
  return arr.reduce((total, num) => total + num, 0);
}
console.log(sumArray([1, 2, 3, 4, 5]));    // 15
console.log(sumArray([10, -5, 3]));         // 8
console.log(sumArray([]));                  // 0
```

---

**12. Filter out non-numeric values from an array**
```js
function filterNumeric(arr) {
  return arr.filter(item => typeof item === 'number' && !isNaN(item));
}
console.log(filterNumeric([1, 'a', 2, null, 3, true, NaN, 4]));
// [1, 2, 3, 4]
```

---

**13. Remove specific objects from an array of objects**
```js
const users = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
  { id: 3, name: 'Charlie' },
];

// Remove user with id 2
const filtered = users.filter(user => user.id !== 2);
console.log(filtered);
// [{ id: 1, name: 'Alice' }, { id: 3, name: 'Charlie' }]

// Remove multiple by ids
const idsToRemove = new Set([1, 3]);
const remaining = users.filter(user => !idsToRemove.has(user.id));
console.log(remaining); // [{ id: 2, name: 'Bob' }]
```

---

**14. Clone an array**
```js
const original = [1, [2, 3], { a: 4 }];

// Shallow clone (nested arrays/objects still shared)
const shallow = [...original];

// Deep clone (fully independent)
const deep = structuredClone(original);
// or: JSON.parse(JSON.stringify(original)) — but loses functions/undefined

deep[1].push(99);
console.log(original[1]); // [2, 3] — unchanged with structuredClone
```

---

**15. Return the data type of an argument**
```js
function getType(arg) {
  if (arg === null) return 'null';      // typeof null === 'object' (bug)
  if (Array.isArray(arg)) return 'array'; // typeof [] === 'object'
  return typeof arg;
}
console.log(getType(42));          // 'number'
console.log(getType('hello'));     // 'string'
console.log(getType(true));        // 'boolean'
console.log(getType(null));        // 'null'
console.log(getType([]));          // 'array'
console.log(getType({}));          // 'object'
console.log(getType(undefined));   // 'undefined'
console.log(getType(() => {}));    // 'function'
```

---

**16. Print the first `n` elements of an array**
```js
function firstN(arr, n) {
  return arr.slice(0, n);
}
console.log(firstN([1, 2, 3, 4, 5], 3));   // [1, 2, 3]
console.log(firstN([10, 20, 30], 10));      // [10, 20, 30] — no error if n > length
console.log(firstN([], 3));                 // []
```

---

**17. Print the last `n` elements of an array**
```js
function lastN(arr, n) {
  return arr.slice(-n);
}
console.log(lastN([1, 2, 3, 4, 5], 2));    // [4, 5]
console.log(lastN([10, 20, 30], 10));       // [10, 20, 30]
console.log(lastN([], 3));                  // []
```

---

**18. Shuffle an array**
```js
function shuffleArray(arr) {
  const shuffled = [...arr]; // don't mutate original
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]; // swap
  }
  return shuffled;
}
console.log(shuffleArray([1, 2, 3, 4, 5]));
// e.g. [3, 1, 5, 2, 4] — random each time
```

---

**19. Get the union of two arrays**
```js
function union(arr1, arr2) {
  return [...new Set([...arr1, ...arr2])];
}
console.log(union([1, 2, 3], [2, 3, 4, 5])); // [1, 2, 3, 4, 5]
console.log(union(['a', 'b'], ['b', 'c']));   // ['a', 'b', 'c']
```
