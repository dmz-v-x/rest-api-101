## HTTP Cache Headers

### 1. Start with the core caching question

When a client requests data, the server must answer:

👉 “Can this response be reused later, or must it always be fresh?”

HTTP cache headers exist **only** to answer this question.

The three most important ones are:

- Cache-Control
- ETag
- Last-Modified

If you understand these three, you understand **HTTP caching**.

---

## PART 1 — Cache-Control 

---

### 2. What Cache-Control is 

👉 **Cache-Control** is a response header that tells clients and intermediaries:

- Whether a response can be cached
- Who can cache it
- For how long it can be cached

This header is the **primary caching rulebook**.

---

### 3. Why Cache-Control matters most

Without Cache-Control:
- Browsers guess
- Proxies guess
- CDNs guess

Guessing leads to:
- Stale data
- Missed caching opportunities
- Bugs

Cache-Control removes ambiguity.

---

### 4. The most important Cache-Control directives

Here are the ones you must know:

- `public`
- `private`
- `no-cache`
- `no-store`
- `max-age`

You don’t need all of HTTP.
Just these cover 90% of cases.

---

### 5. public vs private

`public`:
- Can be cached by browsers, proxies, CDNs
- Used for public data

Example:
    Cache-Control: public, max-age=60

`private`:
- Can be cached only by the browser
- Not by shared caches

Example:
    Cache-Control: private, max-age=60

Use `private` for user-specific data.

---

### 6. max-age 

`max-age` defines freshness duration (in seconds).

Example:
    Cache-Control: public, max-age=60

Meaning:
👉 “This response is fresh for 60 seconds.”

During this time:
- No server request
- Cache serves response directly

---

### 7. no-store 

`no-store` means:

👉 “Do not cache this response anywhere.”

Used for:
- Authentication responses
- Passwords
- Sensitive data

Example:
    Cache-Control: no-store

This disables caching completely.

---

### 8. no-cache

`no-cache` does NOT mean “don’t cache”.

It means:
👉 “You may cache, but you must revalidate before using it.”

Example:
    Cache-Control: no-cache

This works together with ETag or Last-Modified.

---

### 9. Express example: Cache-Control

    app.get('/users', (req, res) => {
      res.set('Cache-Control', 'public, max-age=60');
      res.json(users);
    });

This single line enables caching everywhere.

---

## PART 2 — ETag 

---

### 10. What ETag is 

👉 **ETag** is a unique identifier representing the **version of a response**.

Think of it as:
- A fingerprint of the response body

If the fingerprint hasn’t changed,
the content hasn’t changed.

---

### 11. Why ETag exists

Instead of sending full data again,
the server can say:

👉 “Nothing changed. Use what you already have.”

This saves:
- Bandwidth
- CPU
- Time

---

### 12. How ETag works 

1. Client requests resource
2. Server responds with data + ETag
3. Client stores both
4. Client requests again with ETag
5. Server compares
6. Server replies:
   - 304 Not Modified (if same)
   - 200 OK (if changed)

---

### 13. Example response with ETag

Response:
    Status: 200 OK
    ETag: "abc123"

Client stores:
- Response body
- ETag value

---

### 14. Conditional request using ETag

Next request:

    GET /users/1
    If-None-Match: "abc123"

If unchanged:
- Server returns 304
- No body sent

Huge performance win.

---

### 15. Express example: ETag

Express supports ETag automatically.

    app.set('etag', 'strong');

    app.get('/users/:id', (req, res) => {
      res.json({ id: 1, name: "Alex" });
    });

Express handles:
- ETag generation
- 304 responses

You get caching almost for free.

---

## PART 3 — Last-Modified 

---

### 16. What Last-Modified is 

👉 **Last-Modified** tells the client:

“When this resource was last changed.”

Example:
    Last-Modified: Tue, 03 Feb 2026 10:30:00 GMT

This is time-based validation.

---

### 17. Why Last-Modified exists

Some systems:
- Can’t compute content hashes easily
- Track update timestamps naturally

Last-Modified works well in those cases.

---

### 18. How Last-Modified works

1. Server sends Last-Modified
2. Client stores it
3. Client sends it back in next request
4. Server checks:
   - Has it changed since then?

---

### 19. Conditional request using Last-Modified

Client sends:

    GET /users/1
    If-Modified-Since: Tue, 03 Feb 2026 10:30:00 GMT

If unchanged:
- Server returns 304
- No body

---

### 20. Express example: Last-Modified

    app.get('/users/:id', (req, res) => {
      res.set('Last-Modified', new Date().toUTCString());
      res.json({ id: 1, name: "Alex" });
    });

Simple, but effective.

---

## PART 4 — ETAG VS LAST-MODIFIED 

---

### 21. Key differences

ETag:
- Content-based
- More precise
- Detects any change

Last-Modified:
- Time-based
- Less precise
- May miss rapid updates

If both exist:
👉 ETag wins.

---

### 22. When to use which

Use ETag when:
- You want accuracy
- Content changes matter

Use Last-Modified when:
- You already track timestamps
- Precision is less critical

Many APIs use both.

---

## PART 5 — HOW THEY WORK TOGETHER

---

### 23. Typical modern caching setup

Response headers:
    Cache-Control: public, max-age=60
    ETag: "abc123"
    Last-Modified: Tue, 03 Feb 2026 10:30:00 GMT

Behavior:
- Fresh for 60 seconds
- After that, revalidate
- Use ETag or Last-Modified

This is ideal REST caching.

---

### 24. 304 Not Modified 

304 means:
- Resource unchanged
- Client should use cached copy
- No response body

This is a **successful response**, not an error.

---

## PART 6 — COMMON BEGINNER MISTAKES

---

### 25. Mistakes to avoid

- Not setting Cache-Control at all
- Using no-store everywhere
- Misunderstanding no-cache
- Ignoring conditional requests
- Re-sending full data unnecessarily

These kill caching benefits.

---

## PART 7 — WHY THIS MATTERS FOR REST

---

### 26. REST assumes caching

Cacheability is a REST constraint.

If you ignore:
- Cache-Control
- ETag
- Last-Modified

You are violating REST principles.

---

### 27. Caching = scalability

Correct caching:
- Reduces server load
- Improves performance
- Saves money
- Improves UX

All from headers alone.

---

## PART 8 — FINAL SUMMARY

---

### 28. Final definitions 

- **Cache-Control**: Defines whether and how long a response can be cached  
- **ETag**: A content-based identifier used for precise cache validation  
- **Last-Modified**: A timestamp indicating when a resource last changed  

Together, they form the backbone of HTTP caching.

---

### 29. Mental model to remember forever

Think like this:

- Cache-Control → “How long can I trust this?”
- ETag → “Did the content change?”
- Last-Modified → “When did it change?”

If all three agree,
you never need to ask the server again.

That’s the power of HTTP caching.
