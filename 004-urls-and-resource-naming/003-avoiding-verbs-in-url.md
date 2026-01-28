## Avoiding Verbs in URL

### 1. Start with the most common beginner mistake

Almost every beginner does this at first:

- /getUsers  
- /createUser  
- /updateOrder  
- /deleteProduct  

It feels natural because it looks like **function names**.

But this is exactly what REST tells you **not** to do.

---

### 2. Why verbs in URLs feel natural

Beginners think like this:

- “I want to GET users” → /getUsers  
- “I want to CREATE user” → /createUser  

This comes from:

- Programming mindset  
- Function naming habits  
- RPC-style thinking  

REST works differently.

---

### 3. The core REST idea

👉 **URLs should represent resources (things)**  
👉 **HTTP methods represent actions (verbs)**  

This one rule explains everything.

---

### 4. What a URL is supposed to represent

A URL answers only one question:

👉 **What thing are you talking about?**

Examples of good URLs:

- /users  
- /users/123  
- /orders  
- /orders/456  

These are **nouns**.
They name things.

---

### 5. Where do actions go then?

Actions are expressed using **HTTP methods**:

- GET → read  
- POST → create  
- PUT / PATCH → update  
- DELETE → delete  

So instead of:

- /getUsers  

You write:

- GET /users  

Same meaning.
Correct REST design.

---

### 6. The wrong way: verb-based URLs

Examples of bad REST URLs ❌:

- GET /getUsers  
- POST /createUser  
- PUT /updateUser/123  
- DELETE /deleteOrder/456  

Why these are bad:

- Duplicate meaning (HTTP already has verbs)
- Harder to scale
- Inconsistent patterns
- Looks like RPC, not REST

---

### 7. The right way: noun-based URLs

Correct REST design ✅:

- GET /users  
- POST /users  
- GET /users/123  
- PATCH /users/123  
- DELETE /users/123  

Notice:

- URL stays the same  
- Method changes behavior  

This is intentional.

---

### 8. Visual comparison

❌ Verb in URL:
- POST /createOrder
- POST /cancelOrder
- POST /shipOrder

✅ Resource-based:
- POST   /orders
- PATCH  /orders/123   (status = cancelled)
- PATCH  /orders/123   (status = shipped)

One resource.
Many actions.
Clean design.

---

### 9. Express example: bad vs good

❌ Bad (verb-based):

    app.post('/createUser', (req, res) => {
      res.send('User created');
    });

✅ Good (resource-based):

    app.post('/users', (req, res) => {
      res.status(201).send('User created');
    });

Same logic.
Better API.

---

### 10. Why verbs in URLs don’t scale

Imagine adding new actions:

- /activateUser  
- /deactivateUser  
- /suspendUser  
- /unsuspendUser  

This explodes quickly.

With REST:

- PATCH /users/123  
  body: { "status": "active" }

No new URLs needed.

---

### 11. Actions are usually state changes

Most “actions” are actually:

👉 **Changes to resource state**

Example:

Order states:
- pending
- paid
- shipped
- cancelled

Instead of:

- /payOrder
- /shipOrder
- /cancelOrder

Use:

- PATCH /orders/123  
  { "status": "paid" }

This is REST thinking.

---

### 12. What about special actions like login/logout?

Good question (advanced but important).

Instead of:

- /loginUser
- /logoutUser

Model them as resources:

- POST /sessions   → login (create session)
- DELETE /sessions → logout (destroy session)

Still no verbs in URL.
Still RESTful.

---

### 13. When verbs MAY appear

Verbs are acceptable only when:

- The operation cannot be modeled as a resource
- It represents a process, not a thing

Even then, prefer nouns:

- /reports/generate ❌
- /reports ✔ (generation happens via POST)

Verbs should be the **last resort**, not the default.

---

### 14. REST vs RPC

Verb-based URLs are closer to **RPC**:

- “Call this function”
- “Do this action”

REST is about:

- Identifying resources
- Applying standard operations

Different philosophy.

---

### 15. Common beginner mistakes

- Treating URLs like function names
- Encoding business logic in paths
- Creating one endpoint per action
- Ignoring HTTP method semantics

All of these lead to messy APIs.

---

### 16. Why big APIs avoid verbs

Large APIs need:

- Predictability
- Consistency
- Longevity

Verb-based URLs change often.
Resource-based URLs last for years.

---

### 17. A simple decision rule 

Before adding a verb to a URL, ask:

- Can this be represented as a resource?
- Can this be expressed using HTTP methods?
- Is this just a state change?

If yes → **do not use a verb**.

---

### 18. Final definition

Avoiding verbs in URLs means designing REST APIs where URLs represent noun-based resources, and all actions are expressed using standard HTTP methods rather than action words in the path.

---

### 19. Mental model to remember forever

Think like this:

- URL = noun (thing)
- HTTP method = verb (action)

You don’t say:
- GET /eatApple

You say:
- GET /apples/1

Same rule.
Every time.
No exceptions.
