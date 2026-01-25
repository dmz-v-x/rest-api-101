## Collection vs Single Resource

### 1. Start with a very simple real-world idea

Imagine a school.

You can talk about:

- All students in the school  
- One specific student  

Both are valid — but they are **not the same thing**.

REST APIs work exactly the same way.

This is where **collection resources** and **single resources** come in.

---

### 2. What is a collection resource?

👉 A **collection resource** represents:

A group of similar resources.

Examples:

- All users  
- All orders  
- All products  

In URLs, collections usually look like:

- /users  
- /orders  
- /products  

They represent **many items**, not one.

---

### 3. What is a single resource? 

👉 A **single resource** represents:

One specific item inside a collection.

Examples:

- One user  
- One order  
- One product  

In URLs, single resources usually look like:

- /users/123  
- /orders/456  
- /products/789  

The ID uniquely identifies the resource.

---

### 4. Collection vs single resource

The difference is **scope**:

- Collection → many resources  
- Single → exactly one resource  

They represent different concepts and allow different actions.

---

### 5. HTTP methods behave differently on each

Same HTTP method can mean different things depending on **collection vs single**.

Example with `/users`:

- GET /users  
  → get list of users  

- POST /users  
  → create a new user  

Example with `/users/123`:

- GET /users/123  
  → get user 123  

- PATCH /users/123  
  → update user 123  

- DELETE /users/123  
  → delete user 123  

This is intentional REST design.

---

### 6. Why POST usually belongs to collections

Important REST rule:

👉 You **POST to a collection**, not to a single resource.

Why?

Because:

- The server decides the ID
- The resource does not exist yet

Example:

POST /users  
→ create a new user  
→ server returns `/users/124`

Posting to `/users/123` usually makes no sense.

---

### 7. Why GET works for both

GET behaves differently:

- GET /users  
  → returns a list  

- GET /users/123  
  → returns one item  

Same method.  
Different scope.

This makes APIs predictable.

---

### 8. Express example: collection vs single routes

Example Express server:

    const express = require('express');
    const app = express();

    app.use(express.json());

    // Collection resource
    app.get('/users', (req, res) => {
      res.json([{ id: 1, name: 'Alex' }]);
    });

    app.post('/users', (req, res) => {
      res.status(201).json({ id: 2, name: req.body.name });
    });

    // Single resource
    app.get('/users/:id', (req, res) => {
      res.json({ id: req.params.id, name: 'Alex' });
    });

    app.patch('/users/:id', (req, res) => {
      res.json({ message: 'User updated' });
    });

    app.delete('/users/:id', (req, res) => {
      res.status(204).send();
    });

    app.listen(3000);

Notice:

- Same base resource (`users`)
- Different behavior based on URL shape

---

### 9. Collection filtering and pagination

Collections often support **query parameters**.

Examples:

- /users?page=2  
- /orders?status=completed  
- /products?category=phones  

This still operates on the **collection**, not a single resource.

---

### 10. Single resource is always uniquely identifiable

A single resource:

- Has a unique ID  
- Represents one logical entity  
- Has its own lifecycle  

Example:

- User 123 exists
- Can be updated
- Can be deleted

Collections don’t have identities like this.

---

### 11. Sub-collections 

Collections can exist **inside** single resources.

Examples:

- /users/123/orders  
- /orders/456/items  

Here:

- User 123 → single resource  
- Orders → collection belonging to that user  

This models real-world relationships cleanly.

---

### 12. When not to over-nest collections

Beginner mistake ❌:

- /users/123/orders/456/items/789/details

Too deep = hard to maintain.

Rule of thumb:

- Nest only when it adds meaning  
- Avoid deep hierarchies  

Flat is often better.

---

### 13. Status codes differ by context

Collection actions:

- POST /users → 201 Created  

Single resource actions:

- DELETE /users/123 → 204 No Content  
- GET /users/999 → 404 Not Found  

Status codes reinforce the difference.

---

### 14. REST design clarity comes from this distinction

When you respect collection vs single resources:

- APIs become intuitive  
- Documentation becomes simpler  
- Clients guess less  
- Bugs reduce  

This distinction is foundational.

---

### 15. Common beginner mistakes

- POSTing to single resources  
- Using verbs instead of resource URLs  
- Returning lists from single resource endpoints  
- Treating collections and singles the same  

All cause confusion.

---

### 16. Mental checklist to identify which one you need

Ask:

- Am I dealing with many things? → collection  
- Am I dealing with one specific thing? → single resource  
- Am I creating something new? → POST to collection  
- Am I modifying something existing? → single resource  

This covers 90% of cases.

---

### 17. Why this matters at scale

At scale:

- Clients rely on predictable behavior  
- Teams rely on conventions  
- APIs must remain stable  

Collection vs single resource distinction provides that stability.

---

### 18. Final definition 

A **collection resource** represents a group of similar resources (like `/users`), while a **single resource** represents one specific item within that collection (like `/users/123`), and REST APIs use this distinction to define clear, predictable behaviors.

---

### 19. Mental model to remember forever

Think like this:

- Collection = folder  
- Single resource = file  

You list folders.  
You open files.

Once this clicks, REST URLs will always make sense.
