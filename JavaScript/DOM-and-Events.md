# JavaScript — DOM & Events

---

## DOM

---

**Q60: How do you select, modify, or remove DOM elements?**

```js
// Select
document.getElementById('myId');
document.querySelector('.myClass');        // first match
document.querySelectorAll('p');            // all matches (NodeList)
document.getElementsByClassName('active'); // HTMLCollection

// Modify
const el = document.getElementById('title');
el.textContent = 'New Title';
el.innerHTML = '<strong>Bold</strong>';
el.setAttribute('class', 'active');
el.style.color = 'red';

// Remove
el.remove();                          // remove element
el.parentNode.removeChild(el);        // old way
```

---

**Q62: Difference between `getElementById`, `getElementsByClassName`, and `getElementsByTagName`?**

| Method | Returns | Live? |
|---|---|---|
| `getElementById` | Single Element or `null` | No |
| `getElementsByClassName` | HTMLCollection | Yes (live) |
| `getElementsByTagName` | HTMLCollection | Yes (live) |
| `querySelector` | First matching Element | No |
| `querySelectorAll` | Static NodeList | No |

A **live** collection updates automatically when the DOM changes.

---

**Q63: What is the difference between `querySelector()` and `querySelectorAll()`?**

```js
// querySelector — returns FIRST matching element (or null)
const first = document.querySelector('.card'); // one element

// querySelectorAll — returns ALL matching elements as a static NodeList
const all = document.querySelectorAll('.card'); // NodeList
all.forEach(card => card.classList.add('active'));
```

---

**Q64: Methods to manipulate elements, properties and attributes of JS DOM**

```js
const el = document.querySelector('#box');

// Attributes
el.getAttribute('href');
el.setAttribute('href', 'https://example.com');
el.removeAttribute('disabled');
el.hasAttribute('class');

// Properties
el.id;
el.className;
el.classList.add('active');
el.classList.remove('hidden');
el.classList.toggle('open');
el.classList.contains('active'); // true/false

// Content
el.textContent = 'text only';
el.innerHTML = '<span>html content</span>';

// Style
el.style.backgroundColor = 'blue';
el.style.display = 'none';
```

---

**Q65: What is the difference between `innerHTML` and `textContent`?**

| `innerHTML` | `textContent` |
|---|---|
| Parses and renders HTML | Plain text only — no HTML parsing |
| Risk of XSS injection | Safe — escapes HTML |
| Slower (HTML parsing) | Faster |

```js
el.innerHTML = '<b>Bold</b>';    // renders as bold text
el.textContent = '<b>Bold</b>';  // shows literal string "<b>Bold</b>"

// Always use textContent when inserting user-provided text!
el.textContent = userInput; // safe from XSS
el.innerHTML = userInput;   // DANGEROUS if userInput contains scripts
```

---

**Q68: How to create new elements in DOM? Difference between `createElement()` and `cloneNode()`?**

```js
// createElement — creates a brand new element
const div = document.createElement('div');
div.textContent = 'New element';
div.classList.add('card');
document.body.appendChild(div);

// cloneNode — copies an existing element
const original = document.querySelector('.card');
const clone = original.cloneNode(true); // true = deep clone (with children)
const shallowClone = original.cloneNode(false); // false = element only, no children
document.body.appendChild(clone);
```

| `createElement()` | `cloneNode()` |
|---|---|
| Creates fresh element | Copies existing element |
| No event listeners | Copies structure, NOT event listeners |
| More control | Faster for complex structures |

---

## Events in JS

---

**Q85: Events in JS**

An event is something that **happens in the browser** — user interaction or browser action.

```js
// addEventListener (preferred)
element.addEventListener('click', handler);
element.addEventListener('keydown', (e) => console.log(e.key));
element.addEventListener('submit', (e) => { e.preventDefault(); });

// Common events
// click, dblclick, mouseover, mouseout, mouseenter, mouseleave
// keydown, keyup, keypress
// submit, change, input, focus, blur
// load, DOMContentLoaded, resize, scroll
```

---

**Q86: Event Delegation in JS**

Instead of adding listeners to each child, add **one listener to the parent** and use `event.target` to identify which child was clicked.

```js
// Bad — a listener for each item
document.querySelectorAll('li').forEach(li => {
  li.addEventListener('click', handler);
});

// Good — one listener on the parent (event delegation)
document.querySelector('ul').addEventListener('click', (e) => {
  if (e.target.tagName === 'LI') {
    console.log('Clicked:', e.target.textContent);
  }
});
```

**Benefits:** Works for dynamically added elements, fewer event listeners, better performance.

---

**Q87: Event Bubbling and Event Capturing in JS**

Events travel in 3 phases:
1. **Capturing** — from root → target (top-down).
2. **Target** — at the element.
3. **Bubbling** — from target → root (bottom-up).

By default, handlers use **bubbling**.

```js
// Bubbling (default — false/omitted)
parent.addEventListener('click', () => console.log('Parent'), false);
child.addEventListener('click', () => console.log('Child'), false);
// Click child → logs: 'Child', then 'Parent'

// Capturing (true)
parent.addEventListener('click', () => console.log('Parent'), true);
// Click child → logs: 'Parent', then 'Child'

// Stop propagation
child.addEventListener('click', (e) => {
  e.stopPropagation(); // prevents bubbling up
});
```

---

**Q88: `event.preventDefault()` method in JS**

Prevents the **default browser behavior** for an event (form submission, link navigation, etc.).

```js
// Prevent form from reloading page
form.addEventListener('submit', (e) => {
  e.preventDefault();
  // handle form data manually
});

// Prevent link navigation
link.addEventListener('click', (e) => {
  e.preventDefault();
  // do custom routing instead
});

// Prevent right-click context menu
document.addEventListener('contextmenu', (e) => e.preventDefault());
```

---

**Q: The use of `this` keyword in the context of event handling**

In a regular function event handler, `this` refers to the element the listener is attached to.

```js
button.addEventListener('click', function() {
  console.log(this); // the button element
  this.classList.toggle('active');
});

// Arrow function — 'this' is the enclosing context, NOT the element
button.addEventListener('click', () => {
  console.log(this); // window (or undefined in strict mode)
});
```

Use regular functions when you need `this` to refer to the element.

---

**Q89: How to remove (unattach) an event handler from an element?**

```js
// Must reference the same function — anonymous functions cannot be removed
function handleClick() {
  console.log('clicked');
}

element.addEventListener('click', handleClick);
element.removeEventListener('click', handleClick); // removes it

// AbortController — modern way to remove multiple listeners
const controller = new AbortController();
element.addEventListener('click', handler, { signal: controller.signal });
element.addEventListener('scroll', handler, { signal: controller.signal });
controller.abort(); // removes all listeners added with this signal
```
