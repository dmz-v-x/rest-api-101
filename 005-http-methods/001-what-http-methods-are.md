## What http methods are

### 1. Start with the most basic idea

When a client sends an HTTP request, the server always asks one question first:

👉 **What do you want to do?**

That “what you want to do” is expressed using an **HTTP method**.

Without HTTP methods, the server would not know:
- whether to read data
- create data
- update data
- or delete data

---

### 2. What are HTTP methods?

👉 **HTTP methods** are:

Standardized action words that tell the server **what operation the client wants to perform on a resource**.

They are sometimes called:
- HTTP verbs

Because they represent **actions**.

---

### 3. Why HTTP methods exist at all

Imagine a world without HTTP methods.

Every request would look the same:
- same URL
- same structure

The server wouldn’t know:
- Is this a read?
- Is this a create?
- Is this a delete?

HTTP methods solve this by clearly separating **intent** from **data**.

---

### 4. The core REST rule 

👉 **URLs identify resources (nouns)**  
👉 **HTTP methods represent actions (verbs)**  

This separation is the foundation of REST.

Example:

- GET /users  
- POST /users  

Same resource.
Different intent.

---

### 5. The most important HTTP methods 

You will use these 5 in almost every REST API:

- GET  
- POST  
- PUT  
- PATCH  
- DELETE  

If you master these, you understand 90% of REST.

---

### 6. GET – read a resource

👉 **GET** is used to **retrieve data**.

Characteristics:
- Does not change server data
- Safe
- Idempotent

Examples:
- GET /users  
- GET /users/123  

Meaning:
- “Give me users”
- “Give me user 123”

---

### 7. Express example: GET

    app.get('/users', (req, res) => {
      res.json([{ id: 1, name: 'Alex' }]);
    });

GET is used only for reading.
Never for creating or updating.

---

### 8. POST – create a new resource

👉 **POST** is used to **create something new**.

Characteristics:
- Changes server state
- Not idempotent
- Usually sent to a collection

Example:
- POST /users  

Meaning:
- “Create a new user”

---

### 9. Express example: POST

    app.post('/users', (req, res) => {
      res.status(201).json({ id: 2 });
    });

POST is usually used when:
- Server generates the ID
- Resource does not exist yet

---

### 10. PUT – replace a resource

👉 **PUT** means:

“Replace the entire resource with this new version.”

Characteristics:
- Idempotent
- Full replacement

Example:
- PUT /users/123  

Meaning:
- “Replace user 123 completely”

---

### 11. PUT example 

    app.put('/users/:id', (req, res) => {
      res.json({ message: 'User replaced' });
    });

If a field is missing in PUT:
👉 It is assumed to be removed.

This is why PUT must be used carefully.

---

### 12. PATCH – partially update a resource

👉 **PATCH** means:

“Update only the fields I provide.”

Characteristics:
- Partial update
- Not full replacement
- Commonly used in modern APIs

Example:
- PATCH /users/123  

Meaning:
- “Update only some fields of user 123”

---

### 13. PATCH vs PUT 

PUT:
- Replaces entire resource
- Missing fields are removed

PATCH:
- Updates only provided fields
- Missing fields are untouched

Most APIs prefer PATCH for updates.

---

### 14. Express example: PATCH

    app.patch('/users/:id', (req, res) => {
      res.json({ message: 'User updated' });
    });

PATCH maps perfectly to real-world updates.

---

### 15. DELETE – remove a resource

👉 **DELETE** is used to **remove a resource**.

Characteristics:
- Idempotent
- Changes server state

Example:
- DELETE /users/123  

Meaning:
- “Delete user 123”

---

### 16. Express example: DELETE

    app.delete('/users/:id', (req, res) => {
      res.status(204).send();
    });

DELETE usually returns:
- 204 No Content

Because there is nothing left to return.

---

### 17. Idempotency 

Idempotent means:

👉 Calling the same request multiple times has the same effect.

Idempotent methods:
- GET
- PUT
- DELETE

Not idempotent:
- POST

This matters for:
- retries
- network failures
- safety

---

### 18. Safe vs unsafe methods

Safe methods:
- GET (does not change data)

Unsafe methods:
- POST
- PUT
- PATCH
- DELETE

Browsers and caches treat them differently.

---

### 19. Other HTTP methods 

There are more methods, but less common:

- HEAD → like GET but no body
- OPTIONS → discover allowed methods
- TRACE → debugging
- CONNECT → tunneling (HTTPS)

As a beginner, focus on the core five.

---

### 20. HTTP methods + status codes 

Methods and status codes work together:

- GET → 200 OK
- POST → 201 Created
- PATCH → 200 OK
- DELETE → 204 No Content
- Invalid method → 405 Method Not Allowed

Correct pairing makes APIs predictable.

---

### 21. Common beginner mistakes

- Using GET to create data
- Using POST for everything
- Ignoring PUT vs PATCH difference
- Encoding actions in URLs instead of methods

These break REST principles.

---

### 22. Why HTTP methods matter in real systems

Correct use of methods enables:
- Caching
- Security rules
- API gateways
- Load balancers
- Observability

HTTP infrastructure relies on method semantics.

---

### 23. Final definition 

**HTTP methods** are standardized verbs that define the action a client wants to perform on a resource, such as reading, creating, updating, or deleting data, and they are a fundamental part of RESTful API design.

---

### 24. Mental model to remember forever

Think like this:

- URL = thing (noun)
- HTTP method = action (verb)

You don’t say:
- /createUser

You say:
- POST /users

Once this clicks,
HTTP methods will feel obvious and natural forever.
