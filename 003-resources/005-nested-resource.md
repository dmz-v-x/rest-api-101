## Nested Resource

### 1. Start with a simple real-world relationship

Think about a user in an application.

That user can have:

- Orders  
- Addresses  
- Payments  

These things **belong to** the user.

REST represents this “belongs to” relationship using **nested resources**.

---

### 2. What are nested resources? 

👉 **Nested resources** are:

Resources that exist **inside the context of another resource**.

They represent relationships like:

- owns  
- belongs to  
- is part of  

Example:

- /users/123/orders  
- /orders/456/items  

Here, one resource is nested under another.

---

### 3. Why nested resources exist

Nested resources solve a simple problem:

👉 How do we represent relationships between resources clearly?

Instead of guessing:

- Which user?
- Which order?

The URL tells the story.

---

### 4. Nested resources vs flat resources

Compare these two designs:

**Flat:**
- /orders?userId=123  

**Nested:**
- /users/123/orders  

Both can work.

Nested version:

- Is more expressive  
- Clearly shows ownership  
- Is easier to reason about  

---

### 5. When to use nested resources 

Use nested resources when:

- Child resource **cannot exist without** parent  
- Child resource is strongly owned by parent  

Examples:

- User → Orders  
- Order → Items  
- Blog → Comments  

Good nesting reflects **strong relationships**.

---

### 6. When NOT to use nested resources

Do NOT nest when:

- Resource exists independently  
- Resource is shared between parents  
- Nesting becomes too deep  

Example ❌:

- /users/123/products/456  

Products don’t belong to a user.

---

### 7. Single vs nested resource clarity

Example:

- /orders/456  
  → order itself  

- /users/123/orders/456  
  → order 456 **belonging to user 123**  

Both represent the same order, but the second enforces context.

---

### 8. Express example: nested resources

Basic Express example:

    const express = require('express');
    const app = express();

    app.use(express.json());

    // Nested collection
    app.get('/users/:userId/orders', (req, res) => {
      res.json([
        { id: 1, userId: req.params.userId }
      ]);
    });

    // Nested single resource
    app.get('/users/:userId/orders/:orderId', (req, res) => {
      res.json({
        id: req.params.orderId,
        userId: req.params.userId
      });
    });

    app.listen(3000);

The URL enforces the relationship.

---

### 9. Creating nested resources

To create a child resource:

👉 POST to the **nested collection**

Example:

- POST /users/123/orders  

This clearly says:

“Create an order for user 123”

This is REST clarity at its best.

---

### 10. Updating nested resources

To update a specific nested resource:

- PATCH /users/123/orders/456  

The server can validate:

- Order exists  
- Order belongs to user 123  

This improves security and correctness.

---

### 11. Nested resources and authorization

Nested URLs help authorization:

- User can only access their own orders  
- Admin can access all orders  

URL context helps enforce permissions cleanly.

---

### 12. Avoid deep nesting 

Beginner mistake ❌:

- /users/123/orders/456/items/789/options/999  

This becomes:

- Hard to read  
- Hard to maintain  
- Hard to version  

Rule of thumb:

👉 Nest **1 level**, maybe 2.  
Flatten beyond that.

---

### 13. Flattening alternative 

Instead of deep nesting:

- /order-items?orderId=456  

or

- /items/789  

Context can move to query params.

REST allows flexibility.

---

### 14. Nested resources are still resources

Important mindset:

Nested resources:

- Still use HTTP methods  
- Still follow REST rules  
- Still have identities  

Nesting only changes **context**, not behavior.

---

### 15. Common beginner mistakes

- Nesting everything  
- Nesting unrelated resources  
- Using deep hierarchies  
- Forgetting single-resource endpoints  

Balance is key.

---

### 16. How big systems use nested resources

Large systems often:

- Use nesting for ownership  
- Provide flat endpoints for direct access  

Example:

- /users/123/orders  
- /orders/456  

Both exist.

---

### 17. Why nested resources improve API clarity

They:

- Communicate relationships  
- Reduce ambiguity  
- Improve security checks  
- Match real-world thinking  

Well-designed nesting feels natural.

---

### 18. Final definition 

**Nested resources** are REST resources that are defined within the context of another resource to represent strong ownership or “belongs to” relationships, using hierarchical URLs.

---

### 19. Mental model to remember forever

Think like this:

- Parent resource = folder  
- Nested resource = file inside folder  

Files belong to folders.

If that relationship makes sense in your head, nesting makes sense in REST too.
