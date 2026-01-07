# Why Data Is Split Across Tables

Up to this point, everything we have done with databases has assumed a simple idea:

> *All of the data we care about fits comfortably in one table.*

That assumption works — for a while.

This document explains **why that approach eventually breaks**, and why splitting data across multiple tables is not an academic preference, but a practical necessity.

By the end of this document, you should understand *why* multi-table databases exist before learning *how* to work with them.

---

### The Temptation of the Single Table

When you first design a database, a single table often feels like the simplest and cleanest solution.

For example, imagine a table that stores customer information:

- customer name  
- email  
- phone number  
- address  

So far, so good.

Now imagine you want to track orders for those customers. The temptation is obvious:

> “I’ll just add some order-related columns to the same table.”

You might add:
- order_id  
- order_date  
- order_total  

At first glance, this feels efficient. Everything is in one place.

This is the moment where problems begin.

---

### Repetition Is the First Warning Sign

Consider what happens when a customer places multiple orders.

You now have two options, and both are bad:

1. **Repeat the customer data** for each order  
2. **Add multiple order columns** to the same row  

If you repeat the customer data, you now have:
- the same name stored multiple times
- the same email stored multiple times
- the same address stored multiple times

This is called **data duplication**, and it is not harmless.

---

### Why Duplicate Data Is Dangerous

Duplicate data creates inconsistency.

If a customer changes their email address, you now have to:
- find every place that email appears
- update all of them
- hope you didn’t miss one

If even one copy is missed, your database is now lying to you.

Worse, the database has no way to know it is wrong — because *you told it this was allowed*.

The more duplication you have, the harder your data is to trust.

---

### The “Just Add More Columns” Trap

The second option — adding more columns — fails differently.

You might try:
- order_1_date
- order_1_total
- order_2_date
- order_2_total

This approach collapses almost immediately.

Questions you can no longer answer cleanly:
- How many orders does this customer have?
- What if they have more orders than you planned for?
- How do you query “all orders” across customers?

When columns start representing *repeated ideas*, the table has stopped making sense.

---

### Table Representation

This is the core rule that fixes these problems:

> **A table should represent one kind of thing.**

A customer is one kind of thing.  
An order is another kind of thing.

They are related, but they are not the same.

Trying to store them together creates confusion because the table no longer has a single, clear meaning.

---

### Splitting Data Solves Structural Problems

Instead of one table doing everything, we separate concerns.

We create:
- one table for customers
- one table for orders

Each table now:
- represents one concept
- has a clear purpose
- avoids repetition

Customer data is stored once.  
Order data is stored once.

This separation eliminates duplication and restores clarity.

---

### Relationships Come from Separation

Once data is split across tables, a new question naturally arises:

> *How do these tables stay connected?*

That question leads directly to:
- relationships
- keys
- joins

We are not solving that yet.

What matters right now is understanding **why** separation happens at all.

Relationships are not theory layered on top of databases.  
They are a *consequence* of sane structure.

---

### Scaling Makes the Problem Obvious

The need to split data becomes unavoidable as systems grow.

Small examples can hide bad design. Large systems cannot.

As soon as you need to:
- store many related records
- query across categories
- update shared information safely

single-table designs fall apart.

Multi-table designs scale because structure scales.

---

### This Is Not About Perfection

Splitting data is not about creating the “perfect” schema.

It is about:
- reducing duplication
- preventing inconsistency
- keeping meaning clear
- making queries possible

Good database design is defensive.  
It assumes change will happen.

---

### Recap

Remember this:

> Single tables fail when one row tries to represent more than one kind of thing.  
> Duplication creates inconsistency.  
> Splitting data restores meaning and safety.

Multi-table databases exist because reality demands them.

---

### What Comes Next

**Next**, we will formalize how split tables remain connected.

That means introducing:
- keys
- relationships
- and the rules the database enforces to keep data consistent
