## Performance Tradeoffs

### 0. Why performance tradeoffs matter

Every API decision has a cost.

- Faster responses may reduce flexibility
- Simpler queries may increase server load
- Rich features may slow down large datasets

👉 **Performance is not about “fast vs slow”**  
👉 **It’s about choosing the right tradeoff for your use case**

Great APIs are not the fastest possible.
They are **predictably fast enough at scale**.

---

## PART 1 — WHAT “PERFORMANCE TRADEOFF” MEANS

---

### 1. Performance tradeoff 

👉 A **performance tradeoff** is:

Choosing one benefit while accepting a cost somewhere else.

Examples:
- Speed vs correctness
- Flexibility vs simplicity
- Real-time accuracy vs cache efficiency

You cannot optimize everything at once.

---

### 2. The golden rule

👉 **Optimize for the common case, not the edge case**

Most traffic:
- Reads > writes
- Small pages > full datasets
- Simple filters > complex queries

Design for reality.

---

## PART 2 — FILTERING TRADEOFFS

---

### 3. Filtering reduces payload, increases CPU

Filtering benefits:
- Smaller responses
- Less network usage
- Better UX

Filtering costs:
- More computation
- More database work
- More complex queries

Tradeoff:
👉 Less data transferred, more work per request

---

### 4. Filtering in database vs in memory

Bad (in-memory filtering):

    app.get('/users', (req, res) => {
      const activeUsers = users.filter(u => u.status === 'active');
      res.json(activeUsers);
    });

Problem:
- Loads everything
- Wastes memory
- Doesn’t scale

Good (DB-level filtering):
- Push filters as close to data as possible

Rule:
👉 Filter early, not late.

---

### 5. Too many filters = slow queries

Allowing unlimited filters:
- Creates unindexed queries
- Causes slow DB scans
- Hurts latency

Tradeoff decision:
- Support common filters
- Restrict exotic ones

Not every field needs filtering.

---

## PART 3 — SORTING TRADEOFFS

---

### 6. Sorting is expensive

Sorting costs:
- CPU
- Memory
- Index usage

Sorting large datasets without indexes is slow.

---

### 7. Sorting with indexes vs without

Indexed sort:
- Fast
- Predictable
- Scales well

Unindexed sort:
- Full scan
- Memory-heavy
- Slows dramatically

Tradeoff:
👉 Only allow sorting on indexed fields.

---

### 8. Multiple sort fields increase cost

Example:

    sort=role,-createdAt,name

Each extra field:
- Adds comparison work
- Increases query complexity

Tradeoff:
- More control for clients
- Slower execution

Best practice:
- Limit number of sortable fields
- Document allowed sort keys

---

## PART 4 — PAGINATION TRADEOFFS

---

### 9. Offset pagination tradeoffs

Benefits:
- Simple
- Page numbers
- Easy UI

Costs:
- Slow for large offsets
- Inconsistent when data changes

Tradeoff:
👉 Convenience vs scalability

---

### 10. Cursor pagination tradeoffs

Benefits:
- Fast at scale
- Stable results
- Handles inserts well

Costs:
- Harder UX
- No random page jumps
- More complex logic

Tradeoff:
👉 Scalability vs simplicity

---

### 11. Limiting page size 

Allowing:

    ?limit=10000

Is dangerous.

Large limits:
- Increase memory usage
- Increase response time
- Hurt all users

Best practice:
- Set max limit (e.g., 100)
- Enforce server-side

---

## PART 5 — CACHING TRADEOFFS

---

### 12. Caching improves speed, risks staleness

Caching benefits:
- Faster responses
- Lower server load
- Better scalability

Caching costs:
- Stale data
- Invalidation complexity
- Debugging difficulty

Tradeoff:
👉 Performance vs freshness

---

### 13. Short TTL vs long TTL

Short TTL:
- Fresher data
- More server hits

Long TTL:
- Faster responses
- Higher staleness risk

Most APIs choose:
👉 Short TTL + revalidation

---

### 14. Cacheable vs non-cacheable endpoints

Cache:
- Public GET endpoints
- Read-heavy resources

Do NOT cache:
- Auth responses
- User-specific data publicly
- Writes

Caching everything is as bad as caching nothing.

---

## PART 6 — FLEXIBILITY VS PERFORMANCE

---

### 15. “Powerful APIs” can be slow APIs

Allowing:
- Arbitrary filters
- Complex expressions
- Nested queries

Makes APIs:
- Hard to optimize
- Hard to index
- Hard to scale

Tradeoff:
👉 Less flexibility = more predictable performance

---

### 16. Restriction is a performance feature

Good APIs:
- Limit query complexity
- Restrict fields
- Enforce conventions

This is not “limiting users”.
It’s protecting the system.

---

## PART 7 — CLIENT VS SERVER WORK

---

### 17. Server-side vs client-side tradeoffs

Server-side:
- Centralized
- Consistent
- Scales better

Client-side:
- Faster iteration
- Less server logic
- More client responsibility

Rule:
👉 Heavy data work belongs on the server.

---

## PART 8 — REAL-WORLD PERFORMANCE PRIORITIES

---

### 18. What to optimize first

In order:
1. Correctness
2. Predictability
3. Simplicity
4. Performance
5. Micro-optimizations

Premature optimization kills APIs.

---

### 19. Measure before optimizing

Never guess.

Use:
- Metrics
- Logs
- Latency percentiles
- Load testing

Optimize what is actually slow.

---

## PART 9 — COMMON BEGINNER MISTAKES

---

### 20. Mistakes to avoid

- Optimizing everything at once
- Allowing unlimited filters/sorts
- Ignoring indexes
- Returning huge payloads
- Assuming “fast locally” means “fast at scale”

These mistakes surface painfully later.

---

## PART 10 — FINAL SUMMARY

---

### 21. Final definition 

**Performance tradeoffs** are conscious design decisions that balance speed, scalability, correctness, and flexibility—accepting certain costs in order to build APIs that remain predictable, stable, and usable as data and traffic grow.

---

### 22. Mental model to remember forever

Think like this:

- Every feature has a performance price
- Every optimization has a complexity cost

Great APIs don’t chase maximum speed.

They choose **sustainable speed**.
