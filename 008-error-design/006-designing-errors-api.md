## Designing Error API

### 0. What we are doing

You already built a **CRUD API in Express**.

Now we will **upgrade it to a production-grade API** by designing errors properly.

This means:
- Clear error types
- Correct status codes
- Consistent error response shape
- No internal detail leaks
- Centralized error handling

This is how **real-world APIs** are built.

---

## PART 1 — WHAT “DESIGNING ERRORS” REALLY MEANS

---

### 1. The beginner approach 

Most beginners do this:

- Throw random errors
- Return different shapes
- Leak internal messages
- Always return `500` or `200`

This works only in demos.
It fails badly in real systems.

---

### 2. The professional mindset 

👉 Errors are **part of the API contract**

They must be:
- Predictable
- Documented
- Consistent
- Safe
- Actionable

If clients can’t reliably handle errors, your API is broken.

---

### 3. Our error design goals

For our CRUD API, we want:

1. One error response format
2. Clear separation of:
   - Validation errors
   - Authentication errors
   - Authorization errors
   - Not found errors
   - Server errors
3. No internal details leaked
4. One centralized place to handle errors

---

## PART 2 — OUR STANDARD ERROR RESPONSE SHAPE

---

### 4. The single error format 

We will **always** return errors like this:

    {
      "error": {
        "code": "ERROR_CODE",
        "message": "Human readable message",
        "details": { ... } // optional
      }
    }

Rules:
- `code` → for machines
- `message` → for humans
- `details` → optional, structured info

Never return raw strings.
Never change this shape.

---

## PART 3 — SETTING UP OUR EXPRESS APP

---

### 5. Base Express app 

    const express = require('express');
    const app = express();

    app.use(express.json());

    let users = [
      { id: 1, name: "Alex", email: "alex@test.com", age: 25 },
      { id: 2, name: "Sam", email: "sam@test.com", age: 30 }
    ];

    let nextId = 3;

    app.listen(3000, () => {
      console.log("Server running on port 3000");
    });

This is our dummy database.

---

## PART 4 — CREATING CUSTOM ERROR CLASSES 

---

### 6. Why custom error classes?

We need to:
- Identify error type
- Attach status code
- Attach error code
- Avoid guessing later

So we define our own errors.

---

### 7. Base API Error class

    class ApiError extends Error {
      constructor(statusCode, code, message, details = null) {
        super(message);
        this.statusCode = statusCode;
        this.code = code;
        this.details = details;
      }
    }

Every error in our app will extend this.

---

### 8. Specific error types

    class ValidationError extends ApiError {
      constructor(details) {
        super(400, "VALIDATION_ERROR", "Invalid request data", details);
      }
    }

    class NotFoundError extends ApiError {
      constructor(resource = "Resource") {
        super(404, "NOT_FOUND", `${resource} not found`);
      }
    }

    class AuthError extends ApiError {
      constructor() {
        super(401, "AUTHENTICATION_REQUIRED", "Authentication required");
      }
    }

    class ForbiddenError extends ApiError {
      constructor() {
        super(403, "PERMISSION_DENIED", "You do not have permission");
      }
    }

These are reusable, predictable, and safe.

---

## PART 5 — CENTRALIZED ERROR HANDLING

---

### 9. Why centralized error handling matters

Without it:
- Every route handles errors differently
- Shapes drift
- Bugs multiply

With it:
- One format
- One place
- Easy to change

---

### 10. Global error-handling middleware

    app.use((err, req, res, next) => {
      // Log full error internally
      console.error(err);

      // Known API error
      if (err instanceof ApiError) {
        return res.status(err.statusCode).json({
          error: {
            code: err.code,
            message: err.message,
            details: err.details || undefined
          }
        });
      }

      // Unknown / exception
      res.status(500).json({
        error: {
          code: "INTERNAL_SERVER_ERROR",
          message: "Something went wrong"
        }
      });
    });

This guarantees:
- No stack traces leaked
- Consistent error responses
- Safe defaults

---

## PART 6 — APPLYING THIS TO OUR CRUD ROUTES

---

## GET — retrieving data

