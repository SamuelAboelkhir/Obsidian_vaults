---
tags: 
- Other
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]

# What is SQL?
- Structured Query Language is the primary language used to manage relational databases
- While all relational DBs use SQL, each language has its own dialect and slightly altered rules
- Always have defined schemas
- Are table based
- Some of the most popular SQL DBs are:
	- SQLite
	- PostgreSQL
	- MySQL
	- CockroachDB
	- Oracle
# What is NoSQL
- Basically, these are all the DBs that are NOT SQL
- Each DB has its own unique way of writing and executing queries
- They are non-relational
- They have dynamic schemas
- Have a variety of different storage methods, including tables, files, key-value pairs, graphs, wide-column, etc...
- Some of the most popular NoSQL DBs are:
	- [MongoDB](https://en.wikipedia.org/wiki/MongoDB)
	- [Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra)
	- [CouchDB](https://en.wikipedia.org/wiki/Apache_CouchDB)
	- [DynamoDB](https://en.wikipedia.org/wiki/Amazon_DynamoDB)
	- [ElasticSearch](https://www.elastic.co/)
# Commands
- The following commands work for all SQL DBs but are mainly meant for SQLite since that's the one used in [boot.dev's](https://www.boot.dev/courses/learn-sql) course
- Also, if you wrap a string in double quotes `"` SQL will interpret it as a column, at least in some SQL DBs, so probably better to stick to single quotes `'` for strings
#### SELECT
```SQL
-- All fields
SELECT * FROM <table>;

-- Multiple fields
SELECT name, balance FROM users;

-- Selecting a count
SELECT COUNT(*) FROM employees;
```
#### CREATE
```SQL
-- One line
CREATE TABLE employees (id INTEGER, name TEXT, age INTEGER, is_manager BOOLEAN, salary INTEGER);

-- Multi line
CREATE TABLE employees(
    id INTEGER,
    name TEXT,
    age INTEGER,
    is_manager BOOLEAN,
    salary INTEGER
);
```
#### ALTER
```SQL
-- Rename a table of column
ALTER TABLE employees
RENAME TO contractors;

ALTER TABLE contractors
RENAME COLUMN salary TO invoice;

-- Add or drop a column
ALTER TABLE contractors
ADD COLUMN job_title TEXT;

ALTER TABLE contractors
DROP COLUMN is_manager;
```
#### INSERT
```SQL
INSERT INTO employees(id, name, title)
VALUES (1, 'Allan', 'Engineer');
```
#### DELETE
```SQL
DELETE FROM employees
    WHERE id = 251;
```
```SQL
DROP TABLE employees;
```
#### UPDATE
```SQL
UPDATE employees
SET job_title = 'Backend Engineer', salary = 150000
WHERE id = 251;
```
# Structuring
## Clauses/Keywords
- A clause is a 2nd parameter in the query like WHERE and AS that's used to add conditions to the query or shape the form of the outcome
#### AS
```SQL
SELECT employee_id AS id, employee_name AS name
FROM employees;
```
#### WHERE
- Doesn't need a specific example as it's the most used clause and will be shown in many of the examples in this note file
#### BETWEEN
```SQL
SELECT employee_name, salary
FROM employees
WHERE salary BETWEEN 30000 AND 60000;

SELECT product_name, quantity
FROM products
WHERE quantity NOT BETWEEN 20 AND 100;
```
#### Functions (IIF)
```SQL
-- The below example is like a ternary conditional
SELECT quantity,
    IIF(quantity < 10, 'Order more', 'In Stock') AS directive
    FROM products;
```
#### DISTINCT
```SQL
-- Returns unique values only
SELECT DISTINCT previous_company
    FROM employees;
```
## Logical Operators
- Logical operators seem to be only usable after a `WHERE` clause, which does honestly make sense
- You can also group logical operators with parentheses to specify the order of operations
#### Comparison Operators
- = (used for equality, not assignment)
- <
- >
- <=
- >=
- <> or !=
#### AND
```SQL
SELECT product_name, quantity, shipment_status
    FROM products
    WHERE shipment_status = 'pending'
    AND quantity BETWEEN 0 and 10;
```
#### OR
```SQL
SELECT product_name, quantity, shipment_status
    FROM products
    WHERE shipment_status = 'out of stock'
    OR quantity BETWEEN 10 and 100;

-- A more complicated example
SELECT count(*) AS junior_count 
  FROM users 
  WHERE (country_code = 'US' OR country_code = 'CA') AND age < 18;
```
#### IN
- Technically a shorthand for multiple OR conditions that returns true of false based on whether or not the first operand matches and of the values in the 2nd operand
```SQL
-- This is
SELECT product_name, shipment_status
    FROM products
    WHERE shipment_status IN ('shipped', 'preparing', 'out of stock');

-- Equivalent to
SELECT product_name, shipment_status
    FROM products
    WHERE shipment_status = 'shipped'
        OR shipment_status = 'preparing'
        OR shipment_status = 'out of stock';
```
#### LIKE
- Like is meant for partial matching and is coupled with two other wildcard operators
	- `%`: matches zero or more characters
	- `_`: matches a single character
		- You can add multiple `_` to extend the string length
		- 'AL___' is a word that's 5 char long starting with AL and then SQL will find all possible combinations for the 3 last chars in the available records
```SQL
-- Product starts with banana
SELECT * FROM products
WHERE product_name LIKE 'banana%';

-- Product ends with banana
SELECT * FROM products
WHERE product_name LIKE '%banana';

-- Product contains banana
SELECT * FROM products
WHERE product_name LIKE '%banana%';
```
#### LIMIT
```SQL
SELECT * FROM products
    WHERE product_name LIKE '%berry%'
    LIMIT 50;
```
#### ORDER BY
- Order by must always come before "LIMIT" if used together
```SQL
-- Order by sorts in ascending order "ASC" by default
SELECT name, price, quantity FROM products
    ORDER BY quantity DESC;
```
#### Case
- A case is a condition that can be used with something like order by to prioritize certain rows over others in the sorting
```SQL
ORDER BY
  CASE WHEN status = 'ACTIVE' THEN 0 ELSE 1 END,
```
# Migrations
- A migration alters the structure of the database as a whole, and can be considered similar to a git commit
- Any command like ALTER or CREATE that changes the DB's schema is a mutation
- Good migrations are small, incremental and ideally reversible
- A bad migration is something like changing the name of a table, without making proper adjustments to the code. This means that for all the code that referred to the old table name, that table no longer exists, which would break the code
	- The proper approach here is to deploy the new code that uses the new table name immediately after the migration
#### Up migration
- Applies changes that moves the schema forward
#### Down migration
- Rolls back the changes to the previous version
- A well written down migration should be the exact opposite of its up migration, and completely revert the changes made when we went up
- Always remember to back up your DB before a migration, especially a migration that removes something, otherwise, if you remove a column in UP, then add it back in DOWN, the column will be back, but not the data it once had
# Constraints
- A constraint is a rule that enforces some behavior on the database
- A `NOT NULL` constraint for example prevents NULL values from existing in a specific column
- Other constraints include:
	- `PRIMARY KEY`: A unique identifier for a record
	- `UNIQUE`: Forces a column to have unique values
	- `NOT NULL`: Already talked about it
	- `FOREIGN KEY`: The reason SQL DBs are relational in the first place. It's a field in one table, that references the PRIMARY KEY of another table
#### Adding foreign key
```SQL
CREATE TABLE departments (
    id INTEGER PRIMARY KEY,
    department_name TEXT NOT NULL
);

-- The CONSTRAINT field allows us to name the constraint, and is optional
CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    department_id INTEGER,
    CONSTRAINT fk_departments -- Create a constraint called fk_deparments
    FOREIGN KEY (department_id) -- Make this field a foreign key
    REFERENCES departments(id) -- Link the foreign field `id` from the departments table
);
```
# Aggregations
- Aggregations are single values derived by combining several other values
- We use them because we store data in its raw form in the database to avoid redundancy, and because we want to store related data in the same place (single source of truth) then we can aggregate it if we want to compute any specific additional information
#### COUNT
```SQL
-- We saw count earlier. It's used to get the number of records that exist in a table, or maybe that meet specific criteria if couple with conditional clauses like WHERE
SELECT COUNT(*)
FROM products
WHERE quantity = 0;
```
#### SUM
```SQL
-- Just note that you need to specify a single field here
SELECT SUM(salary)
FROM employees;
```
#### MAX
```SQL
-- Just note that you need to specify a single field here
SELECT MAX(price)
FROM products;
```
#### MIN
```SQL
SELECT product_name, MIN(price)
FROM products;
```
#### GROUP BY
- This clause groups rows that have similar values into summary rows and returns 1 row per group.
- We can apply aggregate functions on each group specifically too
```SQL
SELECT album_id, count(song_id) AS song_count
FROM songs
GROUP BY album_id;
```
#### AVERAGE
```SQL
SELECT AVG(song_length)
FROM songs;
```
#### HAVING
- This clause is used to filter the results of a GROUP BY query
- It's similar to WHERE but operates specifically on groups after they've been grouped rather than the individual rows before they've been grouped
```SQL
SELECT album_id, count(id) as count
FROM songs
GROUP BY album_id
HAVING count > 5;

-- A more complicated example
SELECT sender_id, SUM(amount) AS balance FROM transactions
  WHERE was_successful = 1  
  AND note LIKE '%lunch%'
  AND sender_id NOT NULL
GROUP BY sender_id
HAVING balance > 20
  ORDER BY balance;
```
#### ROUND
```SQL
SELECT ROUND(AVG(song_length), 1)
FROM songs
```
#### WITH
- WITH provides a way to write auxiliary statements for use in a larger query. These statements, which are often referred to as Common Table Expressions or CTEs, can be thought of as defining temporary tables that exist just for one query. Each auxiliary statement in a WITH clause can be a SELECT, INSERT, UPDATE, or DELETE; and the WITH clause itself is attached to a primary statement that can also be a SELECT, INSERT, UPDATE, or DELETE.
##### SELECT in WITH
- The basic value of SELECT in WITH is to break down complicated queries into simpler parts. An example is:
```sql
WITH regional_sales AS (
    SELECT region, SUM(amount) AS total_sales
    FROM orders
    GROUP BY region
), top_regions AS (
    SELECT region
    FROM regional_sales
    WHERE total_sales > (SELECT SUM(total_sales)/10 FROM regional_sales)
)
SELECT region,
       product,
       SUM(quantity) AS product_units,
       SUM(amount) AS product_sales
FROM orders
WHERE region IN (SELECT region FROM top_regions)
GROUP BY region, product;
```
# Schema
- There is no perfect way to architect a database schema, we can only do our best to choose a sane set of tables fields and constraints that will accomplish our goals
- For example, if we want a table that stores a user's balance, then we need a table that:
	- Keeps track of the user's current balance
	- Can see the historical balance at any point in the past
	- Can see a log of which transactions changed the balance over time
- One way of accomplishing this is via these fields
```SQL
CREATE TABLE transactions (
	id - INTEGER PRIMARY KEY
	sender_id - INTEGER
	recipient_id - INTEGER
	memo - TEXT - NOT NULL
	amount - INTEGER - NOT NULL
	balance - INTEGER - NOT NULL
);
```
# Subqueries
- In case the result of the query isn't enough to return the specific records that awe need, we can actually run another query on the result of that query, called a subquery
- One example of this is querying multiple tables
```SQL
-- The "IN" operator is not a must here, it could have easliy been an "=" if we expected only one exact match
-- This example here will return the id and song_name from the songs table, where the artist_id of the artist matches the same ID in the users table, but only for artists who's names start with Rick
SELECT id, song_name, artist_id
FROM songs
WHERE artist_id IN (
    SELECT id
    FROM artists
    WHERE artist_name LIKE 'Rick%'
);
```
#### No tables?
- Remember, SQL is actually a programming language, and as such, you don't even need a table to be able to use it
```SQL
SELECT 5 + 10 as sum;
```
# Normalization
- When it comes to normalization, we can start by talking about relations
- OneToOne relations are the least used, as they're usually solved by adding a field to the table
- ManyToOne relations are the most common though
- In ManyToMany relations we need to create join tables
- We have many ways of declaring foreign keys and references in either ManyToOne or ManyToMany tables
- Also, note how we made a column pair unique here instead of individual columns. This means that a column can repeat, but a combination of the same 2 columns, can't repeat
```SQL
-- The most verbose
CREATE TABLE users_countries (
  country_id INTEGER,
  user_id INTEGER,
  UNIQUE(country_id, user_id)
  FOREIGN KEY (country_id)
  REFERENCES countries(id)
  FOREIGN KEY (user_id)
  REFERENCES users(id)
);
-- A clearner version
CREATE TABLE users_countries (
  country_id INTEGER,
  user_id INTEGER,
  UNIQUE(country_id, user_id),
  FOREIGN KEY (country_id) REFERENCES countries(id),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
-- Inline
CREATE TABLE users_countries (
  country_id INTEGER REFERENCES countries(id),
  user_id INTEGER REFERENCES users(id),
  UNIQUE(country_id, user_id)
);
```
- The goal of normalization is to improve the data's structure or schema, by decreasing redundancy, and increasing integrity (data correctnes)
- This means we want to store data in its simplest form, and the least amount of copies, as copies can introduce bugs
- Data normalization follows 1 of 4 forms (1NF, 2NF, 3NF, BCNF) where NF means normal form and BCNF is Boyce-Codd NF
- These forms are just a set of rules, and if the data follows the rules, then its in one of the normal forms, and each normal form = the previous form + some extra rules
- As a rule of thumb, optimize for data integrity and de-duplication first, and denormalize later if there are any speed issues
- Also regarding data integrity, try to think long term here. For example, what if we stored the user's age in the database? Well, the user's age will become wrong with the passage of time right? So it would be better to store his birthday, and compute the age from it
- While for data redundancy within the same database, the issue here is that you would no longer have a single source of truth, and if the data changes in one place, but not another, now you have inconsistent data, you will have to update the redundant data everywhere it exists
- For joining tables, remember that a good joining table only manages the relation it's created to manage, and nothing else, so it shouldn't have any other information
#### Rules of Thumb for Database Design
1. Every table should always have a unique identifier (primary key)
2. 90% of the time, that unique identifier will be a single column named id
3. Avoid duplicate data
4. Avoid storing data that is completely dependent on other data. Instead, compute it on the fly when you need it.
5. Keep your schema as simple as you can. Optimize for a normalized database first. Only denormalize for speed's sake when you start to run into performance problems.
# Joins
- Joins allow us to utilize the relations that we set up between tables
#### Inner Join
- The default type of join
- It returns all the records in table_a that have matching records in table_b
![[inner_join.png]]
#### ON
- To perform a join, we must tell the database how to match the rows from each table
- The ON clause tells SQL which columns needs to be compared
```SQL
-- If the two columns have the same name, then we use the table name or an alias followed by a "." before the column name
SELECT *
FROM employees
INNER JOIN departments
ON employees.department_id = departments.id;
```
#### Namespacing on tables
- We can also select the columns we want from each table by prefixing the column name with the table name
```SQL
SELECT students.name, classes.name
FROM students
INNER JOIN classes ON classes.class_id = students.class_id;
```
#### Left join
- Returns every record from table_a regardless of whether or not they match records in table_b alongside only matching records in table_b
![[left_join.png]]
#### Defining an alias
- In order to make things easier, we can define an alias to use instead of a table's name
```sql
SELECT e.name, d.name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.id;
```
#### Right join
- The opposite of left join. That's all
![[right_join.png]]
#### Full join
- The combination of all three joins
- Returns all the records from both tables regardless of whether or not they have matches
![[full_join.png]]
#### Multiple joins
- You can also join more than two tables together
```sql
SELECT *
FROM employees
LEFT JOIN departments
ON employees.department_id = departments.id
INNER JOIN regions
ON departments.region_id = regions.id;
```
# Performance
#### SQL Indexes
- Finally, getting into indexes
- First thing to know is that indexes are in-memory structures and that they ensure that queries are performant and quick
- They are mostly binary trees that can be stored in RAM or disk making it easy to look up an entire row
- Primary keys are indexed by default
```SQL
CREATE INDEX index_name ON table_name (column_name);
```
- Indexes are commonly named after the columns they are indexing, with an '_idx' suffix
- Also, remember that trees are really fast to search with a O(log(n)) time complexity [[PG trees and tree traversal]]
- Indexes can make lookups super fast, but they do also add some performance overhead and can slow down the DB in other ways. For example, if we index everything, we will end up with hundreds of B-trees in memory, and while insertion and searching are both O(log(n)) in B-trees, you still will have to insert hundreds of times for every newly added record which will ultimately make insertion slower
- As a rule, only add indexes to columns that you know you frequently look up
#### Multi-Column Indexes
- These are indexes that are created for lookups targeting multiple columns
```SQL
CREATE INDEX first_name_last_name_age_idx
ON users (first_name, last_name, age);
```
- Multi-column indexes are sorted by the first column in the index, then the 2nd, the 3rd and so on
- These indexes unlike when we made a multi-column UNIQUE, are not limited to a combination of these columns only, you can actually look up any of the columns in the index separately. However, only the first column gets the first indexing benefit when you look it up alone, while the 2nd, 3rd, and so on will have degraded performance
- So again, another rule, only make a multi-column index when you actually frequently look up a specific group of column together
#### Denormalizing for Speed
- Although we kept hammering down the idea that you should always prioritize normalization in your design [[#Rules of Thumb for Database Design]], that approach does come at a cost, which is speed
- Joining tables, subqueries, aggregations, post-hoc calculations, all that takes time, and at very large scales, they can actually start having a huge performance toll on an application, and a big one at that
- So, this leaves us with a choice, normalize to ensure data integrity, or denormalize to ensure database speed. Ensuring data integrity is still the most important part, so it's still for the best to go prioritize normalization, until speed starts taking a hit, and even then, denormalize with care
#### SQL Injection
- What would happen if we had this SQL for inserting students
```SQL
INSERT INTO students(name) VALUES (?);
```
- And then someone decides to name his son 'Robert'); DROP TABLE students;--?
- We would get
```SQL
INSERT INTO students(name) VALUES ('Robert'); DROP TABLE students;--);
```
- This is an example of SQL injection, where not sanitizing our inputs properly can lead to unintentional queries taking place, like dropping the entire students table
- However, while it's best to be aware of this, modern SQL libraries actually sanitize inputs anyway, and it's not something we really need to worry about these days
- Go's standard library's SQL packages automatically protects against SQL attacks if used properly by sanitizing inputs, so just avoid interpolating user inputs into raw strings, and let your database handle sanitizing the inputs first
# ORM
- Object-Relational Mapping (ORMs) allow you to perform CRUD operations on a database using a traditional programming language instead of SQL.
- They come in the form of framworks or libraries that you use in your backend, and a good example would be [[PG TypeORM |TypeORM]]
- ORMs map database records to in-memory objects
- ORMs trade control for simplicity, and tend to limit you to whatever SQL the ORM is capable of generating and whatever features it provides
- It's also harder to debug with ORMs since you'll need to go through the documentation and framework/library's code to figure out what went wrong with the generated SQL
# Transactions
- The following section in the file assumes the use of kysely and DB2 for i (work reasons)
- Every statement that runs in SQL, is a transaction
- A transaction must be "Commited" in order for it to persist in the database
- SQL drivers actually abstract this fact by setting the flag `autocommit` to true by default
- This means that every statement that runs is immediately commited
```SQL
-- (implicit BEGIN)
INSERT INTO users VALUES ('alice');
COMMIT;  -- done for you
```
```SQL
-- autocommit is ON
BEGIN;                    -- some drivers ignore this, some error
INSERT INTO a VALUES (1); -- COMMITTED immediately!
INSERT INTO b VALUES (2); -- COMMITTED immediately!
ROLLBACK;                 -- does nothing, everything is already saved
```
- If you wanted to run a migration, the migration normally has to "lock" the database as it runs, similar to [[PG Go Main#Mutexes]] which lock a certain function to a specific goroutine to avoid conflicting insertions/deletions
- In order to have a proper multi-statement transaction, `autocommit` needs to be turned off
## Migration locking
### The problem
- You deploy your app to 3 servers. They all boot at the same time. They all run migrate.ts which says "add a column to the users table." Without coordination:
	- Server 1 starts adding the column
	- Server 2 starts adding the column → ERROR (column already exists, or worse, half-applied)
	- Server 3 marks the migration as complete before it's actually done
- A single migrator needs to run at a given point in time
### How a row lock becomes a mutex
- When a transaction does `SELECT ... FOR UPDATE`, the database marks that row as "I'm going to modify this, nobody else touch it." Any other transaction trying to `SELECT ... FOR UPDATE` the same row will **block** (wait) until the first transaction ends.
- The lock protocol is
```SQL
1. BEGIN (autocommit OFF)
2. SELECT is_locked FROM kysely_migration_lock 
   WHERE id='migration_lock' FOR UPDATE WITH RS;
   ↑ If someone else is migrating, this waits here.
3. Run all the migrations.
4. COMMIT  ← this releases the lock.
```
- This protocol utilizes a dedicated locking table
- It's kinda like being the one holding the "talking pillow" (quick Breaking Bad reference)
- Not all databases require a locking table, as something like postgres utilizes a different protocol `advisory locking`
## Isolation Levels: What Your Transaction Can See
- Imagine two transactions running at the same time. What should Transaction A see of Transaction B's uncommitted work?
- The SQL standard defines four answers, from loosest to strictest:
### READ UNCOMMITTED (DB2: UR — Uncommitted Read)
"I'll read whatever's there, even half-finished writes."
- Fastest, but you can see data that later gets rolled back ("dirty reads").
- Useful for approximate analytics where precision doesn't matter.
### READ COMMITTED (DB2: CS — Cursor Stability)
"I only read data that's been committed."
- But if I read the same row twice in my transaction, I might get different values (because another transaction committed in between).
- DB2's default on many platforms.
### REPEATABLE READ (DB2: RS — Read Stability)
"If I read a row, it won't change for the rest of my transaction."
- But new rows matching my query might appear ("phantom reads").
### SERIALIZABLE (DB2: RR — Repeatable Read, confusingly named)
"My transaction behaves as if no other transaction exists."
- Strictest, slowest. No surprises.
### DB2 example
| Kysely             | DB2 |
| ------------------ | --- |
| `read uncommitted` | UR  |
| `read committed`   | CS  |
| `repeatable read`  | RS  |
| `serializable`     | RR  |
- For DB2 transactions, `RS` is the sweet spot, but note that DB2 as per the above table, doesn't utilize the same standard isolation levels nomenclature
## DDL vs DML
- SQL statements split into categories:
	- **DML — Data Manipulation Language**: `INSERT`, `UPDATE`, `DELETE`, `SELECT`. You're changing _data_.
	- **DDL — Data Definition Language**: `CREATE TABLE`, `ALTER TABLE`, `DROP INDEX`. You're changing _structure_ (the schema).
- Migrations are almost always DDL: "add this column, create this index, drop that table."
## Transactional DDL
- The question is: can DDL participate in a transaction like DML does?
```SQL
BEGIN;
CREATE TABLE users (id INT);
INSERT INTO users VALUES (1);
ROLLBACK;  -- does the table still exist?
```
- **PostgreSQL**: No, the table is gone. Fully transactional DDL.
- **MySQL**: Yes, the table still exists. DDL auto-commits. Not transactional.
- **DB2 LUW**: Mostly yes, with caveats.
- **DB2 for i**: Only if the schema is _journaled_.
### Why this matters for migrations
- If DDL is transactional and your migration fails halfway, everything rolls back. Clean.
- If DDL is not transactional and your migration fails halfway, you have **a half-applied migration**. The `users` table was created, but the `INSERT` failed. Now your database is in a state that doesn't match any migration version. This is a nightmare to recover from manually.
- Kysely's `supportsTransactionalDdl` flag tells the migrator: "can I wrap this whole migration in a transaction for safety?" If `true`, it does. If `false`, it runs migrations without that safety net and trusts you to write idempotent migrations (ones that can be retried).
## Returning
```SQL
INSERT INTO users (name) VALUES ('alice');
-- server returns: "1 row affected"
```
- Usually, when we write to the database, it only shows how many rows were affected by the write, but it doesn't tell us much else about the written data
- If the DB driver supports returning, and returning is enabled, the database will return the inserted row as a response
```SQL
-- Postgres
INSERT INTO users (name) VALUES ('alice') RETURNING id;

-- DB2
SELECT id FROM FINAL TABLE (INSERT INTO users (name) VALUES ('alice'));
```
## Connection Pools and Why Pinning Matters

### A pool
- A pool is a set of open database connections your app reuses. Opening a connection is expensive (TCP + authentication), so the pool keeps, say, 10 of them warm.
- When you run a query:
	1. Pool hands you an unused connection.
	2. You run the query.
	3. Pool takes the connection back.
- Your next query might get a **different** connection.
### Why transactions need pinning
- A transaction is a state that lives **inside one connection**. If you do:
```
Pool → gives you connection A → BEGIN, INSERT
Pool → gives you connection B → INSERT  ← different connection!
Pool → gives you connection A → COMMIT
```
- The second INSERT is running outside the transaction. It's not protected.
- **Pinning** means: once you start a transaction, keep using the same connection until you commit or rollback. Kysely does this correctly _if your dialect's `DatabaseConnection` abstraction respects it_. You have to ensure your driver doesn't secretly swap connections.
### Why the migration lock is extra sensitive to this
- The lock lives on a specific connection's transaction. If any statement during the migration accidentally uses a different connection, those statements are not protected by the lock, and can run concurrently with another server's migrations. Chaos.
## Putting It All Together: What Happens During a Migration
```
1. Kysely gets a connection from the pool — call it Conn#3.
2. Driver sets autocommit OFF on Conn#3.
3. On Conn#3: BEGIN transaction at isolation level RS.
4. On Conn#3: SELECT ... FOR UPDATE WITH RS on the lock row.
   → If another server holds it, we wait here.
   → When we get it, we are the only migrator alive.
5. On Conn#3 (still pinned): run migration DDL statements.
6. On Conn#3: update the kysely_migration table to record this version applied.
7. On Conn#3: COMMIT.
   → Lock releases. Other servers can now proceed.
8. Connection returns to pool.
```
- Every arrow there depends on the concepts above working correctly. Pinning fails → lock bypassed. Autocommit on → lock released early. DDL not transactional → half-applied migration on failure. Wrong isolation level → lock doesn't hold.
