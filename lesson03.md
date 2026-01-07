# Reading Data Safely with SQL

So far, we have talked about databases **without interacting with them**.

You know:
- what a database is
- why it exists
- how data is structured inside it
- what tables, columns, rows, and data types mean

Now it is finally time to **interact** with that structure.

But we are going to do this in the safest possible way.

This document is about **reading data**, not changing it.

---

### SQL as Observation

Structured Query Language (SQL) is the language used to communicate with a relational database.

Before we use it to modify anything, we use it to **ask questions**.

This is an important mindset shift:

> When you use SQL to read data, you are not performing an action.  
> You are making a request.

The database evaluates that request and returns a result.  
Nothing is changed. Nothing is at risk.

This is why reading data is always the first skill to learn.

---

### The SELECT Statement

The most basic SQL request looks like this:

```sql
SELECT * FROM Customer;
```

This statement has three parts, and each one matters.

- `SELECT` means *I want data*
- `*` means *all columns*
- `FROM Customer` means *from the table named Customer*

Read it out loud:

> “Select all columns from the Customer table.”

That’s all it does.

It does **not**:
- change the table
- modify rows
- reorder data permanently
- create side effects

It simply returns a result.

---

### Query Results Are Not the Table

This is a critical idea that beginners often miss.

When you run a `SELECT` statement, the database does **not** give you the table itself.

It gives you a **result set**.

A result set:
- is temporary
- exists only for that query
- disappears when the query finishes

You are looking at a *view* of the data, not the data itself.

This distinction is why reading data is safe. You cannot accidentally damage a table by selecting from it. There are absolutely ways to accidentally damage a table, which we will talk about, but this is not one of them.

---

### Selecting Specific Columns

Using `*` is convenient, but it is rarely what you actually want.

You can ask for specific columns instead:

```sql
SELECT name, email
FROM Customer;
```

Now the database returns:
- only the `name` column
- and the `email` column

The table itself is unchanged.  
You are simply choosing what information you want to see.

This reinforces an important idea:

> SQL queries shape **results**, not tables.

---

### Filtering Rows with WHERE

Most of the time, you do not want *every* row.

You want **some** of them.

That is what the `WHERE` clause is for.

Example:

```sql
SELECT *
FROM Customer
WHERE city = 'St. Louis';
```

This query:
- examines every row in the table
- checks the condition in `WHERE`
- includes only rows where the condition is true

Nothing is deleted.  
Nothing is hidden permanently.

You are filtering the *result*, not the data.

---

### Thinking in Conditions, Not Loops

If you come from a programming background, this is important:

SQL does not loop the way your code does.

You do not say:
> “For each row, do this.”

You say:
> “Give me rows where this condition is true.”

The database handles the mechanics.

Your job is to describe **what qualifies**, not how to iterate.

---

### Ordering Results

Databases do not guarantee the order of rows unless you explicitly ask for one.

If order matters, you must request it.

Example:

```sql
SELECT name, signup_date
FROM Customer
ORDER BY signup_date;
```

This tells the database:
- return the result sorted by `signup_date`

If you do not specify an order, you should assume:
> the order is arbitrary

You can also specify if the order should be ascending or descending using `ASC` or `DESC`:

```sql
SELECT name, signup_date
FROM Customer
ORDER BY signup_date DESC
```

It is also possible to order by multiple columns in sequence. For example, maybe several customers have the same signup date, and you want to order them by their names secondarily. This can be done like so:

```sql
SELECT name, signup_date
FROM Customer
ORDER BY signup_date, name DESC
```

---

### Limiting Results

Sometimes you only want a small sample.

You can limit how many rows are returned:

```sql
SELECT *
FROM Customer
LIMIT 10;
```

This returns **up to** 10 rows from the result.

Again:
- the table is unchanged
- the database is not trimmed
- only the result is affected

---

### NULL Values

Sometimes a column does not have a value.

In a relational database, that absence is represented by `NULL`.

`NULL` does **not** mean:
- empty string
- zero
- false

It means:
> “No value is present.”

This distinction will matter more later. For now, understand this:

- `NULL` is a real state
- it requires explicit handling
- it often explains confusing results

We will revisit `NULL` when it becomes unavoidable.

---

### Reading Queries Before Running Them

A habit we want to build early is this:

> **You should be able to predict what a query will return before you run it.**

Ask yourself:
- What table am I selecting from?
- Which columns am I requesting?
- Which rows qualify?
- How will the results be ordered?

If you cannot answer those questions, don’t run the query yet.

SQL rewards thinking first.

---

### Why Reading Comes First

We are starting with reading data because:

- it is safe
- it builds intuition
- it reveals structure
- it exposes mistakes early

If you cannot confidently read data, modifying it will always feel dangerous.

That is not a confidence problem.  
It is a sequencing problem.

We are fixing that now.

---

### Recap

Remember this:

> A `SELECT` query asks a question.  
> The database answers with a temporary result.  
> The underlying data is unchanged.

If you keep that model in mind, SQL becomes predictable instead of intimidating.

---

### What Comes Next

**Next**, we will use SQL to **change data** — inserting, updating, and deleting — while keeping everything you’ve learned about safety and intention intact.
