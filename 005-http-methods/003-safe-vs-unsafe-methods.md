## Safe vs Unsafe Methods

### 1. Start with a very simple question

When a client sends a request to a server, an important question is:

👉 **Is this request just looking at data, or can it change something?**

This single question divides HTTP methods into two groups:

- **Safe methods**
- **Unsafe methods**

Understanding this distinction is critical for:
- REST correctness
- Caching
- Retries
- Security
- Real-world system behavior

---

### 2. What “safe” means in HTTP 

👉 A **safe HTTP method** is:

A method that **must not change anything on the server**.

Calling it:
- once
- twice
- a hundred times

…should never modify data or server state.

Safe = **read-only**

---

### 3. What “unsafe” means in HTTP 

👉 An **unsafe HTTP method** is:

A method that **can change server state**.

It may:
- create data
- update data
- delete data
- trigger side effects

Unsafe = **write or modify**

---

### 4. The most important rule 

👉 **Safe methods do not change data**  
👉 **Unsafe methods may change data**

This rule exists so that:
- browsers behave correctly
- caches work safely
- retries don’t break systems

---

### 5. Which HTTP methods are SAFE?

The commonly used safe methods are:

- **GET**
- **HEAD**
- **OPTIONS**

These methods are designed to be **non-destructive**.

---

### 6. GET — safe method

GET is the most common safe method.

Characteristics:
- Reads data
- Never changes data
- Cacheable
- Retry-safe

Example:
- GET /users
- GET /users/123

Calling GET repeatedly:
- Does not create users
- Does not update users
- Does not delete users

That’s why browsers freely reload pages.

---

### 7. HEAD — safe method 

HEAD is like GET but:
- Returns headers only
- No response body

Used for:
- Checking if a resource exists
- Checking size or metadata

Still safe.
Still read-only.

---

### 8. OPTIONS — safe method

OPTIONS tells the client:
- What methods are allowed
- What CORS rules apply

Example:
- OPTIONS /users

It is informational only.
No data changes.

---

### 9. Which HTTP methods are UNSAFE?

Common unsafe methods:

- **POST**
- **PUT**
- **PATCH**
- **DELETE**

These methods **can change server state**.

---

### 10. POST — unsafe method

POST is unsafe because it:
- Creates new resources
- Triggers side effects

Example:
- POST /users

Calling it twice:
- Creates two users

This is why POST must be handled carefully.

---

### 11. PUT — unsafe method

PUT replaces a resource.

Example:
- PUT /users/123

Calling it:
- Overwrites user data

Even though PUT is **idempotent**, it is still **unsafe** because it modifies data.

---

### 12. PATCH — unsafe method

PATCH partially updates a resource.

Example:
- PATCH /users/123

It:
- Changes server state
- Modifies data

Unsafe by definition.

---

### 13. DELETE — unsafe method

DELETE removes a resource.

Example:
- DELETE /users/123

Calling it:
- Deletes data

Even though DELETE is idempotent, it is unsafe.

---

### 14. Safe vs Idempotent

Many beginners confuse these two.

They are **not the same**.

Safe:
- Does NOT change data

Idempotent:
- Multiple calls have same effect

Examples:
- GET → safe + idempotent
- DELETE → unsafe + idempotent
- POST → unsafe + NOT idempotent

Safety is about **side effects**, not repetition.

---

### 15. Why browsers care about safe methods

Browsers assume safe methods:

- Can be prefetched
- Can be cached
- Can be retried automatically
- Can be triggered by links

That’s why:
- Clicking a link sends GET
- Reloading a page sends GET

Browsers never auto-trigger POST or DELETE.

---

### 16. Why caching depends on safety

Caches assume:

👉 Safe methods won’t change data

So they:
- Cache GET responses
- Never cache POST by default

If GET were unsafe:
- Caches would corrupt data
- Systems would break

---

### 17. Express example: safe vs unsafe routes

Safe route (GET):

    app.get('/users', (req, res) => {
      res.json(users);
    });

Unsafe route (POST):

    app.post('/users', (req, res) => {
      users.push(req.body);
      res.status(201).send();
    });

Same URL.
Different method.
Very different risk level.

---

### 18. Common beginner mistakes 

❌ Using GET to:
- create users
- trigger emails
- delete data

❌ Using GET like:
- GET /deleteUser?id=123

This breaks:
- safety rules
- caching
- browser assumptions

---

### 19. Security implications of unsafe methods

Unsafe methods:
- Must be authenticated
- Must be authorized
- Must be validated

Safe methods:
- Often allow broader access
- Are easier to cache
- Are safer to expose publicly

This distinction shapes API security design.

---

### 20. A simple decision table

GET / HEAD / OPTIONS:
- Read-only
- Safe
- No side effects

POST / PUT / PATCH / DELETE:
- Modify data
- Unsafe
- Require protection

---

### 21. Final definition

**Safe HTTP methods** are methods that do not change server state and can be called repeatedly without side effects, while **unsafe HTTP methods** are methods that can create, modify, or delete data and therefore must be handled with care.

---

### 22. Mental model to remember forever

Think like this:

- Safe methods = looking through a window  
- Unsafe methods = rearranging furniture  

Looking never changes the room.
Moving things does.

HTTP works the same way.
