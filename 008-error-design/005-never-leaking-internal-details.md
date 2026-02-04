## Never Leaking Internal Details

### 1. Start with a dangerous beginner mistake

Many beginners return errors like this:

    {
      "error": "TypeError: Cannot read property 'email' of undefined at UserService.js:42"
    }

Or worse:

    {
      "error": "SQLSTATE[23000]: Integrity constraint violation: 1062 Duplicate entry..."
    }

This feels “helpful”.
It is actually **dangerous**.

This is called **leaking internal details**.

---

## PART 1 — WHAT “LEAKING INTERNAL DETAILS” MEANS

---

### 2. What leaking internal details means 

👉 **Leaking internal details** means:

Exposing server-side implementation information to API clients that they should never see.

This includes:
- Stack traces
- File names
- Line numbers
- Database errors
- Library names
- Infrastructure details

Clients don’t need this.
Attackers love this.

---

### 3. Why this is a serious problem

Leaking internals causes:

- Security vulnerabilities
- Easier attacks
- Information disclosure
- Tight coupling to implementation
- Scary-looking UI errors

Even in private APIs, this is a bad habit.

---

### 4. The golden rule 

👉 **Clients should only see what they need to fix the request — nothing more**

Everything else belongs in:
- logs
- monitoring
- internal dashboards

Not in API responses.

---

## PART 2 — WHAT SHOULD NEVER BE LEAKED

---

### 5. Never leak stack traces

❌ Bad:

    {
      "error": "TypeError at auth.js:27"
    }

Why bad:
- Reveals file structure
- Reveals language/runtime
- Reveals code paths

Attackers use this to map your system.

---

### 6. Never leak database errors

❌ Bad:

    {
      "error": "Duplicate entry 'test@test.com' for key 'users_email_unique'"
    }

Why bad:
- Reveals table names
- Reveals column names
- Reveals constraints

This is sensitive internal design info.

---

### 7. Never leak library or framework details

❌ Bad:

    {
      "error": "SequelizeValidationError"
    }

❌ Bad:

    {
      "error": "MongoError: E11000 duplicate key error"
    }

Clients should not know or care which ORM or DB you use.

---

### 8. Never leak infrastructure details

❌ Bad:

    {
      "error": "ECONNREFUSED 10.0.0.5:5432"
    }

Why bad:
- Reveals IPs
- Reveals ports
- Reveals internal network layout

This is a security risk.

---

## PART 3 — WHAT CLIENTS ACTUALLY NEED

---

### 9. What clients really need from errors

Clients need:
- To know **that** something failed
- **Why** it failed (at a high level)
- **What to do next**

They do NOT need:
- how your server works
- what libraries you use
- where the bug occurred

---

### 10. Clean error response example 

    {
      "error": {
        "code": "EMAIL_ALREADY_EXISTS",
        "message": "An account with this email already exists"
      }
    }

This is:
- Safe
- Clear
- Actionable
- Stable

Perfect.

---

## PART 4 — ERROR VS LOGGING 

---

### 11. Error response vs server logs

This distinction is critical:

- **Response** → for clients
- **Logs** → for developers

Never mix them.

---

### 12. Correct pattern

When an error occurs:

1. Log the full internal error
2. Return a clean external error

Example:

    try {
      // business logic
    } catch (err) {
      console.error(err); // full details for devs

      res.status(500).json({
        error: {
          code: "INTERNAL_SERVER_ERROR",
          message: "Something went wrong"
        }
      });
    }

Clients stay safe.
Developers stay informed.

---

### 13. Express example: validation error without leaks

❌ Bad:

    {
      "error": "SequelizeValidationError: email cannot be null"
    }

✅ Good:

    {
      "error": {
        "code": "VALIDATION_ERROR",
        "message": "Email is required"
      }
    }

Translate internal errors into API-level errors.

---

## PART 5 — SECURITY IMPLICATIONS (WHY THIS REALLY MATTERS)

---

### 14. Leaked internals help attackers

Attackers use leaked details to:
- Identify frameworks
- Find known vulnerabilities
- Craft targeted attacks
- Bypass protections

Every leaked detail reduces your security margin.

---

### 15. OWASP explicitly warns about this

One of the most common security issues is:
👉 **Information disclosure**

Leaking internals is a textbook example.

Even “harmless” errors add up.

---

## PART 6 — DESIGNING SAFE ERROR MESSAGES

---

### 16. Good error messages are:

- High-level
- Non-technical
- Action-oriented
- Non-revealing

Example:
- “Invalid credentials”
- “Access denied”
- “Invalid request data”

Not:
- “JWT signature mismatch at auth.js”

---

### 17. Different environments, same rule

Some teams think:

> “It’s okay in development.”

This is risky.

Best practice:
- Same response shape everywhere
- Same non-leaky messages
- Only logging differs

Never conditionally leak internals.

---

### 18. Use error codes, not internals

Error codes give you:
- Precision
- Stability
- Safety

Instead of:
- exposing stack traces

Use:
- INTERNAL_SERVER_ERROR
- VALIDATION_ERROR
- AUTHENTICATION_FAILED

---

## PART 7 — COMMON BEGINNER MISTAKES

---

### 19. Common mistakes to avoid

- Returning raw exception messages
- Returning stack traces in JSON
- Using DB error text directly
- Logging nothing and exposing everything
- Assuming “internal API” means “safe to leak”

These mistakes come back to bite later.

---

## PART 8 — FINAL SUMMARY

---

### 20. Final definition 

**Never leaking internal details** means ensuring API responses expose only safe, high-level, client-relevant error information while keeping all implementation details—such as stack traces, database errors, and infrastructure data—strictly internal to logs and monitoring systems.

---

### 21. Mental model to remember forever

Think like this:

- API response = public press release
- Server logs = private investigation report

You never publish the investigation report.

Good APIs are calm, clean, and vague —
even when things go wrong.
