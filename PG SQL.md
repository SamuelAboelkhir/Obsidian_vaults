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
	-  [MongoDB](https://en.wikipedia.org/wiki/MongoDB)
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
#### UPDATE
```SQL
UPDATE employees
SET job_title = 'Backend Engineer', salary = 150000
WHERE id = 251;
```
# Clauses/Keywords
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
# Logical Operators
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
# ORM
- Object-Relational Mapping (ORMs) allow you to perform CRUD operations on a database using a traditional programming language instead of SQL.
- They come in the form of framworks or libraries that you use in your backend, and a good example would be [[PG TypeORM |TypeORM]]
- ORMs map database records to in-memory objects
- ORMs trade control for simplicity, and tend to limit you to whatever SQL the ORM is capable of generating and whatever features it provides
- It's also harder to debug with ORMs since you'll need to go through the documentation and framework/library's code to figure out what went wrong with the generated SQL