### 11. GET single user with proper error handling

    app.get('/users/:id', (req, res, next) => {
      try {
        const id = Number(req.params.id);
        const user = users.find(u => u.id === id);

        if (!user) {
          throw new NotFoundError("User");
        }

        res.json(user);
      } catch (err) {
        next(err);
      }
    });

Notice:
- No `res.status(404)` here
- We throw errors
- Middleware handles response

---

## POST — creating resources

### 12. POST with validation errors

    app.post('/users', (req, res, next) => {
      try {
        const { name, email, age } = req.body;
        const errors = {};

        if (!name) errors.name = "Name is required";
        if (!email) errors.email = "Email is required";
        if (typeof age !== "number") errors.age = "Age must be a number";

        if (Object.keys(errors).length > 0) {
          throw new ValidationError(errors);
        }

        const user = { id: nextId++, name, email, age };
        users.push(user);

        res.status(201).json(user);
      } catch (err) {
        next(err);
      }
    });

This cleanly separates:
- Validation logic
- Error formatting
- Response logic

---

## PUT — full replacement

### 13. PUT full update with validation

    app.put('/users/:id', (req, res, next) => {
      try {
        const id = Number(req.params.id);
        const index = users.findIndex(u => u.id === id);

        if (index === -1) {
          throw new NotFoundError("User");
        }

        const { name, email, age } = req.body;
        if (!name || !email || typeof age !== "number") {
          throw new ValidationError({
            message: "All fields are required for PUT"
          });
        }

        users[index] = { id, name, email, age };
        res.json(users[index]);
      } catch (err) {
        next(err);
      }
    });

PUT semantics are preserved.
Errors stay consistent.

---

## PATCH — partial update

### 14. PATCH partial update safely

    app.patch('/users/:id', (req, res, next) => {
      try {
        const id = Number(req.params.id);
        const user = users.find(u => u.id === id);

        if (!user) {
          throw new NotFoundError("User");
        }

        const { name, email, age } = req.body;

        if (age !== undefined && typeof age !== "number") {
          throw new ValidationError({ age: "Age must be a number" });
        }

        if (name !== undefined) user.name = name;
        if (email !== undefined) user.email = email;
        if (age !== undefined) user.age = age;

        res.json(user);
      } catch (err) {
        next(err);
      }
    });

PATCH updates only what’s valid.
Invalid fields are rejected cleanly.

---

## DELETE — removing resources

### 15. DELETE with correct semantics

    app.delete('/users/:id', (req, res, next) => {
      try {
        const id = Number(req.params.id);
        const index = users.findIndex(u => u.id === id);

        if (index === -1) {
          throw new NotFoundError("User");
        }

        users.splice(index, 1);
        res.status(204).send();
      } catch (err) {
        next(err);
      }
    });

DELETE returns:
- 204 No Content
- No response body
- Clean error if missing

---

## PART 7 — WHAT WE ACHIEVED 

---

### 16. What this design gives us

✅ One error format  
✅ Correct status codes  
✅ No internal leaks  
✅ Clear client behavior  
✅ Easy frontend handling  
✅ Easy future expansion  

This is **real API design**, not toy code.

---

### 17. Why this scales well

As your app grows:
- Add new error classes
- Reuse error logic
- Never touch client code
- Never change error shapes

This is long-term thinking.

---

## PART 8 — COMMON BEGINNER MISTAKES 

---

### 18. Mistakes we intentionally avoided

- Returning raw errors
- Repeating error JSON everywhere
- Mixing status logic into routes
- Returning different shapes
- Leaking stack traces

Every one of these causes pain later.

---

## PART 9 — FINAL SUMMARY

---

### 19. Final definition 

**Designing errors in an API** means treating errors as first-class, predictable responses—using consistent structures, correct status codes, centralized handling, and safe messaging—so clients can reliably understand, handle, and recover from failures.

---

### 20. Mental model to remember forever

Think like this:

- Routes decide **what failed**
- Errors describe **why it failed**
- Middleware decides **how it is shown**

When you separate these concerns,
your APIs become clean, secure, and professional.
