# Designing Small Databases That Make Sense

Up to this point, we have focused on **how databases work**.

You now know:
- how data is structured
- how tables relate to each other
- how to read data
- how to modify data safely
- how joins reconstruct meaning

What we have *not* done yet is talk directly about **design**.

This document is about that step.

Design is where databases either become easy to work with — or quietly turn into a mess that everyone is afraid to touch.

Our goal here is not perfection.  
Our goal is **reasonableness**.

---

### Design Is About Decisions, Not Diagrams

Database design is often presented as a formal process:
- draw diagrams
- apply named rules
- chase ideal forms

That approach hides the truth.

In practice, database design is a series of decisions:

- What deserves its own table?
- What belongs as a column?
- What should be split?
- What should stay together?

This document gives you a way to **reason through those decisions**, not memorize rules.

---

### Start with Real Questions

The best way to design a database is to start with the questions it needs to answer.

For example:
- Who are our customers?
- What orders did they place?
- When did they place them?
- How much did they spend?

If your database can answer real questions cleanly, it is probably structured reasonably well.

If answering a simple question requires awkward queries or assumptions, something is wrong.

---

### One Table, One Kind of Thing (Again)

This rule shows up again because it matters.

> **A table should represent one kind of thing.**

If you find yourself saying:
- “This table mostly represents customers, but also…”
- “These columns are only used sometimes…”
- “We’ll just leave this blank most of the time…”

Those are warning signs.

Tables that represent multiple ideas become confusing to query and fragile to change.

---

### Columns Should Describe the Table’s Thing

Columns exist to describe the thing the table represents.

For a `Customer` table, columns might include:
- name
- email
- phone
- city

For an `Order` table, columns might include:
- order_date
- total_amount
- status

If a column describes *something else entirely*, it probably belongs in a different table.

---

### Repeating Groups Are a Red Flag

Any time you see patterns like:
- `item_1`, `item_2`, `item_3`
- `phone1`, `phone2`
- `date_a`, `date_b`, `date_c`

You are looking at a design problem.

Repeating groups usually mean:
- you don’t know how many values exist
- you are forcing multiple records into one row

Databases are very good at storing many rows.  
They are very bad at guessing how many columns you’ll need.

When you see repetition, think **new table**.

---

### If It Can Happen Many Times, It Probably Needs Its Own Table

This is one of the most useful heuristics you can learn.

Ask yourself:
- Can this occur more than once?
- Can it grow over time?
- Can it vary independently?

If the answer is yes, it likely belongs in its own table.

Orders belong in a table.  
Payments belong in a table.  
Log entries belong in a table.

Trying to compress repeating data into columns almost always fails.

---

### Designing for Change, Not for Today

Bad designs often look fine at first.

They break later.

A good design assumes:
- requirements will change
- data volume will grow
- new relationships will appear

This is why structure matters.

You are not designing for the current dataset.  
You are designing for **future uncertainty**.

---

### Avoid Storing Derived Data

Another common beginner mistake is storing values that can be calculated.

Examples:
- total_price when item prices already exist
- age instead of birthdate
- counts that can be derived from rows

Storing derived data creates a new problem:
- keeping it in sync

Unless there is a clear performance reason, let the database compute values when needed.

Inconsistent derived data is worse than missing data.

---

### Normalize Just Enough

You may hear the term *normalization*.

At its core, normalization means:
- removing unnecessary duplication
- ensuring each fact has one home

You do not need to memorize normal forms to do this well.

If you follow these principles:
- one table, one thing
- no repeating groups
- no duplicated facts

You are already normalizing in practice.

---

### Good Design Feels Boring

This is not a joke.

A well-designed database:
- looks obvious in hindsight
- doesn’t feel clever
- doesn’t surprise you later

If your design feels “smart,” it is often fragile.

Clarity beats cleverness every time.

---

### Critiquing a Design

When evaluating a database schema, ask:

- What does each table represent?
- Why does each column exist?
- Where could duplication occur?
- What happens when this grows?

If you can answer those questions confidently, the design is probably serviceable.

---

### Recap

Remember this:

> Good database design is about clarity, separation, and restraint.  
> Each table has a purpose.  
> Each column earns its place.

You are not chasing perfection.  
You are avoiding obvious failure modes.

---

### What Comes Next

**Next**, we will step back one final time and look at how database data is actually **used outside the database**.

We will explore:
- exporting data
- feeding applications
- and treating databases as part of larger systems
