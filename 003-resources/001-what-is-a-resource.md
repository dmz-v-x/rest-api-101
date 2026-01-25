## What is a Resource

### 1. Start with the simplest human idea

Imagine a library.

Inside it, you have:

- Books  
- Authors  
- Shelves  
- Members  

Each of these is **something you can talk about, identify, and act on**.

In REST and HTTP, these “things” are called **resources**.

---

### 2. What is a resource? 

👉 A **resource** is:

Any meaningful piece of data or concept that a server exposes and allows clients to interact with.

That’s it.

If you can:

- Name it  
- Identify it  
- Read it  
- Change it  

It’s a resource.

---

### 3. Resource ≠ database row

Common beginner mistake ❌

A resource is **not** necessarily:

- A database table  
- A database row  

A resource is a **representation**, not storage.

Examples of resources:

- A user profile  
- A list of orders  
- A generated report  
- A search result  

They may come from databases — but they are not the database.

---

### 4. Resources are nouns, not verbs

Key REST rule:

👉 Resources are **nouns**.

Examples:

- users  
- orders  
- products  
- invoices  

❌ Bad (verbs):
- getUser  
- createOrder  
- deleteProduct  

✅ Good (nouns):
- /users  
- /orders  
- /products  

Actions are expressed using **HTTP methods**, not URLs.

---

### 5. How resources are identified

Each resource is identified by a **URL**.

Examples:

- /users → collection of users  
- /users/123 → single user  
- /orders/456/items → sub-resource  

URL = **address of a resource**, not an action.

---

### 6. Collections vs single resources

Important distinction:

**Collection resource:**
- /users  
- Represents many users  

**Single resource:**
- /users/123  
- Represents one specific user  

Both are resources.

---

### 7. Sub-resources 

Resources can be nested:

- /users/123/orders  
- /orders/456/items  

This expresses relationships.

Think:

User → has → Orders  
Order → has → Items  

REST models real-world structure.

---

### 8. Resource representations 

A resource is **not the data itself**.

It is a **representation of data**.

Example representations:

- JSON  
- XML  
- HTML  

Same resource, different representations.

Example:

    GET /users/123
    Accept: application/json

vs

    GET /users/123
    Accept: text/html

Same resource.  
Different format.

---

### 9. Resources are manipulated via HTTP methods

Actions are performed using HTTP methods:

- GET /users/123 → read user  
- POST /users → create user  
- PUT /users/123 → replace user  
- PATCH /users/123 → update user  
- DELETE /users/123 → delete user  

URL stays the same.  
Method changes behavior.

---

### 10. Resource lifecycle

Most resources follow a lifecycle:

1. Created (POST)  
2. Read (GET)  
3. Updated (PUT/PATCH)  
4. Deleted (DELETE)  

This is often called **CRUD**.

---

### 11. Express example: resource-based API

Example REST API using Express:

    const express = require('express');
    const app = express();

    app.use(express.json());

    app.get('/users/:id', (req, res) => {
      res.json({ id: req.params.id, name: 'Alex' });
    });

    app.post('/users', (req, res) => {
      res.status(201).json({ message: 'User created' });
    });

    app.delete('/users/:id', (req, res) => {
      res.status(204).send();
    });

    app.listen(3000);

Notice:

- URLs represent resources  
- Methods represent actions  

That’s REST thinking.

---

### 12. Resources are stateless

Important REST rule:

Each interaction with a resource is stateless.

Server does not remember:

- Previous reads  
- Previous updates  

Each request must contain all required information.

---

### 13. Derived and virtual resources

Not all resources map directly to tables.

Examples:

- /reports/daily-sales  
- /users/123/summary  
- /search?query=phone  

These are **computed resources**.

Still resources.  
Still valid.

---

### 14. Resources vs endpoints

Beginners often mix these up.

- Resource = concept / thing  
- Endpoint = specific URL + method  

Example:

Resource: user 123  
Endpoints:
- GET /users/123  
- PATCH /users/123  
- DELETE /users/123  

Same resource.  
Different endpoints.

---

### 15. Designing good resources

Good resources are:

- Stable  
- Meaningful  
- Predictable  
- Noun-based  
- Hierarchical when needed  

Bad resource design causes pain later.

---

### 16. Resources enable decoupling

Because clients interact with resources:

- Backend implementation can change  
- Database can change  
- Logic can change  

Clients don’t care.

They only know resources.

---

### 17. Resources + HTTP = REST power

REST works because:

- HTTP provides methods  
- Resources provide structure  

Together they create:

- Predictable APIs  
- Scalable systems  
- Easy-to-use interfaces  

---

### 18. Common beginner mistakes

- Using verbs in URLs  
- Mixing actions into resource names  
- Exposing database internals  
- Over-nesting resources  

These break REST principles.

---

### 19. Why understanding resources is critical

If you understand resources:

- REST design becomes obvious  
- URLs stop feeling random  
- APIs become intuitive  
- Clients become simpler  

This is a **foundational REST concept**.

---

### 20. Final definition

A **resource** is a uniquely identifiable, meaningful piece of data or functionality exposed by a server, which clients interact with using standard HTTP methods.

---

### 21. Mental model to remember forever

Think:

- Resource = thing  
- URL = address of the thing  
- HTTP method = what you want to do to the thing  

If you remember this, REST will never confuse you again.
