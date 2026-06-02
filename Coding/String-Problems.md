# Coding — String Problems

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

## 3. Count character frequency in a string
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

## 4. Title case a sentence
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

## 5. Reverse each word in a sentence
```js
function reverseEachWord(sentence) {
  return sentence
    .split(' ')
    .map(word => word.split('').reverse().join(''))
    .join(' ');
}
console.log(reverseEachWord('I love coding')); // 'I evol gnidoc'
```
