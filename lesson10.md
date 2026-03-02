# What Is an API?

“API” is one of those terms people throw around constantly.  API stands for:

> **Application Programming Interface**

That sounds complicated, but it's generally not so.

An API is simply:

> A controlled way for one piece of software to talk to another piece of software.

---

### The Core Idea

In many modern systems there are three conceptual layers:

1. Data
1. Backend
1. Frontend

These terms are somewhat loose, but in 2026 these are the generally accepted terms. They encompass more than what you might think and are fluid, but in our case today you can think of the 3 layers like this:

1. Database
2. Application Programming Interface (API)
3. Client

Client here just means an app or piece of software accessing the data through the API, and can be all kinds of things including a web app, mobile app, desktop app, CLI tool, or another service. This is sometimes called the 'Presentation Layer' or 'User'.

<u>Here's the important part</u> - In most real systems, the client does **not** talk directly to the database.

Instead:

- The client sends a request to the api.
- The api decides what to do.
- The api talks to the database.
- The api returns a response.

That middle layer — the part that receives requests and sends responses — is the **API**.

It is the interface between systems.

---

### Why Not Just Let Everything Access the Database Directly?

Good question.

Why not just let every program connect directly to the SQLite file and run whatever SQL it wants?

Because that would be chaos.

Here are the problems with direct database access:

#### 1) Security

If everyone can run SQL directly:
- They can delete data.
- They can modify data incorrectly.
- They can read data they shouldn’t see.

An API controls access.

It decides:
- What can be read
- What can be written
- What must be validated

---

#### 2) Validation

Suppose a user tries to insert:

- a negative quantity
- a 5-character state abbreviation
- a blank last name

The database *might* reject it. It might not.

The API is where you check:
- Is this valid?
- Does this make sense?
- Should this be allowed?

The API enforces logic before data reaches the database.

---

#### 3) Abstraction

The database structure might change.

If every program depends directly on table structure, then:
- Every change breaks everything.

But if everything talks to the API instead:
- The API can adapt internally.
- External systems don’t have to change.

The API becomes a buffer layer.

---

### What Actually Happens in an API?

Let’s walk through a simple example.

The client visits:

```
GET /customers/12
```

Here is what happens:

1. A request is sent to a server.
2. The API receives that request.
3. The API runs SQL like:
   ```sql
   SELECT * FROM Customer WHERE customer_id = 12;
   ```
4. The database returns a row.
5. The API converts that row into JSON.
6. The API sends the JSON back.

That’s it.

An API is just:

> Request in → Logic → Database → Response out

---

### APIs and Databases

The database stores data.

The API controls access to that data.

Think of it like this:

- The database is the **vault**.
- The API is the **guard at the door**.

The guard:
- checks credentials
- verifies requests
- prevents nonsense
- allows only safe operations

Without the guard, the vault would be exposed.

---

### What Is HTTP?

Most modern APIs use something called **HTTP**.

HTTP is a protocol for sending requests and responses across networks.

When you open a browser and go to:

```
https://example.com
```

You are making an HTTP request.

APIs use the same system.

Common HTTP methods include:

- **GET** → retrieve data
- **POST** → create data
- **PUT** → update data
- **DELETE** → remove data

Each method has a purpose.

---

### What Is JSON?

Most APIs send and receive data using **JSON**.

JSON stands for:

> JavaScript Object Notation

It looks like this:

```json
{
  "customer_id": 12,
  "first_name": "Jessica",
  "last_name": "Kim",
  "active": 1
}
```

JSON is:
- structured
- readable
- language-independent

It is the standard way APIs communicate data.

---

### Types of APIs

Not all APIs are the same.

Here are the most common types.

#### 1) REST APIs (Most Common)

REST stands for:

> Representational State Transfer

In practice, this usually means:

- Resources have URLs
- HTTP methods define actions
- Data is sent as JSON

Examples:

- `GET /customers`
- `POST /orders`
- `DELETE /items/5`

REST APIs are:
- simple
- predictable
- widely used

This is what we will build.

---

#### 2) GraphQL APIs

GraphQL allows clients to request exactly the data they want.

Instead of:

```
GET /customers/12
```

You send a structured query asking for specific fields.

GraphQL is powerful, but more complex.

---

#### 3) Internal vs External APIs

### Internal APIs

Used within a company.
Example:
- A web frontend talking to a backend server.

### External (Public) APIs

Used by third parties.
Example:
- Twitter API
- Stripe API
- Google Maps API

Same concept. Different audience.

---

#### 4) Web APIs vs Library APIs

Not all APIs are web-based.

A function library is also an API.

For example:

```python
math.sqrt(9)
```

The `math` module exposes functions.
That is an API.

The difference is:
- Library APIs are local.
- Web APIs use HTTP.

---

### Where Flask Fits In

Flask is what you will be using to build your api for this course. Flask is a lightweight Python framework for building web APIs.

It allows you to define routes like:

```python
@app.route("/customers")
def get_customers():
    ...
```

Flask handles:
- receiving HTTP requests
- sending HTTP responses

You handle:
- logic
- validation
- SQL
- JSON

Flask is just the tool.
The API is the concept.

---

### Putting It All Together

In a real application, the flow looks like this:

User / App  
↓  
HTTP Request  
↓  
Flask API  
↓  
SQL Query  
↓  
SQLite Database  
↓  
JSON Response  
↓  
User / App  

The API is the controlled bridge between the outside world and your data.

---

### Why This Matters

Up to now, you have been writing SQL directly.

That is important.

But in real systems:

- Applications talk to databases.
- Users talk to applications.
- APIs control the communication.

Understanding APIs means you now understand:

- How mobile apps retrieve data
- How websites load content
- How services communicate
- How software systems scale

This is the moment where databases stop being isolated files and start becoming infrastructure.

And that is why we are building one next.