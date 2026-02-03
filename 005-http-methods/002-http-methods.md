## HTTP Methods

### 0. What we are building

You asked for **one dummy CRUD API** that clearly shows **when and why** to use:

- GET
- POST
- PUT
- PATCH
- DELETE

We will:

- Use **Node.js + Express**
- Use an **in-memory array** as a fake database
- Add **basic input validation**
- Explain **each method through real usage**

No database.
No frameworks magic.
Pure REST fundamentals.

---

## Dummy resource we’ll use: `users`

Each user looks like this:

    {
      id: number,
      name: string,
      email: string,
      age: number
    }

---

### 1. Setup: basic Express app + fake database

    const express = require('express');
    const app = express();

    app.use(express.json());

    // Fake in-memory database
    let users = [
      { id: 1, name: 'Alex', email: 'alex@test.com', age: 25 },
      { id: 2, name: 'Sam', email: 'sam@test.com', age: 30 }
    ];

    let nextId = 3;

    app.listen(3000, () => {
      console.log('Server running on port 3000');
    });

This array is our **database**.
When the server restarts → data resets (that’s fine for learning).

---

## 4.2 GET — Retrieving data

### What GET is used for

👉 GET is used to **read data**  
👉 GET must **never change data**

We’ll implement:
- GET all users
- GET a single user

---

### GET all users

    app.get('/users', (req, res) => {
      res.json(users);
    });

Example request:
- GET /users

Meaning:
- “Give me all users”

Nothing changes on the server.

---

### GET single user by ID

    app.get('/users/:id', (req, res) => {
      const id = Number(req.params.id);

      const user = users.find(u => u.id === id);

      if (!user) {
        return res.status(404).json({ error: 'User not found' });
      }

      res.json(user);
    });

Example request:
- GET /users/1

Meaning:
- “Give me user with id = 1”

---

### Key GET rules

- No request body
- Safe
- Idempotent
- Cacheable

---

## 4.3 POST — Creating resources

### What POST is used for

👉 POST is used to **create a new resource**  
👉 Server generates the ID

---

### POST create a new user

    app.post('/users', (req, res) => {
      const { name, email, age } = req.body;

      // Validation
      if (!name || !email || typeof age !== 'number') {
        return res.status(400).json({
          error: 'name, email, and age are required'
        });
      }

      const newUser = {
        id: nextId++,
        name,
        email,
        age
      };

      users.push(newUser);

      res.status(201).json(newUser);
    });

Example request:
- POST /users

Body:
    {
      "name": "John",
      "email": "john@test.com",
      "age": 28
    }

Meaning:
- “Create a new user”

---

### Why POST goes to `/users`

Because:
- You are adding to the **collection**
- The resource does not exist yet

---

## 4.4 PUT — Full replacement

### What PUT really means

👉 PUT means **replace the entire resource**

Important rule:
- Missing fields = removed fields

---

### PUT replace a user completely

    app.put('/users/:id', (req, res) => {
      const id = Number(req.params.id);
      const { name, email, age } = req.body;

      // Full validation (all fields required)
      if (!name || !email || typeof age !== 'number') {
        return res.status(400).json({
          error: 'name, email, and age are required for full update'
        });
      }

      const index = users.findIndex(u => u.id === id);

      if (index === -1) {
        return res.status(404).json({ error: 'User not found' });
      }

      users[index] = { id, name, email, age };

      res.json(users[index]);
    });

Example request:
- PUT /users/1

Body:
    {
      "name": "Alex Updated",
      "email": "alex@new.com",
      "age": 26
    }

Meaning:
- “Replace user 1 completely with this data”

---

### PUT mental model

PUT = overwrite the file  
PATCH = edit the file

---

## 4.5 PATCH — Partial update

### What PATCH is used for

👉 PATCH updates **only the fields you send**  
👉 Missing fields stay unchanged

This is what most real APIs use.

---

### PATCH update part of a user

    app.patch('/users/:id', (req, res) => {
      const id = Number(req.params.id);
      const user = users.find(u => u.id === id);

      if (!user) {
        return res.status(404).json({ error: 'User not found' });
      }

      const { name, email, age } = req.body;

      // Update only provided fields
      if (name !== undefined) user.name = name;
      if (email !== undefined) user.email = email;
      if (age !== undefined) {
        if (typeof age !== 'number') {
          return res.status(400).json({ error: 'age must be a number' });
        }
        user.age = age;
      }

      res.json(user);
    });

Example request:
- PATCH /users/1

Body:
    {
      "age": 27
    }

Meaning:
- “Only update age for user 1”

---

### Why PATCH is preferred

- Safer than PUT
- Less data sent
- Less accidental data loss

---

## 4.6 DELETE — Removing resources

### What DELETE is used for

👉 DELETE removes a resource  
👉 Calling DELETE multiple times is safe (idempotent)

---

### DELETE a user

    app.delete('/users/:id', (req, res) => {
      const id = Number(req.params.id);
      const index = users.findIndex(u => u.id === id);

      if (index === -1) {
        return res.status(404).json({ error: 'User not found' });
      }

      users.splice(index, 1);

      res.status(204).send();
    });

Example request:
- DELETE /users/1

Meaning:
- “Remove user 1 from the system”

---

### Why DELETE returns 204

- Resource is gone
- Nothing left to return

---

## Final CRUD summary 

GET  
→ Read data  
→ No body  
→ No side effects  

POST  
→ Create new resource  
→ Goes to collection  

PUT  
→ Replace entire resource  
→ All fields required  

PATCH  
→ Update partial resource  
→ Only sent fields change  

DELETE  
→ Remove resource  
→ Resource disappears  


