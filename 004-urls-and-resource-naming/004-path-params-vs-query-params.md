## Path Params vs Query Params

### 1. Start with the beginner confusion

One of the most common API questions is:

- “When should I use `/users/123`?”
- “When should I use `/users?id=123`?”
- “Are path params and query params the same?”

They look similar, but they serve **very different purposes**.

Understanding this difference is a **core REST skill**.

---

### 2. What are path parameters? 

👉 **Path parameters** are values that are part of the URL path.

They are used to **identify a specific resource**.

Example:

- /users/123

Here:
- `123` is a path parameter
- It identifies **which user**

Without it, the server doesn’t know which user you mean.

---

### 3. What are query parameters?

👉 **Query parameters** are key–value pairs added after `?`.

They are used to **modify how a request behaves**, not what resource it is.

Example:

- /users?role=admin&page=2

Here:
- role and page modify the result
- The resource is still `/users`

---

### 4. The core difference

👉 **Path params identify the resource**  
👉 **Query params modify or filter the resource**

This single sentence clears most confusion.

---

### 5. Visual comparison

Path parameter example:

- GET /orders/456  
→ “Get order 456”

Query parameter example:

- GET /orders?status=shipped  
→ “Get orders that are shipped”

Very different intent.

---

### 6. Path params are usually required

Path params are:

- Mandatory
- Positional
- Part of the resource identity

Example:

- GET /users/:id  

Without `id`, the request makes no sense.

---

### 7. Query params are usually optional

Query params are:

- Optional
- Flexible
- Order-independent

Example:

- /users
- /users?page=2
- /users?page=2&limit=10

All are valid.

---

### 8. Path params for single resources

Path params are perfect for:

- Single resource access

Examples:

- /users/123
- /orders/456
- /products/789

These represent **one specific thing**.

---

### 9. Query params for collections

Query params shine when working with collections.

Examples:

- /users?role=admin
- /orders?status=pending
- /products?minPrice=100&maxPrice=500

You are still dealing with the same collection.
You’re just narrowing it.

---

### 10. Mixing path and query params

They are often used together:

- /users/123/orders?status=completed&page=1

Meaning:

- User 123’s orders
- Only completed ones
- First page

Path → identity  
Query → behavior

---

### 11. Express example: path params

Express path param example:

    app.get('/users/:id', (req, res) => {
      console.log(req.params.id); // path param
      res.json({ id: req.params.id });
    });

Express extracts them cleanly.

---

### 12. Express example: query params

Express query param example:

    app.get('/users', (req, res) => {
      console.log(req.query.page);   // query param
      console.log(req.query.role);   // query param

      res.json({ page: req.query.page, role: req.query.role });
    });

Query params are key–value objects.

---

### 13. Why NOT use query params for identity

Bad design ❌:

- /users?id=123

Problems:

- Identity hidden in query
- Harder to cache
- Less RESTful
- Less readable

Better design ✅:

- /users/123

Clear and intentional.

---

### 14. Why NOT use path params for filtering

Bad design ❌:

- /users/admin
- /orders/pending

These look like new resources.

Better design ✅:

- /users?role=admin
- /orders?status=pending

Filtering belongs in query params.

---

### 15. How caching sees them

Caching systems treat:

- /users/123
- /users?role=admin

Very differently.

Path changes → different resource  
Query changes → same resource, different view

This is why correct usage matters.

---

### 16. Common real-world use cases

Use path params when:
- Identifying a single resource
- Resource must exist

Use query params when:
- Filtering
- Sorting
- Pagination
- Searching

This covers most scenarios.

---

### 17. Common beginner mistakes

- Using query params for IDs
- Using path params for filters
- Mixing responsibilities
- Overloading paths

These lead to confusing APIs.

---

### 18. A simple decision checklist

Before choosing, ask:

- Am I identifying *which* resource? → path param  
- Am I modifying *how* it’s returned? → query param  
- Is it optional? → query param  
- Is it required for identity? → path param  

---

### 19. Final definition 

Path parameters are used to uniquely identify specific resources within a URL, while query parameters are used to modify, filter, or control how those resources are returned without changing their identity.

---
