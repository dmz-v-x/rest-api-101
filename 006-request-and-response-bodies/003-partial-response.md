## Partial Response

### 1. Start with a very common real-world problem

Imagine this API response:

    {
      "id": 1,
      "firstName": "Alex",
      "lastName": "Smith",
      "email": "alex@test.com",
      "phone": "9999999999",
      "address": { ... },
      "preferences": { ... },
      "settings": { ... },
      "auditLogs": [ ... ],
      "permissions": [ ... ]
    }

Now imagine the frontend only needs:

- id
- firstName
- lastName

Everything else is wasted.

This is where **partial responses** matter.

---

### 2. What “partial responses” means 

👉 **Partial responses** mean:

Returning **only the fields the client actually needs**, instead of always returning the full resource.

In simple words:

Don’t over-serve data.

---

### 3. Why returning everything is a bad idea

Always returning full objects causes:

- Bigger response sizes
- Slower APIs
- More bandwidth usage
- Slower mobile apps
- Accidental data leaks
- Tight frontend–backend coupling

More data ≠ better API.

---

### 4. The core principle 

👉 **APIs should be flexible in what they return, but strict in structure**

Clients should be able to ask:
- “Give me only these fields”

Server should respond:
- “Here you go, nothing extra”

---

### 5. Partial responses are about performance AND design

Partial responses help with:

- Performance (smaller payloads)
- Security (less sensitive data exposed)
- Maintainability (clients depend on fewer fields)
- Scalability (less network & memory usage)

This is not an optimization — it’s good API design.

---

### 6. The most common way: `fields` query param

The most popular pattern is a `fields` query parameter.

Example:

- GET /users/1?fields=id,firstName,lastName

Meaning:
👉 “Return only id, firstName, and lastName”

---

### 7. What stays the same

Important rule:

- Resource identity stays the same
- Endpoint stays the same
- Only the **representation** changes

This is still:
- GET /users/1

Just a different view.

---

### 8. Express example: basic partial response

Dummy data:

    const users = [
      {
        id: 1,
        firstName: 'Alex',
        lastName: 'Smith',
        email: 'alex@test.com',
        age: 25
      }
    ];

Route with partial response support:

    app.get('/users/:id', (req, res) => {
      const user = users.find(u => u.id === Number(req.params.id));

      if (!user) {
        return res.status(404).json({ error: 'User not found' });
      }

      const fields = req.query.fields;

      if (!fields) {
        return res.json(user);
      }

      const selectedFields = fields.split(',');
      const partialUser = {};

      selectedFields.forEach(field => {
        if (user[field] !== undefined) {
          partialUser[field] = user[field];
        }
      });

      res.json(partialUser);
    });

---

### 9. Example requests & responses

Request:
- GET /users/1

Response:

    {
      "id": 1,
      "firstName": "Alex",
      "lastName": "Smith",
      "email": "alex@test.com",
      "age": 25
    }

Request:
- GET /users/1?fields=id,firstName

Response:

    {
      "id": 1,
      "firstName": "Alex"
    }

Same resource.
Different representation.

---

### 10. Why query params are the right place

Query params are used to:
- filter
- sort
- paginate
- **shape the response**

Partial responses fit perfectly here.

❌ Do NOT create new endpoints like:
- /users/summary
- /users/basic

This doesn’t scale.

---

### 11. Partial responses vs filtering 

Filtering:
- Reduces **rows**
- Example: /users?status=active

Partial responses:
- Reduce **columns**
- Example: /users?fields=id,name

They solve different problems.

---

### 12. Partial responses and collections

Partial responses also work for collections.

Example:
- GET /users?fields=id,firstName

Response:

    [
      { "id": 1, "firstName": "Alex" },
      { "id": 2, "firstName": "Sam" }
    ]

This is extremely common in list views.

---

### 13. Always validate requested fields

Never blindly trust `fields`.

Why?
- Clients might request private fields
- Typos can cause silent bugs

Best practice:
- Maintain an allowlist of fields

Example:
- allowedFields = ['id', 'firstName', 'lastName', 'email']

Ignore or reject others.

---

### 14. Security benefit 

Partial responses reduce:

- Exposure of sensitive fields
- Accidental data leaks
- Over-sharing internal data

If a client never needs `passwordHash`,
don’t send it.

---

### 15. Partial responses reduce frontend coupling

If frontend only depends on:
- id
- firstName

Then backend can:
- change other fields
- add new fields
- refactor internals

Without breaking clients.

This is huge at scale.

---

### 16. What NOT to do 

❌ Returning full DB rows always  
❌ Creating multiple endpoints for views  
❌ Allowing arbitrary fields without validation  
❌ Breaking response shape randomly  

Partial responses should be:
- predictable
- documented
- consistent

---

### 17. When NOT to use partial responses

Avoid partial responses when:
- Resource is already very small
- Client always needs full data
- Complexity outweighs benefit

Use them intentionally, not everywhere.

---

### 18. Partial responses vs GraphQL 

GraphQL:
- Field selection is mandatory
- Built-in partial responses

REST:
- Partial responses are optional
- Implemented via query params

Both solve the same problem.
REST just does it explicitly.

---

### 19. Simple decision checklist

Before implementing partial responses, ask:

- Is the resource large?
- Do clients need different views?
- Is bandwidth a concern?
- Are sensitive fields present?

If yes → partial responses make sense.

---

### 20. Final definition

**Partial responses** allow clients to request and receive only the specific fields they need from a resource, reducing payload size, improving performance, enhancing security, and lowering frontend–backend coupling.

---

### 21. Mental model to remember forever

Think like this:

- Full response = entire meal
- Partial response = just the dishes you ordered

Don’t serve the whole kitchen
when the customer asked for a sandwich.

Good APIs serve **exactly what’s needed**.
