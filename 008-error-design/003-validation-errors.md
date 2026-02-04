## Validation Errors

### 1. Start with the most common API failure

The **most frequent error in any API** is not server crashes.
It is **bad input**.

Examples:
- Missing required fields
- Wrong data types
- Invalid formats
- Values out of range

These are called **validation errors**.

They are normal.
They are expected.
They must be handled cleanly.

---

## PART 1 — WHAT VALIDATION ERRORS ARE

---

### 2. What a validation error is (plain English)

👉 A **validation error** happens when:

The client sends data that does **not meet the rules** required by the API.

Important:
- The server is working fine
- The request format is understood
- The data itself is invalid

This is not a crash.
This is the client sending bad input.

---

### 3. Validation errors are NOT bugs

This is a mindset shift:

- ❌ Validation error ≠ bug
- ✅ Validation error = expected behavior

Users make mistakes.
Clients send bad data.
APIs must handle this gracefully.

---

### 4. Where validation happens

Validation typically applies to:

- Request body
- Query parameters
- Path parameters
- Headers (sometimes)

Anywhere user input enters your system.

---

## PART 2 — VALIDATION ERRORS VS OTHER ERRORS

---

### 5. Validation error vs authentication error

Validation error:
- Data is invalid
- Example: email missing

Authentication error:
- Identity problem
- Example: token missing

Different problems.
Different status codes.

---

### 6. Validation error vs business rule error

Validation error:
- Data shape or format is wrong

Business rule error:
- Data is valid but violates rules

Example:
- Validation: age is not a number
- Business rule: age must be ≥ 18

They are related, but not the same.

---

## PART 3 — STATUS CODES FOR VALIDATION ERRORS

---

### 7. The most common status code: 400 Bad Request

👉 Use **400 Bad Request** when:

- Required fields are missing
- Data types are incorrect
- JSON structure is invalid

Example:
- Missing email
- Age is a string instead of number

---

### 8. When to use 422 Unprocessable Entity

👉 Use **422 Unprocessable Entity** when:

- Request format is valid JSON
- But semantic validation fails

Examples:
- Email format invalid
- Password too weak
- End date before start date

Not mandatory, but very expressive.

---

### 9. Pick ONE strategy and be consistent

Some APIs:
- Use only 400

Some APIs:
- Use 400 + 422

Both are acceptable.

❗ Consistency matters more than precision.

---

## PART 4 — CONSISTENT VALIDATION ERROR SHAPE

---

### 10. Why validation errors need structure

Validation errors often:
- Affect multiple fields
- Need to show specific feedback

Returning a single message is often not enough.

---

### 11. Recommended validation error response shape

Example:

    {
      "error": {
        "code": "VALIDATION_ERROR",
        "message": "Invalid request data",
        "fields": {
          "email": "Email is required",
          "age": "Age must be a number"
        }
      }
    }

This structure is:
- Predictable
- Scalable
- Frontend-friendly

---

### 12. Why this structure works

- `code` → machine-readable
- `message` → general explanation
- `fields` → per-field details

Frontend can:
- Highlight specific fields
- Show inline messages
- Handle logic cleanly

---

## PART 5 — EXPRESS EXAMPLES

---

### 13. Simple validation example (POST /users)

    app.post('/users', (req, res) => {
      const { name, email, age } = req.body;

      const errors = {};

      if (!name) {
        errors.name = "Name is required";
      }

      if (!email) {
        errors.email = "Email is required";
      }

      if (age === undefined) {
        errors.age = "Age is required";
      } else if (typeof age !== 'number') {
        errors.age = "Age must be a number";
      }

      if (Object.keys(errors).length > 0) {
        return res.status(400).json({
          error: {
            code: "VALIDATION_ERROR",
            message: "Invalid request data",
            fields: errors
          }
        });
      }

      res.status(201).json({ message: "User created" });
    });

This is textbook validation handling.

---

### 14. Validation errors should stop processing early

Important rule:

👉 If validation fails, **return immediately**

Do NOT:
- continue processing
- partially save data
- attempt business logic

Fail fast.
Fail clearly.

---

## PART 6 — HUMAN VS MACHINE READABILITY

---

### 15. Machine-readable part

Machine-readable:
- error.code
- field keys

Used for:
- logic
- analytics
- conditional handling

Never change these casually.

---

### 16. Human-readable part

Human-readable:
- error.message
- field messages

Used for:
- UI display
- user feedback

These can evolve over time.

---

## PART 7 — COMMON VALIDATION MISTAKES

---

### 17. Common beginner mistakes

- Returning only one error when many exist
- Using 500 for validation issues
- Mixing validation and business logic errors
- Returning inconsistent field names
- Returning raw library error messages

Avoid these early.

---

### 18. Do NOT leak internal validation details

❌ Bad:

    {
      "error": "SequelizeValidationError: email cannot be null"
    }

This leaks:
- Internal libraries
- Implementation details

Always translate to clean API errors.

---

## PART 8 — BEST PRACTICES SUMMARY

---

### 19. Validation error best practices checklist

- Validate all external input
- Use 400 or 422 consistently
- Return structured errors
- Include per-field messages
- Never expose internals
- Fail fast

If you follow this, your APIs feel professional.

---

### 20. Final definition

**Validation errors** are controlled, expected API responses that occur when client-provided data fails to meet required rules or constraints, and they should be communicated using clear status codes and structured, consistent error responses.

---

### 21. Mental model to remember forever

Think like this:

- Validation = checking the form before submitting
- Errors = polite feedback, not punishment

A good API doesn’t just say:
“No.”

It says:
“No — and here’s exactly what to fix.”
