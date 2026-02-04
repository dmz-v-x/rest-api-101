## Error Codes vs Messages

### 1. Start with a very common beginner mistake

Most beginners design errors like this:

    {
      "error": "User not found"
    }

It looks fine.
It works.
Until…

- Frontend needs to behave differently for different errors
- Messages change slightly
- Localization is added
- Multiple teams consume the API

Suddenly, everything breaks.

This is where **error codes vs error messages** become critical.

---

### 2. The core idea

👉 **Error codes and error messages serve different purposes**

They are NOT interchangeable.

- Error code → for machines
- Error message → for humans

A good API always provides **both**.

---

## PART 1 — WHAT ERROR CODES ARE

---

### 3. What an error code is

👉 An **error code** is:

A stable, machine-readable identifier that represents a specific error condition.

Examples:
- USER_NOT_FOUND
- INVALID_EMAIL
- PERMISSION_DENIED

Error codes are:
- Predictable
- Consistent
- Designed for program logic

---

### 4. Why error codes exist

Code (frontend, mobile apps, other services) needs to:

- Make decisions
- Show different UI states
- Trigger retries or redirects
- Handle edge cases

Code cannot reliably do this using free-text messages.

---

### 5. Error codes must be stable 

Once you publish an error code:

👉 **Treat it like a public API**

Do NOT casually change:
- spelling
- casing
- meaning

Clients depend on it.

---

### 6. Example: using error codes in frontend logic

    if (error.code === "USER_NOT_FOUND") {
      showEmptyState();
    }

    if (error.code === "PERMISSION_DENIED") {
      redirectToLogin();
    }

This is impossible with messages alone.

---

## PART 2 — WHAT ERROR MESSAGES ARE

---

### 7. What an error message is

👉 An **error message** is:

A human-readable explanation of what went wrong.

Examples:
- "User does not exist"
- "Email is required"
- "You do not have permission"

Messages are:
- For users or developers
- Displayed in UI or logs
- Meant to be readable

---

### 8. Error messages are allowed to change

Unlike error codes, messages:

- Can be reworded
- Can be improved
- Can be localized
- Can be made friendlier

This flexibility is intentional.

---

### 9. Why messages alone are dangerous

If you rely on messages for logic:

- UI breaks when text changes
- Localization breaks logic
- Typos cause bugs
- Refactoring becomes impossible

Messages are not contracts.
Codes are.

---

## PART 3 — USING ERROR CODES + MESSAGES TOGETHER

---

### 10. The correct error response pattern

Best practice is to include **both**.

Example:

    {
      "error": {
        "code": "USER_NOT_FOUND",
        "message": "User does not exist"
      }
    }

This gives:
- Machines → `code`
- Humans → `message`

Everyone is happy.

---

### 11. Error codes vs HTTP status codes

These are NOT the same thing.

- HTTP status code → protocol-level meaning
- Error code → application-level meaning

Example:
- HTTP status: 404
- Error code: USER_NOT_FOUND

Both are required.
They operate at different layers.

---

### 12. Why you should NOT encode logic into messages

❌ Bad:

    {
      "error": "USER_NOT_FOUND"
    }

Why bad:
- Not human-friendly
- Not descriptive
- Leaks internal identifiers

❌ Worse:

    if (error.message.includes("not found")) { ... }

Never do this.

---

### 13. Naming error codes

Good error codes are:

- UPPER_SNAKE_CASE
- Descriptive
- Domain-specific

Examples:
- EMAIL_ALREADY_EXISTS
- INVALID_PASSWORD
- ORDER_OUT_OF_STOCK

Avoid:
- Numeric-only codes
- Cryptic abbreviations
- HTTP status duplication

---

### 14. Grouping error codes logically

As APIs grow, group error codes by domain:

- USER_NOT_FOUND
- USER_ALREADY_EXISTS
- USER_INVALID_EMAIL

- ORDER_NOT_FOUND
- ORDER_PAYMENT_FAILED

This improves clarity and maintainability.

---

### 15. Express example: error codes + messages

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

This is clean, predictable, and professional.

---

### 16. Localization becomes easy with error codes

Frontend can map codes to localized messages:

    USER_NOT_FOUND → "Usuario no existe"
    USER_NOT_FOUND → "Utilisateur introuvable"

The backend stays unchanged.
This is powerful.

---

### 17. Error codes help analytics & monitoring

With error codes you can:
- Count specific failures
- Track trends
- Alert on critical errors

Messages are too inconsistent for analytics.

---

### 18. What NOT to expose in messages

Never include:
- Stack traces
- Database errors
- Internal service names
- Sensitive data

Messages should be:
- Safe
- Clean
- User-appropriate

---

### 19. Common beginner mistakes

- Using only messages
- Using only codes
- Treating messages as logic
- Changing codes casually
- Mixing HTTP status and error codes

Avoid these early.

---

### 20. A simple decision rule

- Code → logic
- Message → display
- Status code → protocol meaning

Each has its role.
Never mix responsibilities.

---

### 21. Final definitions

- **Error code**: A stable, machine-readable identifier used by clients to programmatically handle errors  
- **Error message**: A human-readable explanation intended for display or debugging  
- **Best practice**: Always return both, with a consistent structure  

---

### 22. Mental model to remember forever

Think like this:

- Error code = error ID number
- Error message = explanation text

You don’t argue with the ID.
You show the explanation.

Professional APIs always speak **both languages** —  
machine language and human language.
