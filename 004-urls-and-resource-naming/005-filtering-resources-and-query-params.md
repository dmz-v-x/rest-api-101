## Filtering Resources and Query Params

### 1. Start with a very simple real-world need

Imagine you have **thousands of users**.

You don’t always want:
- all users

Sometimes you want:
- only active users
- only admin users
- users from a specific country

This is where **filtering resources** comes in.

---

### 2. What filtering means

👉 **Filtering** means:

Reducing a large collection of resources into a smaller, more relevant subset.

You are not changing the resource itself.
You are just asking:

“Give me only the ones that match these conditions.”

---

### 3. Why filtering belongs to query params

REST rule:

👉 **Query parameters modify how a resource is returned**

They do NOT change:
- which resource it is
- its identity

So filtering belongs naturally in **query parameters**.

Example:

- /users?status=active

Resource: users  
Filter: status=active

---

### 4. The wrong way to filter

❌ Bad designs:

- /activeUsers
- /users/active
- /getUsersByStatus

Problems:
- Looks like new resources
- Hard to extend
- Violates REST conventions

---

### 5. The right way to filter

✅ RESTful filtering:

- /users?status=active
- /orders?state=pending
- /products?category=electronics

Same resource.
Different view.

---

### 6. Multiple filters together

You can combine filters easily:

- /users?status=active&role=admin
- /orders?status=completed&year=2025

Each query param adds a condition.

This scales naturally.

---

### 7. Query params are optional

These are all valid:

- /users
- /users?status=active
- /users?status=active&role=admin

Clients choose how much filtering they want.

---

### 8. Express example: basic filtering

Simple Express route:

    app.get('/users', (req, res) => {
      const { status, role } = req.query;

      res.json({
        filterApplied: { status, role }
      });
    });

Express gives you query params as an object.

---

### 9. Filtering vs searching

Filtering:
- Exact matches
- Known fields
- Structured

Example:
- /users?status=active

Searching:
- Free text
- Partial matches

Example:
- /users?search=alex

Both usually use query params.

---

### 10. Filtering numeric ranges

Query params can represent ranges:

- /products?minPrice=100&maxPrice=500
- /orders?fromDate=2025-01-01&toDate=2025-01-31

This is common and REST-friendly.

---

### 11. Boolean filters

Boolean filters are very common:

- /users?active=true
- /features?enabled=false

They read naturally.

---

### 12. Filtering nested resources

Filtering works with nested resources too:

- /users/123/orders?status=completed
- /orders/456/items?inStock=true

Path defines context.
Query filters within that context.

---

### 13. Express example: real filtering logic

Example with mock data:

    app.get('/orders', (req, res) => {
      let orders = [
        { id: 1, status: 'pending' },
        { id: 2, status: 'completed' }
      ];

      if (req.query.status) {
        orders = orders.filter(
          o => o.status === req.query.status
        );
      }

      res.json(orders);
    });

This shows how filtering is applied server-side.

---

### 14. Filtering vs pagination vs sorting

Important separation:

Filtering:
- reduces results

Pagination:
- limits results

Sorting:
- orders results

Example:

- /users?status=active&page=2&sort=createdAt

All work together using query params.

---

### 15. Why filtering should not use path params

❌ Bad:

- /users/active
- /users/admin

These look like new resources.

Filters are not resources.
They are conditions.

---

### 16. Validation of filter params

Servers must validate filters:

- Allowed fields
- Allowed values
- Types

Invalid filters → 400 Bad Request

This protects your API.

---

### 17. Advanced filtering patterns

Large APIs may support:

- AND filters (default)
- OR filters
- IN filters
- Date ranges

Even then, they stay in query params.

---

### 18. Common beginner mistakes

- Creating new endpoints for each filter
- Putting filters in path
- Overloading query params
- No validation
- Unclear naming

These cause maintenance pain.

---

### 19. Why filtering matters for performance

Filtering:
- Reduces data transfer
- Improves response times
- Reduces client-side work

It’s not just design — it’s performance.

---

### 20. Final definition

Filtering resources means narrowing down a collection of resources based on conditions, and in REST APIs this is done using query parameters that modify how the resource collection is returned without changing its identity.



