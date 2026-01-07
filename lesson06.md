# Keys and Relationships

In the previous document, we answered an important question:

> *Why is data split across tables at all?*

The answer was structural necessity. Single tables fail when they try to represent more than one kind of thing.

Once data is split, a new problem appears immediately:

> *How do separate tables stay connected?*

This document answers that question.

The answer is **keys** and **relationships** — not as theory, not as diagrams, but as **rules enforced by the database**.

---

### Why Connections Need to Be Enforced

Once you separate data into multiple tables, those tables no longer exist in isolation.

For example:

- customers live in one table  
- orders live in another table  

These tables are clearly related, but the database does not *assume* that relationship.

Without explicit rules:

- an order could reference a customer that doesn’t exist  
- a customer could be deleted while orders still reference them  
- data could drift into an inconsistent state silently  

Relationships are not implied.  
They must be **declared and enforced**.

That enforcement begins with keys.

---

### Primary Keys: Identity, Not Just Uniqueness

Every table needs a way to uniquely identify its rows.

That identifier is called a **primary key**.

A primary key is:

- a column (or set of columns)
- whose value uniquely identifies a single row
- and never changes meaning

For example, a `Customer` table might include:

- `customer_id`

Two rows may share:

- a name  
- a city  
- an email  

But no two rows may share the same primary key value.

This is not optional.  
This is how the database knows which row is which.

---

### What a Primary Key Actually Does

A primary key serves several purposes at once:

- it guarantees uniqueness  
- it provides stable identity  
- it allows other tables to reference this row safely  

Think of a primary key as the row’s **handle** — the thing the database uses to grab it reliably.

Names change.  
Addresses change.  
IDs do not.

---

### Primary Keys Are Enforced by the Database

When a column is declared as a primary key, the database enforces rules automatically:

- no duplicate values are allowed  
- values cannot be NULL  
- each row must have exactly one primary key value  

You do not need to write code to check these rules.

The database does it for you — every time.

---

### Foreign Keys: How Tables Point to Each Other

Once one table has a primary key, another table can **reference it**.

That reference is called a **foreign key**.

A foreign key is:

- a column in one table  
- that stores values from another table’s primary key  

For example:

- `Customer` has `customer_id` as its primary key  
- `Order` might also have a `customer_id` column  

That column does not identify the order itself.  
It identifies **which customer the order belongs to**.

---

### What a Foreign Key Enforces

A foreign key creates a rule the database must obey:

> **You may not reference a row that does not exist.**

If an order claims to belong to customer 42, then customer 42 must exist in the `Customer` table.

If someone tries to insert or update data that violates this rule, the database rejects it.

This prevents entire classes of bugs:

- orphaned records  
- broken references  
- silent data corruption  

Again, this enforcement is automatic.

---

### Relationships Are Rules, Not Diagrams

At this point, it is tempting to think in diagrams and arrows. That's fine, as that is often how they are represented, but understand that a relationship is more than just a conceptual diagram - they are **constraints enforced by the database**.

The diagram is just a representation of rules that already exist.

---

### One-to-Many Relationships

The most common relationship is **one-to-many**.

Example:

- one customer  
- many orders  

This is implemented by:

- a primary key in the “one” table  
- a foreign key in the “many” table  

Each order references exactly one customer.  
Each customer may have many orders referencing them.

---

### Many-to-Many Relationships

Some relationships do not fit cleanly into one-to-many.

Example:

- students and classes  
- products and categories  

A student can enroll in many classes.  
A class can contain many students.

Relational databases handle this by introducing a **third table**, often called a join table.

That table contains:

- a foreign key to the first table  
- a foreign key to the second table  

This is not a workaround.  
It is the correct and intended solution.

---

### Creating Tables with Primary Keys

All of the rules we’ve discussed so far are enforced **when the table is created**.

A simple table with a primary key might look like this:

```sql
CREATE TABLE Customer (
    customer_id INT PRIMARY KEY,
    name TEXT,
    email TEXT
);
```

This SQL does several things at once:

- creates a table named `Customer`
- defines three columns
- declares `customer_id` as the primary key

By declaring the primary key here, you are telling the database:

- this column uniquely identifies each row  
- duplicates are not allowed  
- NULL values are forbidden  

These rules are enforced automatically for every insert and update.

---

### Creating Tables with Foreign Keys

Foreign keys are also declared when a table is created.

Here is an example of an `Order` table that references `Customer`:

```sql
CREATE TABLE Order (
    order_id INT PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id)
);
```

This statement declares:

- `order_id` as the primary key for orders  
- `customer_id` as a column  
- a foreign key rule tying `customer_id` to `Customer(customer_id)`  

The database now enforces the relationship.

You cannot insert an order for a customer that does not exist.  
You cannot break the connection accidentally.

This is the database doing real work for you.

---

### Creating Many-to-Many Relationships

Many-to-many relationships require a third table.

For example, a `Student` table and a `Course` table might be connected like this:

```sql
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Student(student_id),
    FOREIGN KEY (course_id) REFERENCES Course(course_id)
);
```

This table:

- contains no independent identity of its own  
- exists solely to connect students and courses  
- enforces valid relationships in both directions  

The combined primary key prevents duplicate enrollments.  
The foreign keys ensure only valid students and courses appear.

---

### What Happens Without Keys

If you skip keys and relationships:

- the database cannot protect you  
- invalid references slip in  
- cleanup becomes manual and dangerous  
- trust in the data erodes  

At that point, you are back to managing files — just with extra steps.

Keys are what make relational databases *relational*.

---

### You Are Declaring Rules, Not Hints

When you define primary keys and foreign keys, you are not giving the database suggestions.

You are declaring **non-negotiable rules**.

The database enforces those rules consistently:

- across all applications  
- across all users  
- at all times  

This is what allows multiple systems to share the same data safely.

---

### Recap

Remember this:

> Primary keys identify rows.  
> Foreign keys reference those identities.  
> Relationships are rules enforced by the database.

---

### What Comes Next

**Next**, we will finally *use* these relationships in practice.

That means learning how to:

- combine data from multiple tables  
- understand why duplicate rows appear  
- reason about query results before running them  

