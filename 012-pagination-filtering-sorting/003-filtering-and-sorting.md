## Filtering & Sorting Conventions

### 0. Why filtering & sorting need conventions

Almost every real API supports:

- Filtering → “Give me only what I want”
- Sorting → “Give it to me in the order I want”

The mistake beginners make is:
- Inventing random query params
- Inconsistent naming
- Mixing logic into URLs
- Creating unscalable patterns

👉 **Filtering and sorting must follow clear conventions**  
Otherwise your API becomes painful to use and impossible to evolve.

---

## PART 1 — FILTERING CONVENTIONS

---

### 1. What filtering means

👉 **Filtering** means:

Reducing a collection by conditions.

Instead of:
- “Give me all users”

We say:
- “Give me only active users”
- “Give me users older than 18”

---

### 2. Core filtering principle 

👉 **Filters belong in query parameters**

Never in:
- URL paths
- Request bodies (for GET)
- Custom endpoints

Correct:
    GET /users?status=active

Incorrect:
    GET /activeUsers
    POST /users/filter

---

### 3. Simple equality filtering

The simplest and most common pattern:

    GET /users?status=active
    GET /orders?paymentStatus=paid
    GET /products?category=books

Rules:
- One field = one filter
- Query param name matches field name
- Value is the desired value

This should be your default.

---

### 4. Multiple filters (AND semantics)

Multiple query params are treated as **AND** conditions.

Example:

    GET /users?status=active&role=admin

Meaning:
- status = active
- AND role = admin

This is intuitive and widely understood.

---

### 5. Range filtering (numbers, dates)

For ranges, use **clear suffixes**.

Common conventions:
- `_gt`  → greater than
- `_gte` → greater than or equal
- `_lt`  → less than
- `_lte` → less than or equal

Example:

    GET /users?age_gte=18&age_lt=60
    GET /orders?createdAt_gte=2026-01-01

This is readable and scalable.

---

### 6. Express demo: filtering

    app.get('/users', (req, res) => {
      let result = users;

      if (req.query.status) {
        result = result.filter(u => u.status === req.query.status);
      }

      if (req.query.age_gte) {
        result = result.filter(u => u.age >= Number(req.query.age_gte));
      }

      res.json(result);
    });

Each filter is independent and composable.

---

### 7. Filtering by multiple values 

When filtering by multiple values:

    GET /users?role=admin,user

Meaning:
- role IN (admin, user)

Comma-separated values are the most common convention.

Avoid complex JSON in query params.

---

### 8. Boolean filtering

For booleans:

    GET /users?isActive=true
    GET /users?isVerified=false

Rules:
- Use explicit true / false
- Avoid magic values like 0/1 unless documented

---

### 9. What NOT to do in filtering

❌ Don’t encode logic:
    GET /users?filter=activeAdmins

❌ Don’t invent verbs:
    GET /users?getActive=true

❌ Don’t nest filters deeply:
    GET /users?filter[age][gt]=18

Simple > clever.

---

## PART 2 — SORTING CONVENTIONS

---

### 10. What sorting means 

👉 **Sorting** means:

Defining the order of returned results.

Examples:
- Newest first
- Alphabetical order
- Highest value first

Sorting never changes *which* items you get —
only the order.

---

### 11. Core sorting principle

👉 **Use a single `sort` query parameter**

Do NOT:
- Create multiple sort params
- Encode sorting in the URL path

Correct:
    GET /users?sort=name

---

### 12. Ascending vs descending order

Most common convention:

- Default → ascending
- Prefix with `-` → descending

Examples:

    GET /users?sort=name        // A → Z
    GET /users?sort=-createdAt // newest first

This pattern is widely used (Stripe, GitHub, etc.).

---

### 13. Sorting by multiple fields

Allow comma-separated fields:

    GET /users?sort=role,-createdAt

Meaning:
1. Sort by role (asc)
2. Then by createdAt (desc)

This gives deterministic ordering.

---

### 14. Express demo: sorting

    app.get('/users', (req, res) => {
      let result = [...users];

      const sort = req.query.sort;

      if (sort) {
        const fields = sort.split(',');

        result.sort((a, b) => {
          for (const field of fields) {
            const desc = field.startsWith('-');
            const key = desc ? field.slice(1) : field;

            if (a[key] < b[key]) return desc ? 1 : -1;
            if (a[key] > b[key]) return desc ? -1 : 1;
          }
          return 0;
        });
      }

      res.json(result);
    });

This supports:
- Single-field sorting
- Multi-field sorting
- Asc & desc

---

### 15. Sorting + pagination 

👉 **Always sort before paginating**

Correct order:
1. Filter
2. Sort
3. Paginate

Wrong order causes:
- Inconsistent pages
- Missing or duplicated items

This is critical.

---

### 16. Stable sorting 

Sorting must be:
- Deterministic
- Stable

If two items have same value:
- Add secondary sort (like id)

Example:

    sort=-createdAt,id

This prevents pagination bugs.

---

## PART 3 — FILTERING & SORTING TOGETHER

---

### 17. Real-world combined example

    GET /orders?
      status=paid
      &amount_gte=100
      &sort=-createdAt
      &limit=20
      &cursor=abc123

This is:
- Readable
- Predictable
- REST-friendly
- Scalable

---

### 18. Why conventions matter long-term

Good conventions give you:
- Consistent APIs
- Easy documentation
- Predictable client code
- Easier evolution

Bad conventions give you:
- Special cases
- Breaking changes
- Technical debt

---

## PART 4 — COMMON BEGINNER MISTAKES

---

### 19. Mistakes to avoid

- Creating custom filter endpoints
- Using POST for filtering
- Encoding filters in path
- Mixing filtering logic in one param
- Changing conventions between endpoints

Consistency beats creativity.

---

## PART 5 — FINAL SUMMARY

---

### 20. Final definitions 

- **Filtering conventions** define how clients restrict result sets using query parameters in a predictable, composable way  
- **Sorting conventions** define how clients control result ordering using a consistent `sort` parameter with clear direction rules  

---

### 21. Mental model to remember forever

Think like this:

- Filtering → “Which items do I want?”
- Sorting → “In what order do I want them?”

Filtering decides the set.
Sorting decides the sequence.

Good APIs make both obvious and boring —
because boring APIs scale best.
