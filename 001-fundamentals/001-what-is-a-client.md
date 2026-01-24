## What is a Client

### 1. Start with the simplest idea

In software, things talk to each other.

Whenever something requests something from something else, we call the requester a **client**.

---

### 2. What is a client? 

👉 A client is:

Any entity that sends a request to another system to get something done.

That’s it.

---

### 3. Client ≠ User 

This is a common beginner confusion.

**User** = human  
**Client** = software acting on behalf of someone (or itself)

Examples:

- Browser → client  
- Mobile app → client  
- Another backend service → client  
- Script / cron job → client  

The user is behind the client.

---

### 4. Common examples of clients

**Web browser**
- Chrome  
- Firefox  

**Mobile app**
- Android app  
- iOS app  

**Backend service**
- Service calling another service  

**Tools**
- Postman  
- curl  

All of these are clients.

---

### 5. What does a client actually do?

A client:

- Creates a request  
- Sends it over the network  
- Waits for a response  
- Uses the response  

That’s the entire job.

---

### 6. Client in REST API context

In REST APIs:

- Client sends an HTTP request  
- Server processes it  
- Server sends an HTTP response  

Client:  
“Please give me data”

Server:  
“Here is the data”

---

### 7. Clients don’t know server internals

Important principle:

A client:

- Does **NOT** know how the server works  
- Does **NOT** access the database  
- Only talks through the API  

This separation is by design.

---

### 8. One server, many clients

A single REST API server may serve:

- Web app  
- Mobile app  
- Admin dashboard  
- Other services  

All of them are clients of the same API.

