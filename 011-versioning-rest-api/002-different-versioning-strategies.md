## Different Versioning Strategies

### 0. What we are learning (context)

You already understand **why versioning is necessary**.

Now we answer the next practical question:

👉 **How do we actually version an API?**

There are **three common strategies**:

1. URI (path) versioning  
2. Header-based versioning  
3. Query-based versioning  

All three work.
But they have very different trade-offs.

---

## PART 1 — URI VERSIONING ( `/v1/users` )

---

### 1. What URI versioning is (plain English)

👉 **URI versioning** puts the API version directly in the URL path.

Example:

    /api/v1/users
    /api/v2/users

The version becomes part of the resource address.

---

### 2. Why URI versioning is the most popular

Because it is:

- Explicit
- Visible
- Easy to debug
- Easy to document
- Easy to route
- Easy for beginners

You can *see* the version immediately.

---

### 3. Express demo: URI versioning

    app.get('/api/v1/users', (req, res) => {
      res.json([{ id: 1, name: "Alex" }]);
    });

    app.get('/api/v2/users', (req, res) => {
      res.json([{ id: 1, fullName: "Alex" }]);
    });

Old clients call `/v1`
New clients call `/v2`

No conflict.
No breaking changes.

---

### 4. Pros of URI versioning

✅ Extremely clear  
✅ Easy for tooling (Postman, curl, browsers)  
✅ Simple routing logic  
✅ Works well with caching & CDNs  
✅ Widely used in industry  

This is why most public APIs use it.

---

### 5. Cons of URI versioning

❌ Version becomes part of URL forever  
❌ Same resource exists at multiple URLs  
❌ Purists argue it violates “pure REST”  

In practice, these cons are acceptable.

---

### 6. When URI versioning is the right choice

Use URI versioning when:

- You want simplicity
- You have public APIs
- You have many external clients
- You want low cognitive overhead

👉 **Best default choice**

---

## PART 2 — HEADER-BASED VERSIONING

---

### 7. What header-based versioning is

👉 **Header-based versioning** keeps the URL the same  
and sends the version in HTTP headers.

Example request:

    GET /users
    API-Version: 1

Or sometimes:

    Accept: application/vnd.myapi.v1+json

---

### 8. Express demo: header-based versioning

    app.get('/users', (req, res) => {
      const version = req.headers['api-version'];

      if (version === '2') {
        return res.json([{ id: 1, fullName: "Alex" }]);
      }

      res.json([{ id: 1, name: "Alex" }]);
    });

Same endpoint.
Different behavior.

---

### 9. Why some teams prefer this

Header-based versioning:

- Keeps URLs clean
- Treats version as metadata
- Feels more “REST-pure”
- Avoids multiple URLs for same resource

Architecturally elegant.

---

### 10. Pros of header-based versioning

✅ Clean URLs  
✅ Version is explicit but separate  
✅ Aligns with HTTP semantics  
✅ No URL explosion  

---

### 11. Cons of header-based versioning

❌ Harder to debug  
❌ Invisible in browser address bar  
❌ Harder for caching & CDNs  
❌ Tooling support is weaker  
❌ Clients must know to set headers  

This increases friction.

---

### 12. When header-based versioning makes sense

Use header-based versioning when:

- You control all clients
- You have strong API governance
- You want clean URLs
- You are comfortable with advanced HTTP usage

👉 Better for internal or controlled ecosystems.

---

## PART 3 — QUERY-BASED VERSIONING

---

### 13. What query-based versioning is

👉 **Query-based versioning** puts the version in query params.

Example:

    /users?version=1
    /users?version=2

The resource stays the same.
Behavior changes based on query.

---

### 14. Express demo: query-based versioning

    app.get('/users', (req, res) => {
      const version = req.query.version;

      if (version === '2') {
        return res.json([{ id: 1, fullName: "Alex" }]);
      }

      res.json([{ id: 1, name: "Alex" }]);
    });

---

### 15. Pros of query-based versioning

✅ Easy to test  
✅ Easy to experiment  
✅ No headers required  
✅ Simple for quick demos  

---

### 16. Cons of query-based versioning 

❌ Versioning feels optional  
❌ Easy to forget  
❌ Poor cache behavior  
❌ Not semantically correct  
❌ Often abused  

Most production APIs avoid this.

---

### 17. When query-based versioning is acceptable

Use it only for:

- Experiments
- Prototypes
- Temporary transitions
- Internal testing

👉 **Not recommended for long-term public APIs**

---

## PART 4 — SIDE-BY-SIDE COMPARISON

---

### 18. Quick comparison table 

URI versioning:
- Visible
- Simple
- Industry standard

Header versioning:
- Clean URLs
- More complex
- Better for controlled clients

Query versioning:
- Easy
- Weak guarantees
- Not ideal for production

---

### 19. Caching & CDN implications 

URI versioning:
- Different URLs → separate cache entries
- CDN-friendly

Header versioning:
- Requires header-aware caching
- More configuration

Query versioning:
- Often breaks cache efficiency

This matters at scale.

---

## PART 5 — COMMON BEGINNER MISTAKES

---

### 20. Mistakes to avoid

- Mixing multiple versioning strategies
- Changing behavior without bumping version
- Versioning non-breaking changes
- Forgetting to document versions
- Supporting infinite versions forever

Discipline matters.

---

## PART 6 — WHAT MOST REAL SYSTEMS DO

---

### 21. Industry reality

Most real-world APIs choose:

👉 **URI versioning for public APIs**  
👉 **Header versioning for internal APIs**

Query-based versioning is rare in mature systems.

---

## PART 7 — FINAL SUMMARY

---

### 22. Final definitions 

- **URI versioning** embeds the version in the URL path and is the most explicit and widely adopted approach  
- **Header-based versioning** keeps URLs clean by sending version metadata in HTTP headers  
- **Query-based versioning** uses query parameters and is generally discouraged for long-term API design  

---

### 23. Mental model to remember forever

Think like this:

- URI versioning → different doors
- Header versioning → same door, different badge
- Query versioning → sticky note on the door

Professional APIs choose the door strategy wisely.
