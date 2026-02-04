## Authentication vs Authorization Errors

### 1. Start with a very common and costly confusion

Most beginners confuse these two:

- Authentication error
- Authorization error

They look similar.
They sound similar.
But they mean **very different things**.

Mixing them up leads to:
- Wrong status codes
- Broken security
- Confusing client behavior

So let’s fix this properly.

---

## PART 1 — THE CORE DIFFERENCE 

---

### 2. The one-line difference (memorize this)

👉 **Authentication = Who are you?**  
👉 **Authorization = What are you allowed to do?**

That’s it.
Everything else is a detail.

---

## PART 2 — AUTHENTICATION ERRORS

---

### 3. What authentication means 

👉 **Authentication** is the process of proving identity.

The server asks:
- “Who are you?”
- “Can you prove it?”

Usually done using:
- Tokens
- API keys
- Sessions
- OAuth credentials

---

### 4. What an authentication error really is

👉 An **authentication error** happens when:

The client fails to prove **who they are**.

Examples:
- No token sent
- Invalid token
- Expired token
- Malformed credentials

---

### 5. Correct status code for authentication errors

👉 **401 Unauthorized**

Yes, the name is confusing.
But this is correct.

401 means:
- Authentication is missing or invalid
- Identity is unknown or unverified

---

### 6. Authentication error example

Request:
- GET /users
- No Authorization header

Response:
- Status: 401 Unauthorized
- Body:

    {
      "error": {
        "code": "AUTHENTICATION_REQUIRED",
        "message": "Authentication token is missing or invalid"
      }
    }

Meaning:
👉 “I don’t know who you are. Authenticate first.”

---

### 7. Important rule about 401 responses

401 responses often include:
- A hint about how to authenticate
- Or instructions to log in

Clients may:
- Redirect to login
- Refresh token
- Prompt user

---

## PART 3 — AUTHORIZATION ERRORS

---

### 8. What authorization means

👉 **Authorization** is about permissions.

The server already knows who you are.
Now it asks:
- “Are you allowed to do this?”

---

### 9. What an authorization error really is

👉 An **authorization error** happens when:

The client is authenticated, but **does not have permission** to access the resource.

Examples:
- Normal user accessing admin endpoint
- User accessing another user’s data
- Missing required role or scope

---

### 10. Correct status code for authorization errors

👉 **403 Forbidden**

403 means:
- Identity is known
- Access is denied

---

### 11. Authorization error example

Request:
- DELETE /users/1
- Token belongs to non-admin user

Response:
- Status: 403 Forbidden
- Body:

    {
      "error": {
        "code": "PERMISSION_DENIED",
        "message": "You do not have permission to perform this action"
      }
    }

Meaning:
👉 “I know who you are. You just can’t do this.”

---

## PART 4 — SIDE-BY-SIDE COMPARISON 

---

### 12. Authentication vs Authorization 

Authentication:
- Question: Who are you?
- Happens first
- Status code: 401
- Problem: Identity not verified

Authorization:
- Question: Are you allowed?
- Happens after authentication
- Status code: 403
- Problem: Permission denied

---

### 13. Order matters 

The server must check in this order:

1. Authentication
2. Authorization

If authentication fails:
- Authorization is never checked

You can’t check permissions for an unknown user.

---

## PART 5 — EXPRESS EXAMPLES

---

### 14. Authentication check example

    function authenticate(req, res, next) {
      const token = req.headers.authorization;

      if (!token) {
        return res.status(401).json({
          error: {
            code: "AUTHENTICATION_REQUIRED",
            message: "Authentication token is missing"
          }
        });
      }

      req.user = { id: 1, role: "user" };
      next();
    }

This middleware answers:
👉 “Who are you?”

---

### 15. Authorization check example

    function authorizeAdmin(req, res, next) {
      if (req.user.role !== "admin") {
        return res.status(403).json({
          error: {
            code: "PERMISSION_DENIED",
            message: "Admin access required"
          }
        });
      }

      next();
    }

This middleware answers:
👉 “Are you allowed?”

---

### 16. Using both together 

    app.delete(
      '/users/:id',
      authenticate,
      authorizeAdmin,
      (req, res) => {
        res.status(204).send();
      }
    );

Flow:
- Authenticate first
- Authorize second
- Perform action last

---

## PART 6 — COMMON BEGINNER MISTAKES

---

### 17. Very common mistakes 

- Using 401 for permission errors
- Using 403 for missing token
- Returning 200 with error message
- Hiding auth errors in response body
- Mixing auth and business logic errors

These mistakes break client behavior.

---

## PART 7 — ERROR CODES + MESSAGES

---

### 18. Recommended error codes

Authentication:
- AUTHENTICATION_REQUIRED
- INVALID_TOKEN
- TOKEN_EXPIRED

Authorization:
- PERMISSION_DENIED
- INSUFFICIENT_ROLE
- ACCESS_FORBIDDEN

Stable codes.
Clear meaning.

---

### 19. Why clients care about the difference

Clients behave differently:

401:
- Redirect to login
- Refresh token
- Prompt user

403:
- Show “Access denied”
- Hide UI actions
- Do NOT retry authentication

Wrong status code = wrong UI behavior.

---

## PART 8 — FINAL SUMMARY

---

### 20. Final definitions

- **Authentication error**: Occurs when the client fails to prove its identity; answered with 401 Unauthorized  
- **Authorization error**: Occurs when an authenticated client lacks permission to access a resource; answered with 403 Forbidden  

---

### 21. Mental model to remember forever

Think like a security guard:

- Authentication: “Show me your ID.”
- Authorization: “You can’t enter this room.”

No ID → 401  
ID but wrong access → 403  

Once this clicks,
you will never mix them up again.
