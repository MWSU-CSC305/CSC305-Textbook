# Inside a Relational Database

In the previous document, we established **what a database is**, why it exists, and how it fits into real systems. We deliberately avoided internal details.

Now it’s time to move **inside** the database.

This document answers a very specific question:

> *What does data actually look like once it is inside a relational database, and why is it structured that way?*

By the end of this document, tables should stop feeling like abstract containers and start feeling like **deliberate, enforced structures**.

---

### The Core Promise of a Relational Database

A relational database makes a very specific promise:

> **Data will be stored in a predictable, structured way, and the database will enforce that structure.**

This is not optional.  
This is the entire point.

If you want flexibility without rules, relational databases are the wrong tool. We are using them *because* of the rules, not despite them.

That promise is delivered through a single core structure:

> **The table**

Everything else we do in this course builds on that.

---

### Tables Are Not Just Grids

At first glance, a table looks like a spreadsheet.

That similarity is both helpful and dangerous.

A table **does** have:
- columns
- rows
- a rectangular shape

But unlike a spreadsheet, a table is not permissive.

A table is a **contract**.

When you create a table, you are declaring:
- what kind of data is allowed
- where that data is allowed to go
- and what is forbidden entirely

Once that contract exists, the database enforces it whether you like it or not.

That enforcement is what makes databases reliable.

---

### Columns: Meaning Comes First

A column represents **one specific kind of information**. You can think of it like an attribute. If you have a table storing Customers, some of the columns in that table might be:

- a customer name
- an email address
- a date
- a price

Columns are composed of:
- a name (what it represents)
- a data type (what kind of values are allowed)

The name communicates *meaning*.  
The data type enforces *correctness*.

You should not attempt to store multiple ideas or points of meaning in a single column. Though technically possible, this defeats the purpose of the table structure. 

---

### Data Types

Every column has a **data type**.

A data type answers the question:

> *What kind of values are allowed here?*

There are many data types, but some common examples include:
- text
- numbers
- dates
- true/false values

The database uses data types to:
- reject invalid data
- store values efficiently
- compare values correctly

If a column is defined to hold dates, you cannot insert text into it. The database will stop you, as is its job.

We will introduce specific data types gradually as they become relevant. For now, the important idea is:

> **Columns are typed on purpose, and the database enforces those types.**

---

### Rows: Individual Records, Not Objects

A row represents **one record**.

One customer.  
One order.  
One entry.

Each row:
- follows the column definitions
- must provide a value (or intentionally not provide one) for each column
- exists independently of other rows

Rows are not objects.  
They are not instances of a class.  
They do not contain behavior, just data.

---

### Tables Represent One Kind of Thing

A well-designed table represents **one concept**.

Examples:
- customers
- products
- orders

Not:
- customers and orders
- products with embedded lists of prices
- “everything related to sales”

If you feel the urge to store unrelated ideas in the same table, that is a warning sign — not a clever shortcut.

We will talk later about *how* to split data across tables. For now, understand this rule:

> **One table, one kind of thing.**

---

### Structure Is Declared Up Front

Unlike files or in-memory structures, database structure is declared **before** data is inserted. Specifically, the structure of a table is declared when the table is created, forcing all data entered into it to conform to that structure.

With relational databases, you do not:
- add columns casually
- change meanings silently
- reshape data on a whim

You define structure intentionally, then store data that obeys it.

This up-front structure is what allows:
- safe querying
- reliable relationships
- predictable behavior

Without it, databases would be very unstable.

---

### What Happens When Structure Is Weak

When structure is poorly defined, problems appear quickly:

- inconsistent data
- invalid values
- ambiguous meaning
- fragile queries
- silent corruption

These problems do not announce themselves. They surface later, when systems grow and assumptions break. This is a very difficult problem to fix without obliterating the entire database in the process. Better to define a proper structure up front.

Your goal when designing a database is to make it make sense, sure - but it is also about **preventing future damage**.

---

### No SQL Yet

At this point, you understand:
- what tables are
- what columns represent
- what rows mean
- why structure exists
- what data types enforce

You have not yet written SQL to create tables or insert data.

Before touching SQL, you need a **mental model** of what SQL is manipulating. Otherwise, commands feel arbitrary instead of purposeful.

---

### Recap

Remember this:

> A relational database stores data in tables.  
> Tables are contracts enforced by the database.  
> Columns define meaning and type.  
> Rows are individual records that obey those rules.

Everything else we do builds on this structure.

---

### What Comes Next

**Next**, we will start interacting with this structure using SQL — carefully and safely — beginning with **reading data**, not modifying it.
