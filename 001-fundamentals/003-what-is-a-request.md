## What is a Request

### 1. Start from what you already know

You now know:

- Client = asks for something  
- Server = listens and responds  

So naturally:

How does a client ask?

The answer is: by sending a **request**.

---

### 2. What is a request? 

👉 A request is:

A message sent by a client to a server asking it to perform an action or return data.

That’s it.

---

### 3. A request is just data

Important mindset:

A request is not magic.  
It’s just:

- Text  
- Structured data  
- Sent over the network  

The server reads this data and decides what to do.

---

### 4. Requests in REST APIs

In REST APIs, requests are:

- HTTP requests  

Every HTTP request has:

- A purpose  
- A destination  
- Some information  

---

### 5. The main parts of a request 

At a high level, a request contains:

- What do you want to do?  
- Where do you want to do it?  
- Extra information (if needed)  

We’ll break these down slowly.

---

### 6. Very simple example 

Client sends:

“Give me user details for user 123”

That message = request  
The server understands it and responds.

---

### 7. Requests are initiated by clients only

Important rule:

❌ Servers do not send requests  
✅ Clients always initiate requests  

Server only responds.

---

### 8. One request = one response

In REST:

- Each request gets exactly one response  

No response → client waits or times out  

This is called the **request–response model**.

---

### 9. Requests are independent 

Each request:

- Is standalone  
- Does not rely on previous requests  

This leads to:

- Scalability  
- Simplicity  

We’ll later call this **statelessness**.

---

### A **request** is a message sent by a client to a server asking it to do something or return data.
