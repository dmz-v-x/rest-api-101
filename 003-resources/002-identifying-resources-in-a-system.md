## Identifying resources in a system

### 1. Start with the real beginner problem

When people start building APIs, they usually ask:

- “What endpoints should I create?”
- “What routes do I need?”
- “What URLs should exist?”

But this is the **wrong starting point**.

The correct first question is:

👉 **What are the resources in my system?**

If you identify resources correctly,  
the API design almost designs itself.

---

### 2. What does “identifying resources” mean?

👉 Identifying resources means:

Figuring out **what things in your system are important enough** to be exposed and interacted with.

These “things” become:

- URLs  
- REST endpoints  
- API contracts  

Bad resource identification → bad API design.

---

### 3. Think in terms of the real world 

Forget databases.  
Forget tables.  
Forget frameworks.

Start with the **real-world domain**.

Ask:

- What things exist in this system?
- What things does the user care about?
- What things can be created, viewed, updated, or deleted?

Those things are candidates for resources.

---

### 4. Example system: an e-commerce app

Let’s imagine a simple e-commerce system.

Possible real-world concepts:

- User  
- Product  
- Order  
- Cart  
- Payment  
- Review  

Each of these is a **noun**.

Each is a strong candidate for a resource.

---

### 5. Resources are nouns, not actions 

Very important REST rule:

👉 Resources are **nouns**, never verbs.

❌ Bad:
- /createUser
- /getOrders
- /payNow

✅ Good:
- /users
- /orders
- /payments

Actions are expressed using **HTTP methods**, not names.

---

### 6. Ask the CRUD question 

For each noun, ask:

- Can it be created?
- Can it be read?
- Can it be updated?
- Can it be deleted?

If yes → it’s almost certainly a resource.

Example:

User:
- Create user ✔
- Read user ✔
- Update user ✔
- Delete user ✔

→ User is a resource.

---

### 7. Identify collections vs single resources

Once you find a resource, split it into two views:

**Collection resource**
- Represents many items
- Example: /users

**Single resource**
- Represents one item
- Example: /users/123

Both are resources.
They just represent different scopes.

---

### 8. Identifying sub-resources 

Resources often relate to each other.

Example:

- A user has orders
- An order has items
- A product has reviews

These become **sub-resources**:

- /users/123/orders
- /orders/456/items
- /products/789/reviews

Rule of thumb:

👉 If one thing *belongs to* another, it may be a sub-resource.

---

### 9. Don’t expose database structure 

Beginners often do this:

- /user_table
- /order_row
- /join_user_order

❌ This leaks internals.

Resources represent **business concepts**, not database design.

You can change your DB.
Your API should not change.

---

### 10. Derived and computed resources

Not all resources are stored directly.

Some are **computed**.

Examples:

- /reports/monthly-sales
- /users/123/summary
- /search?query=phone

These are still valid resources.

Rule:

👉 If it represents a meaningful result → it can be a resource.

---

### 11. Identifying resources by user actions 

Another powerful trick:

List what users want to do.

Example:

- “User wants to see their orders”
- “Admin wants sales report”
- “User wants to review product”

Now remove verbs:

- Orders
- Sales report
- Review

Those nouns → resources.

---

### 12. Express example: resources mapped to routes

Example REST API structure:

    GET    /users
    GET    /users/:id
    POST   /users
    PATCH  /users/:id
    DELETE /users/:id

    GET    /users/:id/orders
    POST   /orders
    GET    /orders/:id

Each route maps cleanly to a resource.

No verbs.
No confusion.

---

### 13. Identifying resources in complex systems

In large systems, you may have:

- Core resources (users, orders)
- Supporting resources (addresses, preferences)
- Process resources (sessions, uploads)

Example:

- /sessions (login sessions)
- /uploads
- /notifications

Even processes can be modeled as resources.

---

### 14. Resources vs endpoints 

Beginners mix these up.

- Resource = **concept**
- Endpoint = **method + URL**

Example:

Resource: Order 123

Endpoints:
- GET /orders/123
- PATCH /orders/123
- DELETE /orders/123

Same resource.
Different interactions.

---

### 15. How good resource identification helps scaling

When resources are identified correctly:

- APIs stay stable
- Clients don’t break
- Teams work independently
- New features fit naturally

Bad resource design causes rewrites.

---

### 16. Common beginner mistakes

- Designing endpoints before resources
- Using verbs in URLs
- Over-nesting resources
- Exposing DB tables directly
- Creating one-off “action endpoints”

All of these stem from poor resource identification.

---

### 17. A simple step-by-step method 

When designing any system:

1. Write down all nouns in the domain
2. Remove UI-only concepts
3. Keep business-meaningful nouns
4. Check CRUD applicability
5. Identify relationships
6. Name resources clearly and consistently

This works every time.

---

### 18. Mental checklist for a good resource

A good resource:

- Has a clear meaning
- Is stable over time
- Is noun-based
- Is uniquely identifiable
- Makes sense to clients

If it passes this → keep it.

---

### 19. Why this matters more than code

Frameworks change.
Languages change.
Databases change.

But **resource design lasts for years**.

Good APIs are remembered.
Bad ones are rewritten.

---

### 20. Final definition 

Identifying resources means discovering and defining the meaningful, noun-based entities in a system that clients can interact with, which then become the foundation of a clean, scalable REST API.

---

### 21. Mental model to remember forever

Think like this:

- Resources = things that matter
- URLs = addresses of those things
- HTTP methods = actions on those things

If you get resources right,
REST becomes easy.
Everything else is details.


