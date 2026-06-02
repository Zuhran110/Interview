# React — Forms & Controlled Components

---

## Controlled & Uncontrolled Components — Part 13

---

**Q78. What are Controlled Components in React?**

A **controlled component** is one where React **controls the form element's value** through state. The input value is always driven by `state`, and every change goes through `setState`.

```jsx
const [name, setName] = useState('');

<input
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

**Single source of truth** — React state is always in sync with the input.

---

**Q79. What are the differences between Controlled and Uncontrolled Components?**

| Controlled | Uncontrolled |
|---|---|
| Value managed by React state | Value managed by the DOM |
| Use `value` + `onChange` | Use `ref` to read value |
| Instant access to input value | Read value only on submit |
| More code, more control | Less code, simpler for basic forms |
| Good for validation | Good for file inputs, simple forms |

---

**Q80. What are the characteristics of Controlled Components?**

1. Input value is **tied to state** via `value` prop.
2. Changes are handled via **onChange** event.
3. React is the **single source of truth**.
4. Easy to perform **real-time validation**.
5. Easy to **reset or pre-fill** form data.

---

**Q81. What are the advantages of Controlled Components in React forms?**

1. **Instant validation** — validate on every keystroke.
2. **Conditional disabling** — disable submit until form is valid.
3. **Programmatic control** — easily reset or pre-populate forms.
4. **Consistency** — state always matches the displayed value.
5. **Easy testing** — just test state changes.

---

**Q82. How to handle forms in React?**

```jsx
const [formData, setFormData] = useState({ email: '', password: '' });

const handleSubmit = (e) => {
  e.preventDefault(); // prevent page reload
  console.log(formData);
};

<form onSubmit={handleSubmit}>
  <input value={formData.email} onChange={e => setFormData({...formData, email: e.target.value})} />
  <button type="submit">Submit</button>
</form>
```

---

**Q83. How can you handle multiple input fields in a controlled form?**

Use a single state object and the input's `name` attribute to dynamically update the right field:

```jsx
const [form, setForm] = useState({ name: '', email: '' });

const handleChange = (e) => {
  setForm({ ...form, [e.target.name]: e.target.value });
};

<input name="name" value={form.name} onChange={handleChange} />
<input name="email" value={form.email} onChange={handleChange} />
```

---

**Q84. How do you handle form validation in a controlled component?**

```jsx
const [email, setEmail] = useState('');
const [error, setError] = useState('');

const validate = (value) => {
  if (!value.includes('@')) setError('Invalid email');
  else setError('');
};

<input
  value={email}
  onChange={(e) => { setEmail(e.target.value); validate(e.target.value); }}
/>
{error && <span style={{ color: 'red' }}>{error}</span>}
```

For complex forms, use libraries like **React Hook Form** or **Formik**.

---

**Q85. When might using Uncontrolled Components be advantageous?**

1. **File inputs** — `<input type="file">` is always uncontrolled; you access it via ref.
2. **Integrating with non-React code** — third-party DOM libraries.
3. **Simple forms with no validation** — just need the value on submit.
4. **Performance** — avoids re-renders on every keystroke.

```jsx
const inputRef = useRef();
const handleSubmit = () => console.log(inputRef.current.value);

<input ref={inputRef} />
```

---
