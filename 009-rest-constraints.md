## REST Constraints

### 0. Why this topic matters so much

Many people say:
> “I built a REST API”

But in reality:
- They just used HTTP
- They just returned JSON
- They violated REST principles everywhere

👉 **REST is not a technology**
👉 **REST is a set of architectural constraints**

If you violate these constraints,
your API may work — but it is **not REST**.

---

## PART 1 — WHAT REST REALLY IS

---

### 1. What REST actually means 

👉 **REST** stands for **Representational State Transfer**

In simple words:

REST is a set of rules that define **how clients and servers should communicate** in a scalable, reliable, and predictable way.

These rules are called **constraints**.

---

### 2. Why REST has constraints

Constraints exist to ensure:
- Scalability
- Simplicity
- Evolvability
- Reliability

REST trades:
- some flexibility  
for:
- massive long-term benefits

---

### 3. REST constraints are NOT optional

This is critical:

👉 If you violate REST constraints,  
👉 your API is **no longer REST**

You may still use HTTP.
But it’s not RESTful.

---

## PART 2 — THE REST CONSTRAINTS 

---

### 4. What client–server separation means

👉 Client and server must be **independent**

- Client handles UI & user interaction
- Server handles data & business logic

They communicate **only via the API**

---

### 5. Why this constraint exists

This separation allows:
- Independent development
- Independent scaling
- Independent deployment

Frontend and backend don’t block each other.

---

### 6. Example 

Server:

    app.get('/users/:id', (req, res) => {
      res.json({ id: 1, name: "Alex" });
    });

Client:
- Doesn’t know DB
- Doesn’t know logic
- Only knows API contract

✅ REST-compliant

---

### 7. Violation example

❌ Frontend directly querying database  
❌ Backend returning HTML + UI logic  
❌ Client depending on DB schema  

This breaks REST.

---

### 8. What statelessness means

👉 Every request must contain **all information needed** to process it.

Server must NOT remember:
- previous requests
- client session state

---

### 9. Why statelessness exists

Statelessness enables:
- Horizontal scaling
- Load balancing
- Reliability
- Simpler servers

---

### 10. Example 

    GET /users/1
    Authorization: Bearer token123

Everything needed is in the request.

---

### 11. Violation example (bad)

❌ Server stores user progress in memory  
❌ Request depends on previous request  

This breaks REST and scaling.

---

### 12. What cacheability means

👉 Responses must define **whether they can be cached**

Clients, proxies, and CDNs rely on this.

---

### 13. Why cacheability exists

Caching:
- Improves performance
- Reduces server load
- Improves scalability

---

### 14. Example 

    app.get('/users', (req, res) => {
      res.set('Cache-Control', 'public, max-age=60');
      res.json(users);
    });

This response:
- Can be cached for 60 seconds

---

### 15. Violation example 

❌ No cache headers at all  
❌ Clients guessing cache behavior  

This breaks REST efficiency.

---

## 8.4 Uniform Interface 

---

### 16. What uniform interface means

👉 All clients interact with the server in a **consistent, standardized way**

This includes:
- Resource-based URLs
- HTTP methods
- Status codes
- Representations (JSON)

---

### 17. Uniform interface rules

- URLs identify **resources**
- HTTP methods define **actions**
- Status codes define **outcome**
- JSON defines **representation**

---

### 18. Example

    GET    /users/1
    POST   /users
    PATCH  /users/1
    DELETE /users/1

Clean.
Predictable.
RESTful.

---

### 19. Violation example 

❌ GET /getUser  
❌ POST /deleteUser  
❌ PUT /updateUserData  

This turns APIs into RPC.
REST is broken.

---

### 20. What layered system means

👉 Client must NOT know whether it’s talking to:
- API server
- Load balancer
- Cache
- Proxy
- Microservice

Layers are invisible.

---

### 21. Why layered systems exist

Layering enables:
- Security layers
- Caching layers
- Load balancing
- API gateways

Without client changes.

---

### 22. Example

Client → CDN → API Gateway → Service → Database

Client doesn’t care.
REST allows this.

---

### 23. Violation example

❌ Client tightly coupled to internal services  
❌ Client aware of microservice boundaries  

This breaks flexibility.

---

### 24. What code-on-demand means

👉 Server can send executable code to the client.

Example:
- JavaScript sent to browser

This is:
- Optional
- Rare
- Mostly used in browsers

---

### 25. Why it’s optional

REST does NOT require this.
Most APIs never use it.

So:
- REST without code-on-demand is still REST

---

### 26. Example 

Server sends JS:
    function validateForm() { ... }

Client executes it.

Optional feature.

---

## PART 3 — WHY VIOLATING CONSTRAINTS BREAKS REST

---

### 27. REST constraints work together

Each constraint supports the others.

Violating one causes:
- Tight coupling
- Poor scalability
- Fragile systems

---

### 28. Common violations and consequences

Violation → Consequence

- Stateful server → scaling issues  
- No cacheability → performance issues  
- No uniform interface → unreadable APIs  
- Tight coupling → slow teams  

---

### 29. REST without constraints = HTTP RPC

Most “REST APIs” are actually:
- HTTP + JSON
- RPC-style endpoints

They work — but they lose REST benefits.

---

### 30. REST is about long-term system health

REST is not about:
- Faster coding
- Fewer lines

REST is about:
- APIs that survive years
- Teams that scale
- Systems that evolve

---

## PART 4 — FINAL SUMMARY

---

### 31. REST constraints recap

- Client–server separation
- Statelessness
- Cacheability
- Uniform interface
- Layered system
- Code-on-demand (optional)

These define REST.

---

### 32. Final definition 

**REST constraints** are architectural rules that define how clients and servers must interact to achieve scalability, simplicity, and evolvability—violating these constraints means the system is no longer truly RESTful.

---

### 33. Mental model to remember forever

Think like this:

- REST is a constitution
- Constraints are laws
- You can’t “partially follow” laws

Follow them all → REST  
Break them → not REST  

Once you see REST this way,
you’ll never misuse the term again.
