## Singular Noun vs Plural Noun

### 1. Start with the beginner confusion

One of the most common REST questions is:

- “Should my endpoint be `/user` or `/users`?”
- “Why do people always use plural nouns?”
- “Does it really matter?”

Yes.  
It matters **a lot** — not for the server, but for **humans and consistency**.

---

### 2. What plural vs singular even means in URLs

Let’s look at two styles:

Singular style:
- /user
- /user/123

Plural style:
- /users
- /users/123

Both *work technically*.

But REST strongly prefers **plural nouns**.

---

### 3. What a resource actually represents

A resource is a **thing**.

In REST, URLs usually represent:

- A collection of things  
- A single thing inside that collection  

Plural nouns map naturally to this idea.

---

### 4. Why collections are plural by nature

A collection always represents **many items**.

Examples:

- users → many users
- orders → many orders
- products → many products

So:

- /users → collection  
- /users/123 → one user inside that collection  

This reads like normal language.

---

### 5. Why singular nouns feel awkward

Consider this:

- GET /user → what does this mean?
  - Which user?
  - The current user?
  - All users?

Singular nouns introduce **ambiguity**.

Plural nouns remove it.

---

### 6. Plural nouns make HTTP methods intuitive

Look at this combination:

- GET /users → get all users  
- POST /users → create a user  
- GET /users/123 → get user 123  
- DELETE /users/123 → delete user 123  

This mapping feels obvious and predictable.

With singular nouns, it gets messy.

---

### 7. What goes wrong with singular-based APIs

Example using singular nouns ❌:

- POST /user
- GET /user/123
- DELETE /user/123

Questions arise:

- Why POST to `/user`?
- Is `/user` one user or many?
- Is this RPC-style?

The mental model breaks down.

---

### 8. REST convention vs rule 

Important clarification:

👉 Using plural nouns is a **convention**, not a hard rule.

REST won’t break if you use singular.

But:

- Conventions exist to reduce thinking
- Consistency beats cleverness

Almost all major APIs use plurals.

---

### 9. Express example: plural resources

Recommended Express routes:

    const express = require('express');
    const app = express();

    app.get('/users', (req, res) => {
      res.json([{ id: 1, name: 'Alex' }]);
    });

    app.post('/users', (req, res) => {
      res.status(201).json({ id: 2 });
    });

    app.get('/users/:id', (req, res) => {
      res.json({ id: req.params.id });
    });

    app.delete('/users/:id', (req, res) => {
      res.status(204).send();
    });

This reads naturally.

---

### 10. Plural nouns reinforce resource hierarchy

Plural nouns also work better with nesting:

- /users/123/orders
- /orders/456/items

Using singular nouns here feels wrong:

- /user/123/order

Plural reflects reality: many orders.

---

### 11. But what about “special” resources?

Some resources are naturally singular:

- /health
- /status
- /metrics

These represent **system state**, not collections.

Rule of thumb:

👉 If it’s a collection → plural  
👉 If it’s a singleton concept → singular  

---

### 12. What about “current user”?

Common case:

“How do I represent the logged-in user?”

❌ Bad:
- /user

Better options:

- /users/me
- /profile
- /account

Avoid bending plural rules.

---

### 13. Consistency beats correctness

The biggest reason for plural nouns:

👉 Consistency across the API.

Mixing styles is worse than choosing either.

❌ Bad:
- /users
- /order
- /products

✅ Good:
- /users
- /orders
- /products

Clients rely on predictability.

---

### 14. Industry practice

Look at major APIs:

- GitHub → /users
- Stripe → /customers
- Google → /projects
- AWS → /instances

Plural nouns everywhere.

This is not accidental.

---

### 15. Common beginner mistakes

- Mixing singular and plural randomly
- Using singular for collections
- Overthinking grammar
- Using verbs instead of nouns

These mistakes confuse clients.

---

### 16. Does plural affect databases? 

No.

- API design ≠ database design
- API nouns are conceptual
- DB tables can be anything

Plural URLs don’t force plural tables.

---

### 17. Why this matters at scale

At scale:

- APIs live for years
- Many teams consume them
- Documentation must be simple

Plural nouns reduce cognitive load.

Less thinking → fewer bugs.

---

### 18. A simple decision guide

When choosing a resource name, ask:

- Does this represent many items? → plural  
- Does this represent one global thing? → singular  
- Will this be nested? → plural fits better  

This covers most cases.

---

### 19. Final definition

In REST APIs, plural nouns are preferred for resource names because they naturally represent collections, eliminate ambiguity, and create predictable, consistent URL structures when combined with HTTP methods.

---

### 20. Mental model to remember forever

Think like this:

- Folder names are plural  
- Files are singular  

You don’t open `/folder/file`.
You open `/folders/123`.

REST URLs work the same way.


