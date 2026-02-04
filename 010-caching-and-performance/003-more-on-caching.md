## More on Caching

### 1. Start with the big picture

When we talk about caching in REST APIs, we are really asking four questions:

1. Where is the cache located?
2. Who controls it?
3. When should it NOT be used?
4. How do we keep cached data correct?

These questions lead to:
- Client-side caching
- Proxy / CDN caching
- Cache invalidation strategies

Let’s break this down properly, from noob → advanced.

---

## PART 1 — CLIENT-SIDE VS PROXY CACHING

---

### 2. Client-side caching

👉 **Client-side caching** means:

The cache lives **inside the client**.

Examples:
- Browser cache
- Mobile app cache
- Desktop app cache

The client stores responses and reuses them later.

---

### 3. How client-side caching works

Flow:
1. Client requests resource
2. Server sends response + cache headers
3. Client stores the response
4. Future requests may be served from cache

Example header:
    Cache-Control: private, max-age=60

This means:
- Only the client can cache it
- Valid for 60 seconds

---

### 4. What client-side caching is good for

Client-side caching excels at:
- Reducing repeated API calls
- Faster UI interactions
- Offline or poor network conditions
- Personalized data

Example use cases:
- User profile
- Settings
- Dashboard data

---

### 5. Client-side caching limitations

Client-side caching:
- Only helps that one user
- Does not reduce load for other users
- Cannot serve shared data globally

It improves UX, not global scalability.

---

## PART 2 — PROXY / SHARED CACHING

---

### 6. What proxy caching means

👉 **Proxy caching** means:

The cache lives **between the client and the server**.

Examples:
- Reverse proxies
- API gateways
- CDNs

These caches serve **many clients**.

---

### 7. How proxy caching works

Flow:
Client → Proxy/Cache → Server

If cached:
- Proxy returns response
- Server is never hit

This is extremely powerful.

---

### 8. Cache-Control rules for proxies

Key directives:

- `public` → shared caches allowed
- `private` → shared caches forbidden

Example:
    Cache-Control: public, max-age=120

This allows:
- Browser cache
- Proxy cache
- CDN cache

---

### 9. What proxy caching is good for

Proxy caching is ideal for:
- Public data
- Read-heavy APIs
- Global traffic
- Performance at scale

Examples:
- Product catalogs
- Blog posts
- Public profiles
- Configuration endpoints

---

### 10. Why proxy caching scales systems

Without proxy caching:
- Every client hits your server

With proxy caching:
- Millions of users hit cache
- Server handles only cache misses

This is how large systems survive.

---

## PART 3 — WHEN NOT TO CACHE 

---

### 11. Caching is powerful — but dangerous if misused

Not everything should be cached.

Caching the wrong thing can cause:
- Security leaks
- Stale data bugs
- User confusion

---

### 12. Never cache sensitive data

❌ Do NOT cache:
- Authentication responses
- Tokens
- Password-related endpoints
- Financial transactions

Example:
    Cache-Control: no-store

This ensures:
- No browser cache
- No proxy cache
- No disk storage

---

### 13. Never cache user-specific data publicly

❌ Bad:
    Cache-Control: public
    (on user profile)

This can leak data between users.

✅ Correct:
    Cache-Control: private, max-age=60

User-specific data must never enter shared caches.

---

### 14. Avoid caching frequently changing data

Data that changes very often:
- Live metrics
- Real-time notifications
- Counters

Caching here:
- Adds complexity
- Gives little benefit

Sometimes fresh > fast.

---

### 15. Never cache unsafe HTTP methods

By default:
- GET → cacheable
- POST / PUT / PATCH / DELETE → NOT cacheable

Never try to cache writes.
It breaks correctness.

---

## PART 4 — CACHE INVALIDATION STRATEGIES 

---

### 16. What cache invalidation means

👉 **Cache invalidation** means:

Deciding when cached data is no longer valid and must be refreshed.

This is famously hard — but manageable.

---

### 17. Strategy #1: Time-based expiration (TTL)

Most common strategy.

Example:
    Cache-Control: public, max-age=60

Meaning:
- Cache is valid for 60 seconds
- After that → revalidate or refetch

Simple.
Safe.
Effective.

---

### 18. Strategy #2: Conditional revalidation (ETag / Last-Modified)

Instead of deleting cache:
- Ask the server if it changed

Server replies:
- 304 Not Modified → reuse cache
- 200 OK → update cache

Efficient and accurate.

---

### 19. Strategy #3: Write-through invalidation

When data changes:
- Invalidate related cache entries

Example:
- User updated
- Invalidate `/users/123` cache

Common in:
- APIs with strong consistency needs

More complex, but precise.

---

### 20. Strategy #4: Versioned URLs

Instead of invalidating cache:
- Change the URL

Example:
- /config?v=1
- /config?v=2

New URL = new cache entry.

Used for:
- Static assets
- Config files

---

### 21. Why TTL + revalidation is the sweet spot

Most APIs combine:
- Short TTL
- Conditional requests

This gives:
- Fresh data
- Reduced load
- Simple logic

You rarely need perfect invalidation.

---

## PART 5 — CDN ROLE IN REST APIs

---

### 22. What a CDN is

👉 A **CDN (Content Delivery Network)** is:

A globally distributed proxy cache that sits close to users.

It caches responses and serves them from nearby locations.

---

### 23. Where CDN fits in REST architecture

Flow:
Client → CDN → API Gateway → Server

The CDN:
- Is invisible to clients
- Respects HTTP headers
- Acts as a REST layer

This aligns perfectly with REST’s layered system constraint.

---

### 24. What CDNs cache in REST APIs

CDNs typically cache:
- GET responses
- Public resources
- JSON APIs
- Media assets

As long as:
- Cache-Control allows it

---

### 25. Benefits of CDN caching

CDNs provide:
- Lower latency
- Global scalability
- DDoS protection
- Reduced origin load

This is not optional at scale.

---

### 26. Express example: CDN-friendly endpoint

    app.get('/products', (req, res) => {
      res.set('Cache-Control', 'public, max-age=120');
      res.json(products);
    });

CDN behavior:
- Cache response
- Serve globally
- Hit origin only after expiration

---

### 27. CDN + REST = internet scale

REST APIs designed with:
- Proper cache headers
- Statelessness
- Uniform interface

Are instantly CDN-compatible.

No extra work needed.

---

## PART 6 — COMMON BEGINNER MISTAKES

---

### 28. Mistakes to avoid

- Treating all caching as client-side
- Using public cache for private data
- Disabling caching everywhere
- Expecting perfect cache invalidation
- Ignoring CDN capabilities

These mistakes block scalability.

---

## PART 7 — FINAL SUMMARY

---

### 29. Key takeaways 

- Client-side caching improves UX
- Proxy & CDN caching improves scalability
- Some data must never be cached
- Cache invalidation is about trade-offs
- CDNs are first-class REST citizens

---

### 30. Final definition 

**Client-side caching** optimizes individual user experience, **proxy and CDN caching** enable system-wide scalability, **cache invalidation strategies** keep data reasonably fresh, and **CDNs** play a critical role in making REST APIs fast, resilient, and globally scalable.

---

### 31. Mental model to remember forever

Think like this:

- Client cache → personal notebook
- Proxy cache → shared library
- CDN → libraries around the world

You don’t rewrite the book every time.
You decide **when** it needs updating.

That’s caching done right.
