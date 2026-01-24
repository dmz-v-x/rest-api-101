## What is a Response

### 1. Start with what you already know

You now know:

- Client sends a request  
- Server receives it  

Naturally, the server must answer back.

That answer is called a **response**.

---

### 2. What is a response?

👉 A response is:

A message sent by a server back to a client after processing a request.

---

### 3. Response completes the communication

In REST APIs:

- Client sends request  
- Server sends response  
- Interaction ends  

No response = client is left waiting.

---

### 4. What does a response contain?

A response usually contains:

- Result of the request  
- Data (if any)  
- Status information  

This tells the client:

- Did it succeed?  
- Did it fail?  
- What happened?  

---

### 5. Responses in REST APIs

REST responses are:

- HTTP responses  

They are standardized so:

- Any client can understand them  
- Different systems can communicate reliably  

---

### 6. Very simple example 

Client requests:

“Give me user 123”

Server responds:

“Here is user 123”  
or  
“User not found”

Both are valid responses.

---

### 7. One request → one response 

In REST:

- One request always maps to one response  
- Server never sends multiple responses  
- Server never responds without a request  

---

### 8. Responses tell clients what to do next

Based on the response:

- Client may show data  
- Client may show error  
- Client may retry  
- Client may redirect user  

Server does not control UI — response guides the client.

---

### 9. Responses are also just data

Important mindset:

A response is:

- Not code execution  
- Not UI logic  
- Just structured data  

Clients decide how to use it.

---

### 10. Real-world analogy 

Think of a question & answer:

- Question = request  
- Answer = response  

Once answered, the conversation ends.

---

### A **response** is the server’s reply to a client’s request, containing the result and any relevant data.
