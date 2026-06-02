# MERN — Web Architecture & Development Basics

---

## Web Architecture & Development Basics

### GraphQL vs REST
**Q: Why is GraphQL often considered more efficient for specific data-fetching scenarios?**

- **REST** returns fixed data shapes — you may get too much (over-fetching) or too little (under-fetching) data, requiring multiple requests.
- **GraphQL** lets the client specify exactly what fields it needs in a single request.

```graphql
# GraphQL: fetch only name and email in one request
query {
  user(id: "1") {
    name
    email
  }
}
```

GraphQL is ideal for complex UIs, mobile clients (bandwidth-sensitive), or apps with many related resources.

---

### CSS Box Model
**Q: Explain the components of the CSS Box Model.**

Every HTML element is a rectangular box made of four layers (inside out):

1. **Content** — the actual text/image area.
2. **Padding** — space between content and the border (inside the element).
3. **Border** — the edge around the padding.
4. **Margin** — space outside the border, between elements.

```css
.box {
  width: 200px;       /* content width */
  padding: 20px;      /* inner spacing */
  border: 2px solid;  /* border */
  margin: 10px;       /* outer spacing */
  box-sizing: border-box; /* padding+border included in width */
}
```

---

### Content Delivery Networks (CDN)
**Q: What are the advantages of using a CDN?**

A CDN is a distributed network of servers that cache and deliver content from locations geographically close to the user.

**Benefits:**
- **Faster load times** — content served from nearest edge server.
- **Reduced origin server load** — CDN handles static assets (images, JS, CSS).
- **High availability** — redundancy across multiple servers.
- **DDoS protection** — traffic is distributed, absorbing attacks.
- **Global reach** — consistent performance worldwide.

---

### Lazy Loading
**Q: How does lazy loading optimize page performance?**

Lazy loading defers loading of non-critical resources (images, components) until they are needed (e.g., when entering the viewport).

**Benefits:**
- Faster initial page load.
- Reduced bandwidth usage.
- Lower memory consumption.

```html
<!-- Native HTML lazy loading -->
<img src="photo.jpg" loading="lazy" alt="..." />
```

```js
// React lazy loading
const LazyComponent = React.lazy(() => import('./HeavyComponent'));
```

---

### Bot / Scraping Prevention
**Q: What strategies can prevent bots from scraping your API?**

1. **Rate Limiting** — restrict requests per IP/token per time window.
2. **CAPTCHA** — challenge suspicious traffic (reCAPTCHA, hCaptcha).
3. **API Keys / Authentication** — require tokens for access.
4. **IP Allowlisting / Blocklisting** — block known bad actors.
5. **Honeypot fields** — hidden form fields that only bots fill.
6. **Request Header Validation** — check `User-Agent`, `Referer`, and other headers.
7. **Behavioral Analysis** — detect non-human patterns (click speed, mouse movement).
8. **CORS policies** — restrict which origins can make requests.

---
