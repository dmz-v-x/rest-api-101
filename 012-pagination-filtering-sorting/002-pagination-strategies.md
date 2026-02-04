## Pagination Strategies

### 0. Why pagination strategy matters

You already know **why pagination exists**.

Now comes the next real-world question:

👉 *How do we paginate correctly as data grows?*

There are **two main strategies** used in real APIs:

1. Offset-based pagination  
2. Cursor-based pagination  

Both exist for a reason.
Both have trade-offs.
Choosing the wrong one causes serious bugs at scale.

---

## PART 1 — OFFSET-BASED PAGINATION

---

### 1. What offset-based pagination is 

👉 **Offset-based pagination** means:

“Skip the first N records and return the next M records.”

It is based on **position**, not identity.

Example:

    GET /users?page=2&limit=10

Meaning:
- Skip first 10 users
- Return next 10 users

---

### 2. How offset pagination works internally

Formula:

    offset = (page - 1) * limit

Example:
- page = 3
- limit = 10
- offset = 20

So the database query becomes:

“Give me 10 users starting from position 20.”

---

### 3. Express demo: offset-based pagination

    app.get('/users', (req, res) => {
      const page = Number(req.query.page) || 1;
      const limit = Number(req.query.limit) || 10;

      const offset = (page - 1) * limit;

      const data = users.slice(offset, offset + limit);

      res.json({
        page,
        limit,
        total: users.length,
        data
      });
    });

This is simple, readable, and intuitive.

---

### 4. Why offset pagination is popular

Offset pagination is popular because:

- Easy to understand
- Easy to implement
- Easy for UI pagination (page numbers)
- Works well for small datasets

This is why beginners start here.

---

### 5. Problems with offset pagination 

Offset pagination breaks down at scale.

#### Problem 1: Performance

As offset grows:
- Database must scan more rows
- Queries become slower
- Large offsets are expensive

Example:
- OFFSET 1,000,000 is very slow

---

### 6. Problem 2: Inconsistent results 

If data changes between requests:

- New records inserted
- Records deleted

Then:
- Items shift positions
- Clients may see duplicates
- Clients may miss records

This causes **pagination drift**.

---

### 7. When offset pagination is acceptable

Offset pagination is okay when:

- Dataset is small
- Data rarely changes
- Order is stable
- UX requires page numbers

Examples:
- Admin dashboards
- Reports
- Backoffice tools

---

## PART 2 — CURSOR-BASED PAGINATION

---

### 8. What cursor-based pagination is 

👉 **Cursor-based pagination** means:

“Give me the next set of records *after this specific item*.”

Instead of positions,
we paginate using **identity or ordering value**.

Example:

    GET /users?limit=10&cursor=abc123

Meaning:
- Start *after* item abc123
- Return next 10 users

---

### 9. Key insight (lock this in)

👉 Offset pagination is **position-based**  
👉 Cursor pagination is **value-based**

This difference is everything.

---

### 10. What a cursor actually is

A cursor is usually:
- A unique ID
- A timestamp
- A compound value (id + time)

Example cursor values:
- lastUserId = 42
- createdAt = 2026-02-01T10:30:00Z

The cursor represents **where to continue from**.

---

### 11. Express demo: cursor-based pagination

Assume users are sorted by `id`.

    app.get('/users', (req, res) => {
      const limit = Number(req.query.limit) || 10;
      const cursor = req.query.cursor;

      let startIndex = 0;

      if (cursor) {
        const index = users.findIndex(u => u.id === Number(cursor));
        startIndex = index + 1;
      }

      const data = users.slice(startIndex, startIndex + limit);

      const nextCursor =
        data.length > 0 ? data[data.length - 1].id : null;

      res.json({
        limit,
        nextCursor,
        data
      });
    });

This gives:
- Stable pagination
- Predictable results

---

### 12. Why cursor pagination scales better

Cursor pagination:

- Does not scan skipped rows
- Works efficiently with indexes
- Handles large datasets well
- Avoids duplicates and gaps

This is why large systems prefer it.

---

### 13. Cursor pagination handles data changes safely

If new data is inserted:
- Cursor still points to a specific item
- No shifting
- No duplication
- No missing records

This is critical for:
- Feeds
- Timelines
- Activity logs

---

## PART 3 — OFFSET VS CURSOR 

---

### 14. Core differences 

Offset-based:
- Uses page numbers
- Uses offsets
- Can skip data incorrectly
- Slower at scale
- Easy UX

Cursor-based:
- Uses cursors
- Uses item identity
- Stable results
- Fast at scale
- Slightly harder UX

---

### 15. UX implications

Offset pagination:
- Page 1, 2, 3…
- Jump to page 10

Cursor pagination:
- Next / Previous
- Infinite scroll
- No random page jumps

Cursor pagination fits modern UX patterns.

---

### 16. Caching implications

Offset pagination:
- Cacheable per page
- Pages shift when data changes

Cursor pagination:
- Cacheable per cursor
- More stable cache keys

Cursor-based caching is more predictable.

---

## PART 4 — WHEN TO USE WHICH

---

### 17. Use offset pagination when

- Dataset is small
- Data is mostly static
- Page numbers are required
- Performance is not critical

---

### 18. Use cursor pagination when

- Dataset is large
- Data changes frequently
- You need consistency
- You build feeds or timelines
- You care about performance

Most modern APIs use cursor pagination.

---

## PART 5 — COMMON BEGINNER MISTAKES

---

### 19. Mistakes to avoid

- Using offset pagination for infinite feeds
- Allowing huge offsets
- Mixing offset and cursor styles
- Forgetting stable sort order
- Not documenting pagination behavior

Pagination bugs are subtle and painful.

---

## PART 6 — FINAL SUMMARY

---

### 20. Final definitions 

- **Offset-based pagination** retrieves data based on positional offsets and is simple but unreliable at scale  
- **Cursor-based pagination** retrieves data relative to a specific item and provides stable, scalable, and consistent pagination  

---

### 21. Mental model to remember forever

Think like this:

- Offset pagination → “Start counting from position 100”
- Cursor pagination → “Continue after this exact item”

Counting positions breaks when things move.
Following items does not.

That’s why cursor pagination wins at scale.
