# Using Database Data in Real Systems

Everything up to this point has been intentionally inward-facing.

We have treated the database as its own world:
- structure
- rules
- safety
- relationships
- design

That was necessary.

But databases are not built to be admired.  
They are built to be **used**.

This document explains how databases fit into real systems — how data leaves the database, how other software interacts with it, and how to think about boundaries so you don’t accidentally turn a useful tool into a liability.

This is the last instructional document because it ties everything together.

---

### A Database Is Not an Application

This needs to be stated clearly.

A database:
- stores data
- enforces rules
- answers questions

A database does **not**:
- decide what users can do
- contain business logic
- manage workflows
- present interfaces

Those responsibilities belong to applications.

If you try to turn a database into an application, you will fight it constantly.

---

### The Database Sits Behind Something

In real systems, users do not talk to databases directly.

Instead, the database sits behind:
- a web application
- a desktop application
- a service or API
- a script or automation tool

The database’s role is to be:
- reliable
- consistent
- boring

That boring reliability is what everything else depends on.

---

### How Programs Talk to Databases

Programs interact with databases through **connections**.

A connection:
- identifies which database to talk to
- authenticates access
- allows SQL to be sent
- returns results

From the database’s perspective, it does not matter whether the request came from:
- Java
- C#
- Python
- a command-line tool
- a GUI client

SQL is SQL.

This is why learning SQL matters more than learning any one database tool.

---

### Databases Do Not Care About Your Program

This is a subtle but important idea.

The database:
- does not know your program exists
- does not care how your code is structured
- does not track your variables or objects

It only sees:
- incoming SQL
- outgoing results

This separation is a **feature**, not a flaw.

It allows:
- multiple programs to share the same data
- systems to evolve independently
- changes to one layer without rewriting everything

---

### Result Sets Become Data Structures

When a program runs a `SELECT` query, it receives a **result set**.

That result set is then:
- mapped into objects
- transformed into JSON
- written to a file
- displayed to a user

This mapping step is where many mistakes happen.

Remember:
- database rows are records
- application objects are not rows
- they serve different purposes

Do not force one to behave like the other.

---

### Exporting Data

Sometimes data leaves the database entirely.

Common reasons include:
- reporting
- backups
- data sharing
- analysis

This often takes the form of:
- CSV files
- spreadsheets
- JSON exports

The important idea is this:

> Exported data is a **snapshot**, not the database itself.

Once data leaves the database:
- constraints are gone
- relationships are no longer enforced
- updates do not flow back automatically

Exports are useful — but they are not authoritative.

---

### Databases Are the Source of Truth

In a healthy system, the database is the **source of truth**.

That means:
- data originates there
- rules are enforced there
- consistency is guaranteed there

Applications may cache data.
Files may duplicate data.
Reports may summarize data.

But when there is disagreement, the database wins.

Designing systems with this assumption prevents subtle bugs and contradictions.

---

### Why You Do Not Let Everyone Write SQL

Another hard rule:

> Not everyone who reads data should be allowed to change it.

In real systems:
- some users read
- some users write
- some users administer

Databases support this through:
- users
- permissions
- roles

This is not paranoia.  
It is protection.

Accidental damage is far more common than malicious damage.

---

### SQL Is a Boundary, Not an Implementation Detail

Many beginners think of SQL as “just how data is stored.”

That framing is incomplete.

SQL is a **contract** between:
- the database
- and everything that uses it

If you change table structure:
- queries break
- applications fail
- reports become wrong

This is why:
- design matters
- discipline matters
- changes should be intentional

Databases reward stability.

---

### The Cost of Bad Boundaries

When boundaries are ignored:
- applications embed database assumptions
- logic leaks into SQL
- schemas become brittle
- nobody knows what is safe to change

These systems still run — until they don’t.

Most production database problems are not technical failures.
They are **design failures** that took years to surface.

---

### Your Responsibility as a Programmer

As a programmer working with databases, your responsibility is not just to:
- write queries
- retrieve data
- update rows

Your responsibility is to:
- respect structure
- predict consequences
- preserve trust in the data

A database full of incorrect data is worse than no database at all.

---

