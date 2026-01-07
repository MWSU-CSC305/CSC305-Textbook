# Working with Multiple Tables

Up to this point, we have done all the hard structural work.

We learned:
- why single tables fail
- why data is split across tables
- how primary keys identify rows
- how foreign keys enforce relationships

At this stage, a natural frustration appears:

> *“My data is split… how do I actually use it together?”*

This document answers that question.

The answer is **joins**.

They are the mechanism that makes a relational database usable.

---

### Why Joins Exist at All

When data is split across tables, each table holds **part of the truth**.

For example:
- the `Customer` table knows *who* a customer is
- the `Order` table knows *what* was ordered

Neither table alone tells the full story.

If you want to answer a real question — such as:

> “Which customers placed orders last month?”

—you must combine data from multiple tables.

That combination happens at **query time**, not at storage time.

This is why joins exist.

---

### What a Join Actually Does

A join does **not** merge tables permanently.

A join:
- compares rows from two tables
- pairs rows based on a condition
- produces a **temporary result set**

Nothing is stored.  
Nothing is modified.

A join is a way of saying:

> “Show me rows from these tables where this relationship holds.”

---

### A Simple Join Example

Assume we have two tables:

- `Customer(customer_id, name, city)`
- `Order(order_id, order_date, customer_id)`

A basic join looks like this:

```sql
SELECT *
FROM Customer
JOIN Order
ON Customer.customer_id = Order.customer_id;
```

Read this slowly:

- take rows from `Customer`
- take rows from `Order`
- pair them where the customer IDs match

Each resulting row contains:
- customer data
- order data
- combined into one result

The tables themselves remain unchanged.

---

### The Join Condition Is Everything

This line is the heart of the join:

```sql
ON Customer.customer_id = Order.customer_id
```

It tells the database:
- *how* rows should be paired
- *which* rows belong together

If this condition is wrong:
- the result is wrong
- often spectacularly wrong

The database will not stop you.
It will do exactly what you asked.

---

### Why Duplicate Rows Appear

Beginners are often surprised by this:

> “Why does the customer data repeat?”

If one customer has five orders, then:

- that customer row will appear five times
- once for each matching order

This is not duplication of stored data.  
It is **multiplication of results**.

The join is showing:
> one row *per valid pairing*

This behavior is correct.

---

### Thinking in Pairings, Not Tables

Joins become predictable when you stop thinking in tables and start thinking in **pairings**.

Ask yourself:
- How many matching rows exist on each side?
- Is this one-to-many?
- Many-to-many?

Each valid match produces a result row.

Once you adopt this mental model, joins stop feeling mysterious.

---

### Selecting Only What You Need

Joined results often contain more columns than you want.

You can (and should) choose specific columns:

```sql
SELECT Customer.name, Order.order_date
FROM Customer
JOIN Order
ON Customer.customer_id = Order.customer_id;
```

This produces:
- one row per order
- with only the columns you care about

Again, this shapes the **result**, not the data.

---

### INNER JOIN vs LEFT JOIN

So far, we’ve used `JOIN`, which is shorthand for `INNER JOIN`.

An inner join returns:
- only rows where a match exists in **both** tables

Sometimes, you want something different.

A **LEFT JOIN** returns:
- all rows from the left table
- even if no match exists on the right

Example:

```sql
SELECT Customer.name, Order.order_id
FROM Customer
LEFT JOIN Order
ON Customer.customer_id = Order.customer_id;
```

This shows:
- all customers
- including those with no orders

For customers without orders, the order columns will be `NULL`.

---

### When to Use Each Join

- Use **INNER JOIN** when:
  - you only care about matching rows
  - missing relationships should be excluded

- Use **LEFT JOIN** when:
  - you want all rows from one table
  - missing related data is still meaningful
- There is also a **RIGHT JOIN**:
  - This is the opposite of `LEFT JOIN`
  - It will display all rows from the table on the right side of the expression
  - `RIGHT JOIN` is typically not used, mostly because you can acheive the same result by flipping the table order when using `LEFT JOIN`


In any case, the choice is about **intent**, not syntax.

---

### Joins Do Not Fix Bad Structure

Joins assume your structure is sane.

If:
- keys are missing
- foreign keys are incorrect
- data is duplicated improperly

joins will amplify the mess.

Joins are powerful, but they are not corrective tools.
They reflect the structure you built earlier.

---

### Reading Joins Before Running Them

Just like with `UPDATE` and `DELETE`, discipline matters.

Before running a join, ask:
- Which table is on the left?
- Which table is on the right?
- How many matches exist?
- What will repeat?

If you cannot answer those questions, stop and think.

SQL rewards prediction.

---

### Recap

Remember this:

> A join pairs rows based on a condition.  
> Each valid pairing produces a result row.  
> Duplicate results reflect real relationships.

Joins do not invent data.  
They reveal connections.

---

### What Comes Next

**Next**, we will step back from querying and focus on **design**.

We will learn how to:
- recognize good and bad schemas
- decide what should be a table
- avoid common beginner design mistakes
