## URL Anatomy

### 1. Start with the simplest mental model

Every time you visit a website or call an API, you use a **URL**.

You type it.
Your browser sends it.
The server understands it.

But a URL is **not just a random string**.

It has a **clear anatomy**, like parts of a body.

If you understand URL anatomy,  
the web stops feeling magical and starts feeling logical.

---

### 2. What is a URL? 

👉 URL stands for **Uniform Resource Locator**.

In simple words:

A URL is the **address of a resource on the internet**.

Just like a home address tells you:
- where to go
- how to get there

A URL tells computers:
- where the resource is
- how to access it

---

### 3. A full example URL 

Let’s take a real-looking URL:

https://api.example.com:443/users/123/orders?status=active&page=2#summary

This looks scary at first.

Relax  
We’ll break it into **small, simple parts**.

---

### 4. High-level anatomy of a URL

A URL can have these parts (not all are mandatory):

- Scheme  
- Host  
- Port  
- Path  
- Query string  
- Fragment  

Each part has a **specific job**.

---

### 5. Scheme 

👉 The **scheme** tells **how** to talk to the server.

Examples:

- http  
- https  
- ftp  
- ws  
- wss  

In our example:

https://api.example.com/...

Here:
- `https` = use HTTPS protocol

This decides:
- security
- encryption
- rules of communication

---

### 6. Why scheme matters

The scheme affects:

- Security (HTTP vs HTTPS)
- Port defaults
- Browser behavior

Example:

http://example.com  
https://example.com  

Same host.
Very different safety.

---

### 7. Host 

👉 The **host** identifies the server.

Example:

api.example.com

This can be:

- Domain name
- IP address

Examples:

- example.com
- api.example.com
- 192.168.1.10

The host answers the question:

👉 “Which computer should receive this request?”

---

### 8. Subdomains 

In `api.example.com`:

- example.com → main domain
- api → subdomain

Subdomains are often used for:

- api.example.com
- admin.example.com
- auth.example.com

They help organize systems.

---

### 9. Port 

👉 The **port** tells which service on the server to use.

Example:

https://api.example.com:443

Common ports:

- 80 → HTTP
- 443 → HTTPS
- 3000 → Node.js dev servers

Ports are usually hidden because defaults are assumed.

---

### 10. Path 

👉 The **path** identifies the resource on the server.

Example:

/users/123/orders

This part answers:

👉 “What thing do you want?”

Paths usually represent:

- Resources
- Hierarchies
- Relationships

REST APIs live here.

---

### 11. Path segments 

Paths are split by `/`.

Example:

/users/123/orders

Segments:
- users → collection
- 123 → single resource
- orders → nested collection

This is how REST expresses structure.

---

### 12. Query string 

👉 The **query string** starts with `?`.

Example:

?status=active&page=2

Queries are **key=value pairs**.

They are used for:

- Filtering
- Sorting
- Pagination
- Searching

They do NOT identify the resource — they modify the request.

---

### 13. Multiple query parameters

Multiple queries are separated by `&`.

Example:

/users?role=admin&active=true&page=2

This is still the same resource:
- /users

Queries only change **how** it is returned.

---

### 14. Query string vs path

Path:
- Identifies the resource
- Mandatory for resource identity

Query:
- Modifies behavior
- Optional

Example:

/users/123        → specific user  
/users/123?full=true → same user, different representation

---

### 15. Fragment 

👉 The **fragment** starts with `#`.

Example:

#summary

Important rule:

❗ Fragment is **never sent to the server**

It is used by:
- Browsers
- Frontend apps

Examples:
- Page anchors
- SPA routing
- Scroll positions

---

### 16. Complete URL anatomy breakdown

Let’s revisit the full URL:

https://api.example.com:443/users/123/orders?status=active&page=2#summary

Breakdown:

- Scheme → https
- Host → api.example.com
- Port → 443
- Path → /users/123/orders
- Query → status=active&page=2
- Fragment → summary

Now it should feel readable, not scary.

---

### 17. URL anatomy in Express 

Express exposes URL parts cleanly:

    const express = require('express');
    const app = express();

    app.get('/users/:id/orders', (req, res) => {
      console.log(req.protocol); // http / https
      console.log(req.hostname); // host
      console.log(req.path);     // path
      console.log(req.params);  // path params
      console.log(req.query);   // query params

      res.send('URL anatomy demo');
    });

    app.listen(3000);

Frameworks don’t invent this — they expose URL anatomy.

---

### 18. Absolute vs relative URLs

Absolute URL:
- https://example.com/users

Relative URL:
- /users

Browsers resolve relative URLs using the current page’s base URL.

APIs usually work with absolute URLs conceptually.

---

### 19. URL encoding

Some characters are not allowed directly in URLs.

They are encoded.

Example:
- Space → %20
- @ → %40

Example:

/search?q=hello%20world

This is normal and expected.

---

### 20. Common beginner mistakes

- Putting actions in paths (/getUser)
- Using query params for identity
- Ignoring encoding
- Overloading path and query responsibilities
- Forgetting fragment is client-only

Understanding anatomy prevents these mistakes.

---

### 21. Why URL anatomy matters so much

If you understand URLs:

- REST design becomes obvious
- Debugging APIs is easier
- Security decisions make sense
- Frontend–backend coordination improves

URLs are the **spine of the web**.

---

### 22. Final definition 

A **URL** is a structured address that identifies a resource on the internet and specifies how to access it, composed of parts like scheme, host, port, path, query string, and fragment—each with a distinct responsibility.

---

### 23. Mental model to remember forever

Think of a URL like a full postal address:

- Scheme → delivery method  
- Host → city & building  
- Port → apartment entrance  
- Path → room inside  
- Query → delivery instructions  
- Fragment → note for the receiver  

Once you see URLs this way,  
they stop being confusing forever.
