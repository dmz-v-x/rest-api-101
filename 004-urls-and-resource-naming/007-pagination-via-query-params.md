## Pagination via Query Params

### 1. Start with a very simple real-world problem

Imagine you have **1 million users**.

If your API returns **all users at once**:

- Response becomes huge
- Server becomes slow
- Browser crashes
- Mobile apps break

This is why **pagination exists**.

Pagination means:
👉 “Give me data in small chunks, not all at once.”

---

### 2. What pagination means

👉 **Pagination** means:

Splitting a large collection of resources into **smaller, manageable pages**.

Instead of:
- “Give me all users”

You say:
- “Give me the first 20 users”
- “Now give me the next 20”

---

### 3. Why pagination uses query params

REST rule reminder:

👉 **Query params modify how a collection is returned**

Pagination:
- Does not change the resource
- Only changes *how much* you get

So pagination belongs in **query parameters**.

Example:
- /users?page=1&limit=20

Resource: users  
Pagination: page + limit

---

### 4. The most common pagination parameters

The two most common query params are:

- `page` → which page you want
- `limit` (or `size`) → how many items per page

Example:
- /users?page=2&limit=10

Meaning:
- Skip first 10 users
- Return the next 10

---

### 5. Visualizing pagination 

Assume this data:

Users:  
1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12

With:
- limit = 4

Pages look like:

- page=1 → [1,2,3,4]
- page=2 → [5,6,7,8]
- page=3 → [9,10,11,12]

Pagination is just slicing data.

---

### 6. Why pagination is mandatory at scale

Without pagination:

- High memory usage
- Slow responses
- Network overload
- Bad user experience

With pagination:

- Faster APIs
- Predictable responses
- Happy clients

Every serious API uses pagination.

---

### 7. Express example: basic pagination

Simple Express example:

    app.get('/users', (req, res) => {
      const page = parseInt(req.query.page) || 1;
      const limit = parseInt(req.query.limit) || 10;

      const users = Array.from({ length: 50 }, (_, i) => ({
        id: i + 1
      }));

      const start = (page - 1) * limit;
      const end = start + limit;

      const paginatedUsers = users.slice(start, end);

      res.json(paginatedUsers);
    });

Key idea:
- page controls offset
- limit controls size

---

### 8. Offset-based pagination 

This is called **offset-based pagination**.

Formula:
- offset = (page - 1) × limit

Used by:
- Most REST APIs
- SQL databases (LIMIT + OFFSET)

Simple and intuitive.

---

### 9. Pagination metadata 

Good APIs return **metadata**, not just data.

Example response:

    {
      "page": 2,
      "limit": 10,
      "totalItems": 95,
      "totalPages": 10,
      "data": [...]
    }

Clients need this to:
- Show page numbers
- Disable “next” button
- Display totals

---

### 10. Pagination with filtering and sorting

Pagination works together with filtering and sorting.

Example:
- /products?category=phones&sort=price&page=2&limit=20

Order of operations:
1. Filter
2. Sort
3. Paginate

Always in this order.

---

### 11. Why pagination should never be in the path

❌ Bad:
- /users/page/2
- /users/limit/10

Pagination is not a resource.
It is a **view instruction**.

✅ Good:
- /users?page=2&limit=10

---

### 12. Page vs limit naming conventions

Common names:

- page + limit
- page + size
- offset + limit

Choose one style.
Be consistent.

Consistency > cleverness.

---

### 13. Offset pagination limitations

Offset pagination has issues when:

- Data changes frequently
- Items are inserted or deleted
- Pages shift unexpectedly

This can cause:
- Duplicate items
- Missing items

This is where cursor pagination comes in.

---

### 14. Cursor-based pagination

Instead of page numbers, you use a **cursor**.

Example:
- /users?cursor=abc123&limit=20

Cursor represents:
- “Start after this item”

Used by:
- Twitter
- GitHub
- Large-scale APIs

More complex, but more stable.

---

### 15. When beginners should NOT worry about cursors

As a beginner:
👉 Start with **page + limit**

Only move to cursor pagination when:
- You hit scale issues
- Data changes rapidly
- Performance matters deeply

Don’t over-engineer early.

---

### 16. Validation of pagination params

Servers must validate:

- page ≥ 1
- limit within reasonable bounds

Example:
- limit max = 100

Why?
- Prevent abuse
- Protect performance

Invalid → 400 Bad Request.

---

### 17. Common beginner mistakes

- Returning all data without pagination
- Using huge limits
- No pagination metadata
- Mixing pagination with path params
- No validation

These mistakes don’t show early — but hurt later.

---

### 18. Pagination and UX 

Pagination enables:
- Infinite scroll
- Page navigation
- “Load more” buttons

APIs designed well make UIs simple.

---

### 19. Why pagination is part of API design, not UI

Pagination is:
- A backend responsibility
- A performance feature
- A scalability requirement

Clients should never paginate massive data themselves.

---

### 20. Final definition 

Pagination via query params means controlling how many items from a resource collection are returned and which subset is retrieved, using optional URL parameters like page and limit to ensure scalable and efficient data access.

---

### 21. Mental model to remember forever

Think like this:

- Resource = huge book
- Pagination = reading page by page

You don’t photocopy the entire book.
You read it **one page at a time**.

That’s pagination.
