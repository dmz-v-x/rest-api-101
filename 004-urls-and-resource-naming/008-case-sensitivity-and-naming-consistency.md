## Case Sensitivity & Naming Consistency

### 1. Start with a very small but painful problem

As a beginner, you might write APIs like this:

- /Users
- /getUsers
- /get_users
- /Getusers
- /userProfile
- /user-profile

Everything “works”… until it doesn’t.

Suddenly:
- One URL works, another returns 404
- Frontend breaks randomly
- Team members are confused

This is where **case sensitivity & naming consistency** become critical.

---

### 2. What case sensitivity means

👉 **Case sensitivity** means:

Whether the system treats uppercase and lowercase letters as **different**.

Example:

- /users  
- /Users  

Are these the same?

👉 In URLs and REST APIs: **NO**  
They are treated as **different paths**.

---

### 3. URLs ARE case-sensitive 

Most web servers and frameworks treat URLs as:

👉 **Case-sensitive (especially paths)**

So:

- GET /users  ✅
- GET /Users  ❌ (404 Not Found)

This alone causes many beginner bugs.

---

### 4. Why case sensitivity causes real bugs

Imagine this:

Backend defines:
- /users

Frontend calls:
- /Users

Result:
- Works in dev?
- Fails in prod?
- Random 404 errors?

These bugs are:
- Hard to spot
- Easy to create
- Very frustrating

---

### 5. Express example: case sensitivity in action

Express route:

    app.get('/users', (req, res) => {
      res.send('Users list');
    });

Requests:

- GET /users  → works
- GET /Users  → 404
- GET /USERS  → 404

Express does **exact matching** by default.

---

### 6. So what should we do?

👉 **Always use lowercase URLs**

This is the industry standard.

Examples:

- /users
- /orders
- /products
- /user-profiles

Never mix uppercase in URLs.

---

### 7. Naming consistency

👉 **Naming consistency** means:

Using the **same naming style everywhere**, without mixing patterns.

Consistency matters more than “what style you choose”.

---

### 8. Common naming styles you’ll see

Here are common styles people mix (bad idea):

- camelCase → userProfile
- snake_case → user_profile
- kebab-case → user-profile
- PascalCase → UserProfile

Mixing these leads to chaos.

---

### 9. The recommended REST naming style

👉 **Use lowercase + kebab-case**

Why kebab-case?

- Easy to read
- URL-friendly
- Industry-standard
- No ambiguity

Examples:

- /users
- /user-profiles
- /order-items
- /password-resets

---

### 10. Why NOT camelCase in URLs

camelCase ❌:

- /userProfile
- /orderItems

Problems:
- Harder to read
- Case-sensitive bugs
- Looks like code, not a resource

camelCase is great for:
- JavaScript variables
- JSON keys

Not URLs.

---

### 11. Why NOT snake_case in URLs

snake_case ❌:

- /user_profile
- /order_items

Problems:
- Underscores are harder to read in URLs
- Less common in public APIs

Kebab-case is visually clearer.

---

### 12. Consistency across the entire API

Bad API ❌:

- /users
- /userProfile
- /order_items
- /GetOrders

Good API ✅:

- /users
- /user-profiles
- /order-items
- /orders

One style.
Everywhere.

---

### 13. Naming consistency applies beyond URLs

Consistency should apply to:

- URL paths
- Query params
- JSON keys
- Header names (when custom)

Example:

❌ Mixed:
- /users?PageSize=10
- /users?per_page=10

✅ Consistent:
- /users?page=1&limit=10

---

### 14. Query params should also be lowercase

Best practice:

- ?page=2
- ?limit=10
- ?sort=createdAt (camelCase OK inside values)

Avoid:
- ?Page=2
- ?LIMIT=10

Lowercase = predictable.

---

### 15. JSON vs URL naming 

It’s OK to have:

- URLs → kebab-case
- JSON → camelCase

Example:

URL:
- POST /user-profiles

JSON body:
    {
      "firstName": "Alex",
      "lastName": "Smith"
    }

Different layers.
Different conventions.
This is normal.

---

### 16. Case sensitivity and caching 

Caching systems treat these as different:

- /users
- /Users

That means:
- Cache misses
- Duplicate cache entries
- Wasted memory

Consistent lowercase URLs improve caching.

---

### 17. Team & scaling problems

In teams:

- One dev uses /Users
- Another uses /users
- Docs say /user

Result:
- Bugs
- Confusion
- Slow onboarding

Consistency is a **team productivity tool**.

---

### 18. Common beginner mistakes

- Mixing case randomly
- Using camelCase in URLs
- Copying function names into routes
- Inconsistent query param names
- Fixing bugs instead of preventing them

---

### 19. Simple rules to follow

Follow these rules and you’re safe:

1. Always use lowercase URLs  
2. Use kebab-case for paths  
3. Be consistent everywhere  
4. Never mix naming styles  
5. Treat URLs as public contracts  

---

### 20. Final definition

Case sensitivity and naming consistency in REST APIs mean using predictable, lowercase, and uniform naming conventions (typically kebab-case) across all URLs and parameters to avoid bugs, improve readability, and ensure long-term maintainability.

---

### 21. Mental model to remember forever

Think like this:

- URLs are street names
- They must be:
  - lowercase
  - clearly written
  - consistently named

If street names randomly change case and style,
people get lost.

APIs work the same way.
Consistency keeps everyone on the right path.
