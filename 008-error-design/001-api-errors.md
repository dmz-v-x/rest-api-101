## Api Errors

### 1. Start with the biggest misconception

Many beginners think:

> “An error means the server crashed.”

This is **wrong**.

In APIs:
- Most errors are **expected**
- Many errors are **part of normal operation**

Understanding what an API error really is will completely change how you design APIs.

---

## PART 1 — WHAT AN API ERROR REALLY IS

---

### 2. What an API error is 

👉 An **API error** is:

A situation where the server **cannot successfully process a request as sent**, and must tell the client **what went wrong**.

Important:
- The server is still working
- The API is still functioning
- The request just failed

Errors are communication, not failure.

---

### 3. Errors are part of the API contract

Errors are not accidents.

They are:
- Defined
- Predictable
- Documented

Examples of normal API errors:
- Invalid input
- Missing authentication
- Resource not found
- Permission denied

These are expected outcomes.

---

### 4. What errors communicate

An API error communicates:
- That the request failed
- Why it failed
- What the client should do next

A good error helps the client **recover**.

---

## PART 2 — ERROR VS EXCEPTION 

---

### 5. What an error is

👉 An **error** is:

A known, expected problem caused by **invalid input or client behavior**.

Examples:
- Email missing
- Invalid ID
- Unauthorized access

Errors are:
- Anticipated
- Handled gracefully
- Returned as 4xx responses

---

### 6. What an exception is

👉 An **exception** is:

An unexpected problem caused by **bugs or system failures**.

Examples:
- Null reference
- Database connection failure
- Crash in business logic

Exceptions are:
- Unplanned
- Indicate server bugs
- Returned as 5xx responses

---

### 7. Why this distinction matters

Errors:
- Are part of API design
- Should be handled cleanly
- Should be documented

Exceptions:
- Are bugs
- Should be logged
- Should be fixed by developers

Clients should never see raw exceptions.

---

### 8. Error vs exception 

Error:
- Client mistake
- Expected
- 4xx status
- Clear message

Exception:
- Server mistake
- Unexpected
- 5xx status
- Minimal message

This separation is critical.

---

## PART 3 — CONSISTENT ERROR RESPONSE SHAPE

---

### 9. Why consistency matters for errors

Clients rely on:
- Predictable structure
- Known fields
- Stable formats

If error shapes vary:
- Frontend code becomes messy
- Error handling becomes fragile

Consistency is non-negotiable.

---

### 10. The golden rule for error responses

👉 **Every error response must follow the same JSON structure**

No exceptions.

---

### 11. A recommended standard error shape

Example:

    {
      "error": {
        "code": "USER_NOT_FOUND",
        "message": "User does not exist"
      }
    }

This structure separates:
- Machine-readable code
- Human-readable message

---

### 12. What each field means

- `code`
  - Stable identifier
  - Used by frontend logic
  - Never changes casually

- `message`
  - Human-readable
  - Can change wording
  - Displayed to users

---

### 13. Optional but useful error fields

You may also include:

    {
      "error": {
        "code": "VALIDATION_ERROR",
        "message": "Email is required",
        "details": {
          "field": "email"
        }
      }
    }

But keep it:
- minimal
- consistent

---

### 14. Express example: consistent error handling

    app.get('/users/:id', (req, res) => {
      const user = users.find(u => u.id === Number(req.params.id));

      if (!user) {
        return res.status(404).json({
          error: {
            code: "USER_NOT_FOUND",
            message: "User does not exist"
          }
        });
      }

      res.json(user);
    });

Same shape.
Every time.

---

## PART 4 — HUMAN-READABLE VS MACHINE-READABLE ERRORS

---

### 15. Why we need both

Humans need:
- Clear explanations
- Friendly messages

Machines need:
- Stable identifiers
- Logic-friendly values

One cannot replace the other.

---

### 16. Machine-readable errors 

Machine-readable errors:
- Are stable
- Are predictable
- Are used in conditionals

Example:

    if (error.code === "USER_NOT_FOUND") {
      // show empty state
    }

Never rely on message text for logic.

---

### 17. Human-readable errors 

Human-readable messages:
- Explain the problem
- Can be shown in UI
- Can be localized

Example:
- "Email is required"
- "You are not authorized"

Messages can change.
Codes should not.

---

### 18. What NOT to do 

❌ Bad:

    {
      "error": "User not found"
    }

Why bad:
- No machine-readable code
- Hard to handle programmatically

❌ Worse:

    {
      "errorCode": 40401
    }

Why bad:
- Cryptic
- Not self-explanatory

---

### 19. Error codes vs HTTP status codes

Important distinction:

- HTTP status code → protocol-level meaning
- Error code → application-level meaning

Example:
- HTTP 404
- Error code: USER_NOT_FOUND

Both are needed.
They serve different layers.

---

### 20. Logging vs returning errors

Rule:

- Clients get clean error responses
- Servers log full exception details

Never expose:
- Stack traces
- Internal errors
- Database messages

Security matters.

---

### 21. Common beginner mistakes

- Treating errors as crashes
- Returning stack traces
- Using messages as logic
- Inconsistent error shapes
- Mixing error & exception handling

Avoid these early.

---

### 22. Simple error-handling checklist

Before shipping an API:
- Are errors expected & documented?
- Are exceptions hidden from clients?
- Is error shape consistent?
- Are codes stable?
- Are messages human-friendly?

If yes → your API is mature.

---

### 23. Final definitions 

- **API error**: A controlled, expected failure caused by invalid client input or request conditions  
- **Exception**: An unexpected server-side failure indicating a bug or system issue  
- **Consistent error response shape**: A stable JSON structure used for all errors  
- **Human-readable vs machine-readable errors**: Separating messages for users from codes for program logic  

---

### 24. Mental model to remember forever

Think like this:

- Error = polite rejection with explanation
- Exception = system malfunction behind the scenes

Clients should only see **polite rejections**.
Developers deal with malfunctions.

That’s how professional APIs are built.
