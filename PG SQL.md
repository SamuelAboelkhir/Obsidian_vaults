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
#### SELECT
```SQL
-- All fields
SELECT * FROM <table>;

-- Multiple fields
SELECT name, balance FROM users;
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