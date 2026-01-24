## What is an API

### 1. Start with the simplest human idea

Imagine you want something from a system, but you are not allowed to touch its internals.

So the system gives you:

- A set of allowed ways to talk to it  
- Clear rules on what you can ask  
- Clear rules on what it will give back  

That set of rules is an **API**.

---

### 2. What is an API? 

👉 API stands for **Application Programming Interface**.

In simple words:

An API is a defined way for one piece of software to communicate with another.

That’s it.

---

### 3. API ≠ server

A common confusion:

- **Server** = runs code, processes logic  
- **API** = the interface exposed by the server  

Think:

- Server = kitchen  
- API = menu  

You don’t go into the kitchen.  
You order from the menu.

---

### 4. What does an API define?

An API defines:

- What actions are allowed  
- What data can be requested  
- What data is returned  
- How requests must be formatted  
- How responses will look  

Clients must follow these rules.

---

### 5. APIs create boundaries 

APIs:

- Hide implementation details  
- Protect internal logic  
- Allow changes inside without breaking clients  

Clients care about:

What the API does, not how it does it.

---

### 6. API in REST context

In REST:

- API is exposed via HTTP  

Clients use:

- URLs  
- HTTP methods  
- Request data  
- Response data  

The REST API is the contract between client and server.

---

### 7. APIs are for machines, not humans

Important mindset:

- Websites → humans  
- APIs → software  

APIs:

- Return data, not HTML pages  
- Are consumed by code  
- Are predictable  

---

### 8. One server can expose many APIs

A backend server may expose APIs for:

- Users  
- Orders  
- Payments  
- Admins  

Each API has:

- Its own endpoints  
- Its own rules  

---

### 9. APIs enable reuse and scale

With APIs:

- Mobile apps  
- Web apps  
- Third-party apps  

Can all use the same backend.

This is huge for scalability.

---

### An **API** is a defined interface that allows one software system to communicate with another in a controlled way.
