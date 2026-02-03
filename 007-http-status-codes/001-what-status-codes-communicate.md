## What Status Codes Communicate

### 1. Start with the simplest idea

Every time a client sends a request, the server must answer **two things**:

1. Did it work or not?
2. What exactly happened?

The **status code** answers the first question immediately.

Before reading any response body, clients look at the **HTTP status code**.

---

### 2. What a status code is (plain English)

👉 An **HTTP status code** is:

A standardized number sent by the server that tells the client the **result of a request**.

In simple words:

Status code = short, machine-readable summary of the outcome.

---

### 3. Why status codes exist at all

Imagine APIs without status codes.

Every response would need:
- parsing the body
- guessing success or failure
- custom logic per API

Status codes solve this by being:
- standardized
- predictable
- universally understood

Every HTTP client knows what `200` or `404` means.

---

### 4. Status codes communicate intent, not data

Important mindset:

👉 **Status codes communicate meaning**  
👉 **Response bodies communicate details**

Example:

- Status: `404 Not Found`
- Body: `{ "error": "User not found" }`

Clients can react **only using the status code** if needed.

---

### 5. Status code categories (big picture)

Status codes are grouped by their first digit:

- **1xx** → Informational
- **2xx** → Success
- **3xx** → Redirection
- **4xx** → Client error
- **5xx** → Server error

This alone tells the client a lot.

---

## 2xx — SUCCESS CODES (request worked)

---

### 6. 200 OK — request succeeded

👉 Used when:
- Data is successfully returned
- Operation completed normally

Examples:
- GET /users
- GET /users/123
- PATCH /users/123 (with response body)

This is the most common status code.

---

### 7. 201 Created — resource was created

👉 Used when:
- A new resource is successfully created

Example:
- POST /users

Meaning:
- The request worked
- Something new now exists

Often includes:
- Created resource in body
- Or a `Location` header

---

### 8. 204 No Content — success, nothing to return

👉 Used when:
- Request succeeded
- No response body is needed

Common use cases:
- DELETE /users/123
- PATCH with no response body

Important:
- Body must be empty

---

## 4xx — CLIENT ERROR CODES (client did something wrong)

---

### 9. 400 Bad Request — invalid input

👉 Used when:
- Request body is malformed
- Validation fails
- Required fields missing

Example:
- Missing required fields
- Wrong data type

This tells the client:
👉 “Fix your request.”

---

### 10. 401 Unauthorized — not authenticated

👉 Used when:
- Authentication is required
- Client is not logged in
- Token is missing or invalid

Important:
- 401 means **who are you?**

---

### 11. 403 Forbidden — not allowed

👉 Used when:
- Client is authenticated
- But lacks permission

Example:
- User tries to access admin-only data

Important:
- 403 means **I know who you are, but you can’t do this**

---

### 12. 404 Not Found — resource does not exist

👉 Used when:
- Requested resource doesn’t exist
- ID is valid format but not found

Example:
- GET /users/999

Very common and very important.

---

### 13. 409 Conflict — request conflicts with current state

👉 Used when:
- Duplicate data
- State conflicts

Examples:
- Creating a user with existing email
- Version conflicts

This communicates a **business rule violation**.

---

### 14. 422 Unprocessable Entity — semantically invalid

👉 Used when:
- Request format is valid
- But business validation fails

Example:
- Password too weak
- Invalid domain-specific rules

Not always used, but very expressive.

---

## 5xx — SERVER ERROR CODES (server failed)

---

### 15. 500 Internal Server Error — server crashed

👉 Used when:
- Unexpected error occurred
- Bug in server code
- Unhandled exception

Client did nothing wrong.

---

### 16. 502 / 503 — service issues

- **502 Bad Gateway** → upstream failure
- **503 Service Unavailable** → server overloaded or down

These usually indicate:
- Infrastructure problems
- Temporary outages

Clients may retry later.

---

## Important design principles

---

### 17. Status codes must match reality

❌ Bad:
- Returning `200` with `{ "error": "Invalid input" }`

✅ Good:
- Return `400` with error body

Status code and body must agree.

---

### 18. Do NOT invent custom status codes

HTTP status codes are:
- Finite
- Well-defined

Do not create:
- 600
- 700

Use the closest correct standard code.

---

### 19. Status codes guide client behavior

Clients use status codes to decide:

- Retry?
- Show error?
- Redirect?
- Logout user?

This is why correct codes matter.

---

### 20. Common beginner mistakes

- Always returning 200
- Using 500 for client errors
- Mixing 401 and 403
- Ignoring 201 and 204
- Hiding errors in response body

These break REST semantics.

---

### 21. A simple mental mapping 

- 2xx → “It worked”
- 4xx → “You did something wrong”
- 5xx → “I messed up”

This mapping is incredibly powerful.

---

### 22. Final definition 

**HTTP status codes** are standardized signals sent by the server that communicate the outcome of a request—indicating success, client errors, or server failures—allowing clients to reliably understand and react to API responses.

---

### 23. Mental model to remember forever

Think like this:

- Status code = traffic light
- Response body = explanation sign

You look at the light first.
Then you read the sign if needed.

That’s how HTTP was designed.
