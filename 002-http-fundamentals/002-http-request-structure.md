## HTTP Request Structure.

### 1. Start with the simplest mental model

You already know this:

- Client sends a request  
- Server receives it  
- Server responds  

But **what exactly is inside a request**?

An HTTP request is **not vague**.  
It has a **strict structure**.

If you understand this structure,  
you understand **how the web actually works**.

---

### 2. What is an HTTP request? 

👉 An HTTP request is:

A structured text message sent by a client to a server that describes:

- What the client wants  
- Where it wants it  
- How it wants it  
- Any extra data needed  

Nothing more. Nothing less.

---

### 3. High-level structure of an HTTP request

Every HTTP request is made of **four main parts**:

1. Method  
2. URL  
3. Headers  
4. Body (optional)

Think of it like a form:

- Method = intent  
- URL = destination  
- Headers = metadata  
- Body = actual data  

We’ll break each one down slowly.

---

### 4. The HTTP Method 

The **method** tells the server the **action**.

Common methods:

- GET → read data  
- POST → create data  
- PUT → replace data  
- PATCH → update data  
- DELETE → remove data  

Example meaning:

GET /users  
→ “I want to read users”

POST /users  
→ “I want to create a user”

The method is **mandatory**.

---

### 5. Why methods matter

Methods are not decoration.

They affect:

- Server logic  
- Caching behavior  
- Security rules  
- REST correctness  

For example:

- GET requests should not change data  
- DELETE should remove something  
- POST usually creates something  

Servers rely on this contract.

---

### 6. The URL 

The URL identifies the **resource**.

Example:

/users  
/users/123  
/orders/456/items  

A URL usually contains:

- Path → resource location  
- Query parameters → filtering, options  

Example:

/users?role=admin&active=true

Here:

- /users → resource  
- role=admin → filter  
- active=true → filter  

---

### 7. URL path vs query parameters

Important distinction:

Path parameters:

- Identify a specific resource  
- Usually mandatory  

Example:

/users/123

Query parameters:

- Modify or filter behavior  
- Usually optional  

Example:

/users?page=2&limit=10

Both are part of the URL.

---

### 8. Headers 

Headers are **key–value pairs**.

They provide **metadata** about the request.

Examples of what headers carry:

- Authentication info  
- Content type  
- Client info  
- Caching rules  

Example headers:

Authorization: Bearer token123  
Content-Type: application/json  
Accept: application/json  

Headers do NOT contain the main data — they describe it.

---

### 9. Why headers exist 

Imagine sending a package.

You attach labels:

- Fragile  
- Destination  
- Sender info  

Headers are those labels.

They tell the server:

- How to read the body  
- Who is sending the request  
- What format is expected  

Without headers, servers guess — and guessing is bad.

---

### 10. Common request headers 

Some very common ones:

Authorization  
→ proves who you are  

Content-Type  
→ format of request body  

Accept  
→ format client wants in response  

User-Agent  
→ who the client is  

Headers make HTTP flexible and extensible.

---

### 11. The request body 

The body contains **actual data sent to the server**.

Examples:

- JSON  
- Form data  
- Plain text  

Usually used with:

- POST  
- PUT  
- PATCH  

Example JSON body:

    {
      "name": "Alex",
      "email": "alex@example.com"
    }

GET requests usually **do not have a body**.

---

### 12. Body format depends on Content-Type

The server decides how to read the body using headers.

Example:

Content-Type: application/json  
→ body is JSON  

Content-Type: application/x-www-form-urlencoded  
→ body is form data  

If Content-Type is wrong → server breaks.

---

### 13. Full raw HTTP request

A real HTTP request looks like this:

    POST /users HTTP/1.1
    Host: example.com
    Content-Type: application/json
    Authorization: Bearer abc123

    {
      "name": "Alex",
      "email": "alex@example.com"
    }

Important:

- First line → method + URL  
- Then headers  
- Empty line  
- Then body  

This exact structure is enforced by HTTP.

---

### 14. How Express reads request parts 

Simple Express server:

    const express = require('express');
    const app = express();

    app.use(express.json());

    app.post('/users', (req, res) => {
      console.log(req.method);   // HTTP method
      console.log(req.url);      // URL
      console.log(req.headers);  // Headers
      console.log(req.body);     // Body

      res.send('User received');
    });

    app.listen(3000);

Express automatically parses:

- Method  
- URL  
- Headers  
- Body  

And exposes them cleanly.

---

### 15. Mapping HTTP structure to Express objects

In Express:

- Method → req.method  
- URL → req.path / req.query / req.params  
- Headers → req.headers  
- Body → req.body  

This mirrors HTTP exactly.

Frameworks do not invent new concepts — they expose HTTP.

---

### 16. Validation happens at request level

Servers validate requests by checking:

- Is method allowed?  
- Is URL valid?  
- Are headers present?  
- Is body correct?  

Bad request structure → 400 Bad Request.

This protects servers from garbage input.

---

### 17. Security lives in request structure

Security is enforced through:

- Headers (Authorization)  
- Methods (blocking unsafe methods)  
- Body validation  
- URL access rules  

If request structure is wrong → request is rejected.

---

### 18. Advanced note: requests are immutable in theory

Conceptually:

- Client sends request  
- Server reads it  
- Server does not “change” the request  

The request is treated as **input**, not mutable state.

This supports statelessness.

---

### 19. Why understanding request structure matters

If you understand HTTP requests:

- Debugging becomes easy  
- API design becomes logical  
- Auth systems make sense  
- REST stops feeling magical  

You stop guessing.  
You start reasoning.

---

### 20. Final definition 

An **HTTP request** is a structured message sent by a client to a server, consisting of a method, a URL, headers, and an optional body, which together fully describe what the client wants the server to do.

---

### 21. Mental model to remember forever

Think of an HTTP request like filling a form:

- What do you want to do? → Method  
- Where? → URL  
- Extra instructions → Headers  
- Actual data → Body  

Fill it correctly → server understands you.  
Fill it wrong → server rejects you.

That’s the entire web.
