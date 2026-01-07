# Modifying Data Without Destroying It

Up to this point, every interaction we have had with a database has been **non-destructive**.

We have asked questions.  
We have received answers.  
Nothing permanent has changed.

But obviously that can't be all that we do with SQL. This document introduces SQL commands that **modify data**. These commands are powerful, necessary, and dangerous if used carelessly.

---

### Why Modifying Data Feels Scary

When you modify data in a database, you are no longer working with temporary results.

You are changing:
- rows stored on disk
- data other programs may rely on
- information that may not be easily recoverable

If modifying data feels intimidating, that’s an accurate assessment of the stakes. But its something you'll need to do at some point, and probably frequently too. In order to not be so intimidated or reckless, we need to develop **understanding and habit**.

---

### The Three Ways Data Is Modified

There are only three fundamental ways to change data in a relational database:

- **INSERT** — add new rows  
- **UPDATE** — change existing rows  
- **DELETE** — remove rows  

Every data modification operation falls into one of these categories.

We will look at each one carefully, but first we need to establish a critical rule.

---

### The Most Important Rule

Before **any** data-modifying statement, you should be able to answer this question:

> *Exactly which rows will this affect?*

If you cannot answer that confidently, you are not ready to run the statement.

This rule alone prevents most database disasters.

---

### INSERT: Adding New Data

`INSERT` is used to add new rows to a table.

A simple example looks like this:

```sql
INSERT INTO Customer (name, email, city)
VALUES ('Alice Smith', 'alice@example.com', 'St. Louis');
```

Read it literally:

- insert a new row
- into the `Customer` table
- placing values into the specified columns

This statement:
- creates exactly one new row
- does not affect existing rows
- fails if the data violates table rules

If the data does not conform to the table’s structure or data types, the database will reject it.

---

### UPDATE: Changing Existing Data

`UPDATE` modifies existing rows.

This is where care becomes critical.

Example:

```sql
UPDATE Customer
SET city = 'Kansas City'
WHERE customer_id = 42;
```

This statement says:

- find rows in `Customer`
- where `customer_id` is 42
- change the `city` value

The `WHERE` clause determines **which rows are affected**.

Without it, the update applies to **every row** in the table.

That is not an exaggeration.  
That is the default behavior.

You should *never* run an `UPDATE` command without a `WHERE` clause. To demonstrate - lets say you had a Customer table with 10,000 rows. This is a fairly standard situation. Lets say you wanted to fix a mispelled name on customer_id 8772. You might write:

```sql
UPDATE Customer
SET name = "Jeanne"
```

If you ran this SQL, every one of the 10,000 rows in the Customer table would now have the name replaced with "Jeanne". I am not being verbose about this to instill fear - but its important that when modifying data that you be careful. Further in this document we will discuss the "Select First Habit", which can often prevent mistakes like this. Just for clarity, the correct syntax for the above situation is:

```sql
UPDATE Customer
SET name = "Jeanne"
WHERE customer_id = 8772
```

This SQL will only update the name for that one specific customer which matches the provided ID.

---

### DELETE: Removing Data

`DELETE` removes rows from a table.

Example:

```sql
DELETE FROM Customer
WHERE customer_id = 42;
```

This removes exactly one row — assuming the condition matches only one, its possible for a `WHERE` clause to match more than one row.

Just like `UPDATE`, a `DELETE` without a `WHERE` clause removes **all rows**. Apply the same caution.

Removing every row from a table does not delete the table itself. A table without rows is still a table with structure (columns). To actually delete a table and all data within, including its definition and structure, you can use the `DROP` command:

```sql
DROP TABLE Customers
```

---

### WHERE Is Not Optional

For `UPDATE` and `DELETE`, the `WHERE` clause is not a convenience. It is a **safety mechanism**.

If you ever find yourself writing:

```sql
UPDATE Customer;
```

or

```sql
DELETE FROM Customer;
```

Stop.

Those statements are almost never correct.

---

### The SELECT-First Habit

Here is the most important habit you will build in this course:

> **Write a SELECT first.**

Before running an `UPDATE` or `DELETE`, write a `SELECT` statement with the same `WHERE` clause.

Example:

```sql
SELECT *
FROM Customer
WHERE customer_id = 42;
```

Look at the result.

If it shows the rows you intend to modify:
- proceed carefully

If it shows more rows than expected:
- stop and fix the condition

This habit turns dangerous operations into predictable ones.

---

### Modifications Are Permanent

Unlike `SELECT`, modification statements:
- change stored data
- do not disappear
- cannot be undone automatically

Some databases support advanced recovery features, but you should never rely on them casually.

The safest assumption is:

> **If you change it, it’s changed.**

That assumption keeps you careful.

---

### Common Mistakes

Most beginner mistakes come from the same sources:

- forgetting a `WHERE` clause  
- misunderstanding which rows qualify  
- assuming changes are temporary  
- running statements without reading them  

Every one of these is preventable.

Slow down.  
Read your queries.  
Predict their effect.

SQL rewards caution.

---

### Recap

Remember this:

> `INSERT` adds rows.  
> `UPDATE` changes rows.  
> `DELETE` removes rows.  
> `WHERE` determines *which* rows are affected.

---

### What Comes Next

**Next**, we will confront a problem that naturally emerges from everything you’ve learned so far:

> *What happens when one table is no longer enough?*

We will explore why data is split across tables — not as theory, but as necessity.
