## What is a Server

### 1. Start with the core idea

If a client asks for something,  
there must be something that listens and responds.

That “something” is the **server**.

---

### 2. What is a server? (plain English)

👉 A server is:

A system that listens for requests from clients and sends back responses.

---

### 3. Server ≠ computer (important)

Many beginners think:

“Server means a powerful computer”

❌ Not necessarily.

A server can be:

- A laptop  
- A cloud VM  
- A container  
- A serverless function  

👉 **Server is a role, not hardware.**

---

### 4. What does a server actually do?

At a high level, a server:

- Waits for incoming requests  
- Receives a request  
- Processes the request  
- Sends back a response  

It does this over and over again.

---

### 5. Server in REST API context

In REST APIs, the server:

- Exposes endpoints  
- Accepts HTTP requests  
- Returns HTTP responses  

Clients never access:

- Database directly  
- Internal logic directly  

Everything goes through the server.

---

### 6. One server, many clients

A single server can handle:

- Thousands of clients  
- Millions of requests  
- Different client types  

Clients do not talk to each other — they all talk to the server.

---

### 7. Server responsibilities (important)

A REST API server usually handles:

- Authentication  
- Authorization  
- Business logic  
- Data access  
- Validation  
- Error handling  

The client should not do these.

---

### 8. Server must always be running

A server:

- Is usually always on  
- Waits for requests  
- Responds whenever a client asks  

If the server is down:

- Clients fail  
- API is unavailable  

---

### 9. Stateless vs stateful (preview)

Many REST servers are:

- **Stateless** → don’t remember clients between requests  

Each request:

- Is independent  
- Contains all needed info  

---

### A **server** is a system that receives requests from clients, processes them, and returns responses.