### A Concrete Example: Connecting to a Database from an API

Up to now, we have talked about databases and applications *conceptually*.

Let’s make this concrete.

A very common real-world setup looks like this:

- a database stores data
- an API sits in front of it
- other programs talk to the API
- users never touch the database directly

This section walks through that setup step by step.

---

### The Role of the API

An API (Application Programming Interface) acts as a **gatekeeper**.

Its job is to:
- accept requests from clients / front end
- decide what is allowed
- retrieve or modify data
- return results in a controlled format

The API is responsible for:
- authentication
- validation
- business rules

The database is responsible for:
- storage
- structure
- consistency

These roles should not be mixed.

---

### A Simple Scenario

Assume we have a database with this table:

```sql
Customer (
    customer_id,
    name,
    email,
    city
)
```

Now imagine we want an API endpoint that returns all customers in a given city.

A client might request:

```
GET /customers?city=StLouis
```

The client does **not** send SQL.
The client does **not** know about tables.
The client only knows the API.

---

### Opening a Database Connection

Inside the API, the first step is establishing a **database connection**.

Conceptually, this requires:
- database address
- database name
- credentials
- configuration options

This information is usually stored outside the code:
- environment variables
- configuration files
- secrets managers

Hard-coding credentials into source code is a serious mistake.

Once the connection is opened, the API can send SQL.

---

### Preparing a Query (Safely)

The API does **not** build SQL by concatenating strings.

Instead, it uses **parameterized queries**.

Conceptually, the API prepares a statement like:

```sql
SELECT customer_id, name, email
FROM Customer
WHERE city = ?
```

The `?` is a placeholder.

The actual value (`StLouis`) is supplied separately.

This matters because:
- it prevents SQL injection
- it avoids syntax errors
- it keeps intent clear

Never trust user input directly in SQL.

---

### Executing the Query

The database receives:
- the SQL statement
- the parameter value

It:
- validates the SQL
- checks permissions
- executes the query
- produces a result set

From the database’s perspective, this is no different than a query run from a client tool.

The database does not know this came from an API.
It only knows it received SQL.

---

### Mapping Results to Application Data

The API receives a result set.

That result set might look like:

- multiple rows
- each row containing column values

The API now:
- iterates over the rows
- maps each row into an internal data structure
- prepares a response format

For example:
- rows → objects
- objects → JSON

At this point, the database is done.
Everything else happens in application code.

---

### Returning the Response

The API sends a response back to the client.

For example:

```json
[
  {
    "id": 1,
    "name": "Alice Smith",
    "email": "alice@example.com"
  },
  {
    "id": 3,
    "name": "Bob Jones",
    "email": "bob@example.com"
  }
]
```

Notice what the client never sees:
- table names
- primary keys beyond what the API chooses
- SQL syntax
- database structure

That separation is intentional.

---

### Where Mistakes Usually Happen

Most API–database problems come from breaking boundaries.

Common mistakes include:
- letting clients send raw SQL
- embedding business logic in queries
- returning database structure directly
- skipping validation
- opening too many connections

None of these are database failures.
They are **design failures**.

---

### Why This Architecture Scales

This API–database pattern works because:

- the database enforces structure
- the API enforces rules
- clients stay simple
- changes are localized

You can:
- change the database without rewriting clients
- change the API without touching the database
- add new consumers safely

That flexibility is the entire point.

---

### The Key Idea to Keep

Remember this:

> The database is not exposed.  
> SQL is not public.  
> The API is the boundary.

If you respect that boundary, systems remain understandable and safe.

---

### What You Should Be Able to Do Now

At this point in the course, you should be able to:

- connect to a database
- inspect its structure
- read data confidently
- modify data deliberately
- design small, reasonable schemas
- split data when necessary
- reconnect it with joins
- use data outside the database safely

That is not trivial.

That is real, transferable skill.

---

### Recap

Remember this:

> A database is a stable core.  
> Applications come and go.  
> Data must survive all of them.

If you treat the database with care, everything built on top of it becomes easier.

---

### Closing Thought

Databases are not exciting because they are flashy.

They are exciting because:
- they outlive programs
- they outlast teams
- they preserve reality when memory fails
