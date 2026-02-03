## Sorting & Searching using Query Params

### 1. Start with a simple everyday problem

Imagine an online store with thousands of products.

Sometimes you want:
- the cheapest products first
- the newest products first
- products matching a name

You don’t want a new API endpoint for each case.

This is where **sorting and searching using query params** comes in.

---

### 2. What sorting means

👉 **Sorting** means:

Arranging a list of resources in a specific order.

Common sorting needs:
- Price: low → high
- Date: newest → oldest
- Name: A → Z

Sorting does NOT change:
- the resource
- which items exist

It only changes the **order**.

---

### 3. What searching means

👉 **Searching** means:

Finding resources that match a keyword or partial input.

Examples:
- Search users by name
- Search products by title
- Search articles by content

Searching usually:
- Uses free text
- Is less structured than filtering

---

### 4. Why sorting and searching use query params

REST rule reminder:

👉 **Query parameters modify how a collection is returned**

Sorting and searching:
- Do not identify a resource
- Do not create new resources

So they belong in **query params**.

---

### 5. Basic sorting with query params

A common pattern:

- /products?sort=price
- /products?sort=createdAt

This tells the server:
“Return products sorted by this field.”

---

### 6. Sorting direction

Sorting often needs direction.

Common conventions:
- asc / desc
- + / -

Examples:
- /products?sort=price&order=asc
- /products?sort=createdAt&order=desc

Clear and readable.

---

### 7. Multiple sort fields 

Some APIs support multiple sorts:

- /products?sort=price,name
- /products?sort=price:asc,name:desc

Order matters.

This is advanced but still query-based.

---

### 8. Searching with query params

Searching usually uses a generic parameter:

- /users?search=alex
- /products?query=iphone
- /articles?q=javascript

The name varies, but the idea is the same.

---

### 9. Searching vs filtering

Filtering:
- Exact match
- Known fields
- Structured

Example:
- /users?role=admin

Searching:
- Partial match
- Free text
- Fuzzy

Example:
- /users?search=ali

Both often coexist.

---

### 10. Combining filtering, sorting, and searching

Very common real-world usage:

- /products?category=phones&search=iphone&sort=price&order=asc

Breakdown:
- category → filter
- search → search
- sort → order results

Still one endpoint.
Still RESTful.

---

### 11. Express example: sorting

Simple Express sorting example:

    app.get('/products', (req, res) => {
      let products = [
        { name: 'Phone', price: 500 },
        { name: 'Laptop', price: 1200 }
      ];

      if (req.query.sort === 'price') {
        products.sort((a, b) => a.price - b.price);
      }

      res.json(products);
    });

Sorting logic lives server-side.

---

### 12. Express example: searching

Simple search example:

    app.get('/users', (req, res) => {
      const search = req.query.search;

      let users = [
        { name: 'Alex' },
        { name: 'Alice' },
        { name: 'Bob' }
      ];

      if (search) {
        users = users.filter(u =>
          u.name.toLowerCase().includes(search.toLowerCase())
        );
      }

      res.json(users);
    });

This demonstrates basic text search.

---

### 13. Case sensitivity and normalization

Good APIs:
- Normalize input
- Handle case-insensitive search

Clients should not worry about casing.

---

### 14. Sorting and pagination work together

Sorting without pagination is dangerous at scale.

Common combo:

- /products?sort=price&order=desc&page=2&limit=20

Sorting defines order.
Pagination limits results.

---

### 15. Validation is critical

Servers must validate:

- Sort fields (allowed only)
- Order values (asc/desc)
- Search length (prevent abuse)

Invalid → 400 Bad Request.

---

### 16. Performance considerations

Searching large datasets:
- Requires indexes
- May require full-text search engines

The API contract stays the same.
Implementation evolves.

---

### 17. Common beginner mistakes

- Creating separate endpoints for sorting
- Putting sorting in path
- No validation
- Allowing arbitrary fields
- Ignoring pagination

These hurt scalability.

---

### 18. Design conventions

Good conventions:
- search or q for searching
- sort for sort field
- order for direction

Consistency > perfection.

---

### 19. Why query-based sorting scales well

Query params allow:
- Endless combinations
- No new endpoints
- Backward compatibility

This is why REST favors them.

---

### 20. Final definition

Sorting and searching using query params means controlling the order and selection of resource collections by passing optional instructions in the URL query string, without changing the identity of the resource itself.

---

### 21. Mental model to remember forever

Think like this:

- Resource = list on a table
- Sorting = rearranging rows
- Searching = highlighting matching rows

You don’t create a new table.
You just change how you look at it.

Once this clicks,
query-based sorting and searching will feel obvious.
