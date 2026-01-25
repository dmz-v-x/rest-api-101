## Resource vs Action

### 1. Start with the beginner confusion

Almost every beginner asks this:

- “Should I create an endpoint called `/createUser`?”
- “Should I have `/getOrders`?”
- “How do I represent actions in REST?”

This confusion comes from **mixing two ideas**:

👉 **Resources**  
👉 **Actions**

Understanding the difference between them is one of the most important REST lessons.

---

### 2. What is a resource? 

👉 A **resource** is:

A meaningful “thing” in your system that can be identified and interacted with.

Examples:

- User  
- Order  
- Product  
- Payment  

Resources are:

- Nouns  
- Stable  
- Identified by URLs  

Example:

- /users  
- /orders/123  

---

### 3. What is an action? 

👉 An **action** is:

Something you **do** to a resource.

Examples:

- Create  
- Read  
- Update  
- Delete  
- Approve  
- Cancel  

Actions are **verbs**.

---

### 4. The core REST rule 

👉 **Resources go in the URL**  
👉 **Actions go in the HTTP method**

This single rule clears 90% of confusion.

---

### 5. The wrong way 

Beginners often design APIs like this ❌:

- /createUser  
- /getUser  
- /updateUser  
- /deleteUser  

Why this is bad:

- HTTP already has methods for actions  
- URLs become inconsistent  
- API becomes hard to scale  

This is RPC-style, not REST.

---

### 6. The REST way 

REST design looks like this ✅:

- POST   /users        → create user  
- GET    /users/123    → read user  
- PATCH  /users/123    → update user  
- DELETE /users/123    → delete user  

Same resource.  
Different actions via HTTP methods.

---

### 7. Visual comparison

❌ Action-based:
- /createOrder
- /cancelOrder
- /getOrder

✅ Resource-based:
- POST   /orders
- PATCH  /orders/123
- GET    /orders/123

The second is predictable and scalable.

---

### 8. Why REST separates resource and action

REST separates them because:

- Resources are stable (users always exist)
- Actions may change over time
- HTTP already defines common actions
- Uniformity improves understanding

Clients don’t guess.
They already know what GET or POST means.

---

### 9. Express example: action-based vs resource-based

❌ Bad (action-based):

    app.post('/createUser', (req, res) => {
      res.send('User created');
    });

✅ Good (resource-based):

    app.post('/users', (req, res) => {
      res.status(201).send('User created');
    });

Same result.  
Better design.

---

### 10. What about “special actions”? 

Beginners often ask:

“What about actions like approve, cancel, login, logout?”

Good question.

---

### 11. Model actions as resource state changes

Most “actions” are actually **state changes**.

Example:

Order states:
- pending
- approved
- cancelled

REST approach:

    PATCH /orders/123
    {
      "status": "cancelled"
    }

You didn’t create `/cancelOrder`.  
You updated the order resource.

---

### 12. When actions deserve their own resources

Some actions represent **processes**.

Examples:

- Login
- File upload
- Password reset

These can be modeled as resources:

- POST /sessions
- POST /uploads
- POST /password-resets

They are not verbs — they are **things**.

---

### 13. Express example: action as resource

Login example:

    app.post('/sessions', (req, res) => {
      res.json({ token: 'abc123' });
    });

You created a session resource.
No `/loginUser` needed.

---

### 14. Why verbs in URLs cause long-term pain

Action-based URLs lead to:

- Inconsistent naming
- Hard-to-document APIs
- Tight coupling
- Difficult versioning

Resource-based APIs age much better.

---

### 15. REST vs RPC 

Action-based APIs are closer to **RPC**:

- Call a function
- Get a result

REST is different:

- Identify a resource
- Apply standard actions

REST is about **data**, not functions.

---

### 16. Common beginner mistakes

- Using verbs in URLs
- Creating endpoints per action
- Ignoring HTTP methods
- Designing from UI actions instead of domain data

These mistakes scale poorly.

---

### 17. How to decide: resource or action? 

Ask yourself:

1. Is it a noun? → resource  
2. Can it be identified? → resource  
3. Is it just changing data? → use PATCH  
4. Is it a long-lived concept? → resource  

If yes → model it as a resource.

---

### 18. Why this matters in real systems

Correct separation means:

- Cleaner APIs
- Easier frontend integration
- Better caching
- Easier security
- Easier scaling

This is not theory — this is production wisdom.

---

### 19. One-sentence rule to remember forever

👉 **URLs name things. HTTP methods describe actions.**

If you remember only this, you’ll design good REST APIs.

---

### 20. Final definition 

A **resource** is a noun that represents a thing in your system, while an **action** is an operation performed on that resource and is expressed using HTTP methods, not URL names.

---

### 21. Mental model to remember forever

Think like this:

- Resource = object  
- URL = address of the object  
- HTTP method = what you want to do with it  

Never mix verbs into addresses.

Once this clicks, REST stops being confusing and starts feeling obvious.
