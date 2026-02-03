## Status Codes & Response Body Rules

### 1. Start with a very common beginner mistake

Many beginners do this:

- Always return `200 OK`
- Put error details only in the response body
- Ignore what the status code actually means

Example ❌:

    Status: 200 OK
    Body: { "error": "User not found" }

This technically “works”…
but it breaks how HTTP is designed to communicate.

---

### 2. The core idea 

👉 **Status codes and response bodies have different jobs**

- Status code → **What happened (at a high level)**
- Response body → **Details about what happened**

They must **agree** with each other.

If they disagree → clients get confused.

---

### 3. What status codes should communicate

Status codes should answer:

- Did it succeed?
- Did it fail?
- Whose fault was it?
- Should the client retry?

They are:
- Short
- Standardized
- Machine-friendly

Clients often make decisions **only from the status code**.

---

### 4. What response bodies should communicate

Response bodies should answer:

- What data was returned?
- What exactly went wrong?
- Any extra context?
- Any metadata?

They are:
- Detailed
- Human-readable
- Flexible

Bodies explain, status codes summarize.

---

### 5. The golden rule 

👉 **Status code and response body must never contradict each other**

Examples:

❌ Bad:
- Status: 200
- Body: `{ "error": "Invalid input" }`

✅ Good:
- Status: 400
- Body: `{ "error": "Invalid input" }`

---

## SUCCESS RESPONSES (2xx)

---

### 6. 200 OK — success with a response body

Use `200 OK` when:
- Request succeeded
- You are returning data

Example:

    Status: 200 OK
    Body:
    {
      "id": 1,
      "name": "Alex"
    }

Common for:
- GET
- PATCH (when returning updated resource)

---

### 7. 201 Created — success + new resource

Use `201 Created` when:
- A new resource was created

Rules:
- Response body may include created resource
- Or include `Location` header

Example:

    Status: 201 Created
    Body:
    {
      "id": 3,
      "name": "Sam"
    }

Never return `200` for creation if `201` fits.

---

### 8. 204 No Content — success with NO body

Use `204 No Content` when:
- Request succeeded
- Nothing needs to be returned

Important rule:
👉 **204 responses must not have a response body**

Example:
- DELETE /users/1

    Status: 204 No Content
    Body: ❌ (must be empty)

---

## CLIENT ERROR RESPONSES (4xx)

---

### 9. 400 Bad Request — invalid request

Use `400` when:
- Request body is malformed
- Validation fails
- Required fields missing

Response body should explain the error:

    Status: 400 Bad Request
    Body:
    {
      "error": "Email is required"
    }

This tells the client:
👉 “Fix your request.”

---

### 10. 401 Unauthorized — authentication issue

Use `401` when:
- Authentication is missing
- Token is invalid or expired

Response body should explain next step:

    Status: 401 Unauthorized
    Body:
    {
      "error": "Authentication required"
    }

Do NOT use 401 for permission problems.

---

### 11. 403 Forbidden — permission denied

Use `403` when:
- Client is authenticated
- But not allowed to access resource

Example:

    Status: 403 Forbidden
    Body:
    {
      "error": "You do not have permission"
    }

401 = who are you?  
403 = you can’t do this.

---

### 12. 404 Not Found — resource missing

Use `404` when:
- Resource does not exist
- ID is valid but not found

Example:

    Status: 404 Not Found
    Body:
    {
      "error": "User not found"
    }

Never return `200` with empty data for missing resources.

---

### 13. 409 Conflict — state conflict

Use `409` when:
- Request conflicts with current state

Examples:
- Duplicate email
- Version conflict

Response body should explain conflict:

    Status: 409 Conflict
    Body:
    {
      "error": "Email already exists"
    }

---

## SERVER ERROR RESPONSES (5xx)

---

### 14. 500 Internal Server Error — server failure

Use `500` when:
- Unexpected error occurs
- Bug in server code

Response body should be minimal:

    Status: 500 Internal Server Error
    Body:
    {
      "error": "Something went wrong"
    }

Do NOT expose stack traces or internals.

---

### 15. 503 Service Unavailable — temporary failure

Use `503` when:
- Server is down
- Under maintenance
- Overloaded

Clients may retry later.

---

## IMPORTANT RESPONSE BODY RULES

---

### 16. Always return JSON consistently

Even for errors, keep response format consistent.

❌ Bad:
- Sometimes string
- Sometimes JSON
- Sometimes empty

✅ Good:
- Always JSON
- Same shape

Example error shape:

    {
      "error": {
        "code": "USER_NOT_FOUND",
        "message": "User does not exist"
      }
    }

---

### 17. Do not overload success responses with error info

❌ Bad:
- Status 200
- `{ success: false, error: "..." }`

HTTP already has error semantics.
Use them.

---

### 18. Response body size should match the status code

- 2xx → return useful data
- 4xx → return error details
- 5xx → minimal error info
- 204 → no body

Match expectations.

---

### 19. Clients often ignore the body on errors

Important insight:

Many clients:
- Look at status code first
- Only read body if needed

If status code is wrong,
body might never be read.

---

### 20. Express example: correct status + body pairing

    app.get('/users/:id', (req, res) => {
      const user = users.find(u => u.id === Number(req.params.id));

      if (!user) {
        return res.status(404).json({
          error: "User not found"
        });
      }

      res.status(200).json(user);
    });

This is textbook-correct HTTP behavior.

---

### 21. Common beginner mistakes 

- Always returning 200
- Returning body with 204
- Using 500 for validation errors
- Hiding errors inside success responses
- Inconsistent error formats

Avoid these early.

---

### 22. A simple decision table 

- Success + data → 200 + body
- Success + created → 201 + body
- Success + no data → 204 + no body
- Client mistake → 4xx + error body
- Server failure → 5xx + minimal error body

---

### 23. Final definition 

**Status codes and response body rules** define a clear contract where the status code communicates the high-level outcome of a request, and the response body provides detailed information—both working together consistently to guide correct client behavior.

---

### 24. Mental model to remember forever

Think like this:

- Status code = verdict
- Response body = explanation

A verdict without explanation is confusing.
An explanation without the right verdict is misleading.

Good APIs always deliver **both correctly**.
