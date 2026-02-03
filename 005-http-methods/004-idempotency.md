## Idempotency

### 1. Start with a very simple real-world idea

Imagine a light switch.

If you flip the switch **ON** once → light turns on  
If you flip the switch **ON** again → light is still on  

Nothing changes after the first time.

This idea is called **idempotency**.

---

### 2. What idempotency means

👉 **Idempotency** means:

Performing the same operation multiple times results in **the same final state** as performing it once.

Key focus:
- Final outcome
- Not how many times you call it

---

### 3. Idempotency in HTTP 

In HTTP:

👉 A method is **idempotent** if making the **same request multiple times** has the **same effect on the server** as making it once.

This is critical for:
- Retries
- Network failures
- Distributed systems
- Reliability

---

### 4. Idempotency is NOT about responses 

Important clarification:

❌ Idempotency is NOT about getting the same response  
✅ Idempotency is about ending up in the same **server state**

Example:
- DELETE /users/1 → 204
- DELETE /users/1 → 404

Different responses ✅  
Same final state (user is gone) ✅  
→ Still idempotent

---

### 5. Why idempotency exists 

Networks are unreliable.

Requests can:
- Timeout
- Fail midway
- Be retried automatically

If a request is retried:
- Will it break data?
- Will it duplicate actions?

Idempotency answers this safely.

---

### 6. Idempotent HTTP methods

These HTTP methods are **idempotent by definition**:

- GET
- PUT
- DELETE
- HEAD
- OPTIONS

Calling them multiple times is safe.

---

### 7. Non-idempotent HTTP methods

These are **not idempotent**:

- POST
- PATCH (usually)

Calling them multiple times can:
- Create duplicates
- Apply changes repeatedly

---

### 8. GET — idempotent and safe

GET:
- Reads data
- Does not change data
- Same request → same state

Example:
- GET /users/1
- GET /users/1

Server state remains unchanged.

---

### 9. PUT — idempotent but unsafe

PUT replaces a resource.

Example:
- PUT /users/1  
  body: { "name": "Alex", "age": 30 }

Calling it:
- Once → user replaced
- Twice → user replaced again (same data)

Final state is the same.

PUT is:
- Idempotent ✅
- Unsafe ❌ (changes data)

---

### 10. DELETE — idempotent but unsafe

DELETE removes a resource.

Example:
- DELETE /users/1

Calling it:
- First time → user removed
- Second time → user already removed

Final state:
- User does not exist

DELETE is idempotent.

---

### 11. POST — NOT idempotent

POST usually creates new resources.

Example:
- POST /users

Calling it:
- Once → user created
- Twice → two users created

Final state changes each time.

POST is NOT idempotent.

---

### 12. PATCH — usually NOT idempotent

PATCH applies partial updates.

Example:
- PATCH /users/1  
  body: { "age": 30 }

If PATCH means:
- “Set age to 30” → idempotent
- “Increase age by 1” → NOT idempotent

This is why PATCH idempotency depends on implementation.

---

### 13. Express example: idempotent PUT

    app.put('/users/:id', (req, res) => {
      users[req.params.id] = req.body;
      res.json(users[req.params.id]);
    });

Calling this multiple times:
- Produces same final state
- No duplication

---

### 14. Express example: non-idempotent POST

    app.post('/users', (req, res) => {
      users.push(req.body);
      res.status(201).send();
    });

Calling this multiple times:
- Adds multiple users
- Changes state every time

---

### 15. Idempotency vs Safety 

These are different concepts.

Safe:
- Does not change data

Idempotent:
- Can change data
- But repeated calls don’t change final state

Examples:
- GET → safe + idempotent
- DELETE → unsafe + idempotent
- POST → unsafe + not idempotent

---

### 16. Why idempotency matters for retries

Imagine this:

Client sends:
- POST /payments

Network times out.
Client retries.

Without idempotency:
- Payment charged twice 

With idempotency:
- Server detects duplicate
- Prevents double charge

This is huge in real systems.

---

### 17. Idempotency keys 

To make POST idempotent, systems use **idempotency keys**.

Example:
- Idempotency-Key: abc123

Server logic:
- If key seen before → return same result
- If new → process request

Used by:
- Stripe
- Payment gateways
- Order systems

---

### 18. When you MUST think about idempotency

Always think about idempotency for:
- Payments
- Orders
- Emails
- Inventory updates
- Distributed systems

Mistakes here cost real money.

---

### 19. Common beginner mistakes

- Assuming POST is safe to retry
- Confusing idempotency with caching
- Thinking same response = idempotent
- Ignoring retries in design

---

### 20. Quick decision table

GET:
- Safe
- Idempotent

PUT:
- Unsafe
- Idempotent

DELETE:
- Unsafe
- Idempotent

POST:
- Unsafe
- Not idempotent

PATCH:
- Unsafe
- Depends on implementation

---

### 21. Final definition 

**Idempotency** means that performing the same HTTP request multiple times results in the same final server state as performing it once, which is essential for safe retries and reliable distributed systems.

---

### 22. Mental model to remember forever

Think like this:

- Idempotent = setting a value
- Non-idempotent = adding something

“Set volume to 5” → idempotent  
“Increase volume by 1” → not idempotent  

HTTP works the same way.
