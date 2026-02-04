## Versioning in REST APIs

### 1. Start with the real-world problem

Imagine this situation:

- You have a REST API used by:
  - Web app
  - Mobile app
  - Third-party clients
- Everything works fine
- Now you need to:
  - Rename a field
  - Change response shape
  - Add stricter validation
  - Remove deprecated behavior

You deploy the change…

👉 **Clients break immediately**

This is the core problem versioning solves.

---

### 2. What API versioning means

👉 **API versioning** means:

Providing a controlled way to evolve an API **without breaking existing clients**.

In simple words:

Versioning lets old clients keep working  
while new clients move forward.

---

### 3. The key insight 

👉 **APIs are contracts**

Once a client depends on:
- a field name
- a response shape
- a behavior

You cannot change it freely.

Versioning is how you change contracts **safely**.

---

## PART 1 — WHY VERSIONING IS NECESSARY

---

### 4. APIs live longer than code

Frontend code:
- Can be redeployed instantly

Mobile apps:
- Stay installed for months
- Users don’t update immediately

Third-party integrations:
- You don’t control them at all

👉 Your API must support **multiple generations of clients**.

---

### 5. Backward compatibility is hard

Some changes are backward compatible:
- Adding optional fields
- Adding new endpoints

Some are NOT:
- Renaming fields
- Removing fields
- Changing data types
- Changing validation rules
- Changing semantics

Versioning exists for **breaking changes**.

---

### 6. Without versioning, you have only bad options

If you don’t version:

1. Never change anything (API stagnates)
2. Break clients silently (angry users)
3. Add hacks and flags everywhere (technical debt)

All three are bad.

---

## PART 2 — WHAT HAPPENS WITHOUT VERSIONING

---

### 7. Silent breaking changes 

Example:

Old response:
    {
      "id": 1,
      "name": "Alex"
    }

New response:
    {
      "id": 1,
      "fullName": "Alex"
    }

Old clients:
- Expect `name`
- Crash or behave incorrectly

No warning.
No control.
No rollback.

---

### 8. Tight coupling between teams

Without versioning:
- Backend can’t evolve
- Frontend is blocked
- Mobile teams panic
- Third-party clients complain

Versioning restores independence.

---

### 9. Fear-driven development

Teams stop improving APIs because:
- “What if this breaks someone?”

This leads to:
- Messy APIs
- Legacy behavior forever
- Inconsistent design

Versioning gives confidence to improve.

---

## PART 3 — WHAT VERSIONING ENABLES

---

### 10. Safe evolution

With versioning, you can:
- Redesign endpoints
- Clean up mistakes
- Improve naming
- Fix bad decisions

Without harming existing users.

---

### 11. Gradual migration

Clients can:
- Stay on old version
- Migrate when ready
- Test new version safely

This is critical for:
- Mobile apps
- External consumers

---

### 12. Clear communication

Versioning makes change explicit:

- “You are using v1”
- “v2 behaves differently”
- “v1 will be deprecated later”

No surprises.

---

## PART 4 — REAL EXAMPLES OF BREAKING CHANGES

---

### 13. Field changes

Breaking:
- Renaming fields
- Removing fields
- Changing required → optional (sometimes ok)
- Changing optional → required (breaking)

Version required.

---

### 14. Behavior changes

Breaking:
- Different status codes
- Different error shapes
- Different validation rules
- Different sorting / filtering defaults

Clients rely on behavior, not just structure.

---

### 15. Semantic changes 

Example:
- `GET /orders` used to return only active orders
- Now returns all orders

Even if structure is same,
meaning changed → breaking change.

Versioning protects against this.

---

## PART 5 — VERSIONING IN PRACTICE 

---

### 16. URL-based versioning

Example:

    GET /api/v1/users
    GET /api/v2/users

Express setup:

    app.get('/api/v1/users', (req, res) => {
      res.json([{ id: 1, name: "Alex" }]);
    });

    app.get('/api/v2/users', (req, res) => {
      res.json([{ id: 1, fullName: "Alex" }]);
    });

Old clients keep working.
New clients opt in.

---

### 17. Why this works well

- Very explicit
- Easy to debug
- Easy to route
- Easy to document

That’s why it’s most popular.

---

## PART 6 — WHAT VERSIONING IS NOT

---

### 18. Versioning is NOT for every change

Do NOT version for:
- Bug fixes
- Performance improvements
- Adding optional fields
- Adding new endpoints

Version only when **breaking**.

---

### 19. Versioning is NOT about perfection

You will still make mistakes.

Versioning gives you:
- An escape hatch
- A safety net
- A recovery mechanism

That’s enough.

---

## PART 7 — COMMON BEGINNER MISTAKES

---

### 20. Mistakes to avoid

- Never version at all
- Version every tiny change
- Breaking behavior inside same version
- No deprecation strategy
- No documentation

Versioning without discipline is useless.

---

## PART 8 — VERSIONING & REST

---

### 21. REST doesn’t forbid versioning

REST does NOT say:
- “Never version APIs”

REST says:
- Respect contracts
- Be explicit
- Don’t surprise clients

Versioning supports REST goals.

---

### 22. Versioning is about respect

You are respecting:
- Client stability
- Integration partners
- User experience

Breaking APIs casually is disrespectful.

---

## PART 9 — FINAL SUMMARY

---

### 23. Final definition

**API versioning is necessary** because APIs are long-lived contracts, and versioning provides a controlled, explicit way to introduce breaking changes while preserving backward compatibility and allowing clients to migrate safely.

---

### 24. Mental model to remember forever

Think like this:

- API = public promise
- Breaking change = broken promise
- Versioning = making a new promise without breaking the old one

Good APIs don’t surprise clients.

They evolve **with care**.
