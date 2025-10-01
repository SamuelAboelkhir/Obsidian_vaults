---
tags: 
- Other
- PG
MOC: Technology
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[_0006 Programming MOC|Back to index]]
-  Acquired from [https://www.datacamp.com/cheat-sheet/postgre-sql-basics-cheat-sheet](https://www.datacamp.com/cheat-sheet/postgre-sql-basics-cheat-sheet)
![](https://media.datacamp.com/legacy/image/upload/v1700745416/Postgre_SQL_Cheat_Sheet_3432a8648a.png)

Have this cheat sheet at your fingertips

[Download PDF](https://media.datacamp.com/legacy/image/upload/v1700745477/Marketing/Blog/PostgreSQL_Cheat_Sheet.pdf)

## What is PostgreSQL?

PostgreSQL is an open-source relational database management system (RDBMS) known for its extensibility and standards compliance. Developed and maintained by a group of volunteers known as The PostgreSQL Global Development Group, it is popular across a wide range of organizations from enterprises to government departments. It has powerful data analysis capabilities.

## Sample Data

The dataset contains details of the world's fastest production cars by 0 to 60 mph acceleration time. Each row contains one car model, and the table is named cars.

| **make** | **model** | **year** | **propulsion\_type** | **time\_to\_60\_mph\_s** | **limited\_production\_count** |
| --- | --- | --- | --- | --- | --- |
| Lamborghini | Huracán Performante | 2018 | ICE | 2.2 |  |
| Ferrari | SF90 Stradale | 2021 | Hybrid | 2 |  |
| Tesla | Model S Plaid | 2021 | Electric | 1.98 |  |
| Porsche | 918 Spyder | 2015 | Hybrid | 2.1 | 918 |
| Rimac | Nevera | 2021 | Electric | 1.74 | 150 |
| Porsche | 911 Turbo S (992) | 2020 | ICE | 2.1 |  |

## Querying tables

Get all the columns from a table using `SELECT *`

```sql
SELECT *
    FROM cars
```

Get a column from a table by name using `SELECT col`

```sql
SELECT model
    FROM cars
```

Get multiple columns from a table by name using `SELECT col1, col2`

```sql
SELECT make, model
    FROM cars
```

Override column names with `SELECT col AS new_name`

```sql
SELECT make, model, propulsion_type AS engine_type
    FROM cars
```

Arrange the rows in ascending order of values in a column with `ORDER BY col`

```sql
SELECT  make, model, time_to_60_mph_s
    FROM cars
    ORDER BY time_to_60_mph_s
```

Arrange the rows in descending order of values in a column with `ORDER BY col DESC`

```sql
SELECT make, model, model_year
    FROM cars
    ORDER BY model_year DESC
```

Limit the number of rows returned with `LIMIT n`

```sql
SELECT *
    FROM cars
LIMIT 2
```

Get unique values with `SELECT DISTINCT`

```sql
SELECT DISTINCT propulsion_type
    FROM cars
```

## Filtering Data

### Filtering on numeric columns

Get rows where a number is greater than a value with `WHERE col > n`

```sql
SELECT make, model, time_to_60_mph_s
    FROM cars
    WHERE time_to_60_mph_s > 2.1
```

Get rows where a number is greater than or equal to a value with `WHERE col >= n`

```sql
SELECT make, model, time_to_60_mph_s
    FROM cars
    WHERE time_to_60_mph_s >= 2.1
```

Get rows where a number is less than a value with `WHERE col < n`

```sql
SELECT make, model, time_to_60_mph_s
    FROM cars
    WHERE time_to_60_mph_s < 2.1
```

Get rows where a number is less than or equal to a value with `WHERE col <= n`

```sql
SELECT make, model, time_to_60_mph_s
    FROM cars
    WHERE time_to_60_mph_s <= 2.1
```

Get rows where a number is equal to a value with `WHERE col = n`

```sql
SELECT make, model, time_to_60_mph_s
    FROM cars
    WHERE time_to_60_mph_s = 2.1
```

Get rows where a number is not equal to a value with `WHERE col <> n` or `WHERE col != n`

```sql
SELECT make, model, time_to_60_mph_s
    FROM cars
    WHERE time_to_60_mph_s <> 2.1
```

Get rows where a number is between two values (inclusive) with `WHERE col BETWEEN m AND n`

```sql
SELECT make, model, time_to_60_mph_s
    FROM cars
    WHERE time_to_60_mph_s BETWEEN 1.9 AND 2.1
```

### Filtering on text columns

Get rows where text is equal to a value with `WHERE col = 'x'`

```sql
SELECT make, model, propulsion_type
    FROM cars
    WHERE propulsion_type = 'Hybrid'
```

Get rows where text is one of several values with `WHERE col IN ('x', 'y')`

```sql
SELECT make, model, propulsion_type
    FROM cars
    WHERE propulsion_type IN ('Electric',  'Hybrid')
```

Get rows where text contains specific letters with `WHERE col LIKE '%abc%'` (% represents any characters)

```sql
SELECT make, model, propulsion_type
    FROM cars
    WHERE propulsion_type LIKE '%ic%'
```

For case insensitive matching, use `WHERE col ILIKE '%abc%'`

```sql
SELECT make, model, propulsion_type
    FROM cars
    WHERE propulsion_type ILIKE '%ic%'
```

### Filtering on multiple columns

Get the rows where one condition and another condition holds with `WHERE condn1 AND condn2`

```sql
SELECT make, model, propulsion_type, model_year
    FROM cars
    WHERE propulsion_type = 'Hybrid'
        AND model_year < 2020
```

Get the rows where one condition or another condition holds with `WHERE condn1 OR condn2`

```sql
SELECT make, model, propulsion_type, model_year
    FROM cars
    WHERE propulsion_type = 'Hybrid'
        OR model_year < 2020
```

### Filtering on missing data

Get rows where values are missing with `WHERE col IS NULL`

```sql
SELECT make, model, limited_production_count
    FROM cars
    WHERE limited_production_count IS NULL
```

Get rows where values are not missing with `WHERE col IS NOT NULL`

```sql
SELECT make, model, limited_production_count
    FROM cars
    WHERE limited_production_count IS NOT NULL
```

## Aggregating Data

### Simple aggregations

Get the total number of rows `SELECT COUNT(*)`

```sql
SELECT COUNT(*)
    FROM cars
```

Get the total value of a column with `SELECT SUM(col)`

```sql
SELECT SUM(limited_production_count)
    FROM cars
```

Get the mean value of a column with `SELECT AVG(col)`

```sql
SELECT AVG(time_to_60_mph_s)
    FROM cars
```

Get the minimum value of a column with `SELECT MIN(col)`

```sql
SELECT MIN(time_to_60_mph_s)
    FROM cars
```

Get the maximum value of a column with `SELECT MAX(col)`

```sql
SELECT MAX(time_to_60_mph_s)
    FROM cars
```

### Grouping, filtering, and sorting

Get summaries grouped by values with `GROUP BY col`

```sql
SELECT propulsion_type, COUNT(*)
    FROM cars
    GROUP BY propulsion_type
```

Get summaries grouped by values, in order of summaries with `GROUP BY col ORDER BY smmry`

```sql
SELECT propulsion_type, AVG(time_to_60_mph_s) AS mean_time_to_60_mph_s
    FROM cars
    GROUP BY propulsion_type
    ORDER BY mean_time_to_60_mph_s
```

Get rows where values in a group meet a criterion with `GROUP BY col HAVING condn`

```sql
SELECT propulsion_type, AVG(time_to_60_mph_s) AS mean_time_to_60_mph_s
    FROM cars
    GROUP BY propulsion_type
    HAVING mean_time_to_60_mph_s > 2
```

Filter before and after grouping with WHERE condn\_before `GROUP BY col HAVING condn_after`

```sql
SELECT propulsion_type, AVG(time_to_60_mph_s) AS mean_time_to_60_mph_s
    FROM cars
WHERE limited_production_count IS NOT NULL
    GROUP BY propulsion_type
    HAVING mean_time_to_60_mph_s > 2
```

## PostgreSQL-Specific Syntax

Not all code works in every dialect of SQL. The following examples work in PostgreSQL, but are not guaranteed to work in other dialects.

Limit the number of rows returned, offset from the top with `LIMIT m OFFSET n`

```sql
SELECT *
    FROM cars
LIMIT 2 OFFSET 3
```

PostgreSQL allows text concatenation with the `||` operator

```sql
SELECT make || ' ' || model AS make_and_model
     FROM cars
```

Get the current date with `CURRENT_DATE` and the current datetime with `NOW()` or `CURRENT_TIME`

```sql
SELECT NOW(), CURRENT_DATE, CURRENT_TIME
```

List available tables by selecting from `pg_catalog.pg_tables`

```sql
SELECT * FROM pg_catalog.pg_tables
```

---

![Richie Cotton's photo](https://media.datacamp.com/cms/richie-sq.jpeg?w=128)

Author

[Richie Cotton](https://www.datacamp.com/portfolio/richie)

Richie helps individuals and organizations get better at using data and AI. He's been a data scientist since before it was called data science, and has written two books and created many DataCamp courses on the subject. He is a host of the DataFramed podcast, and runs DataCamp's webinar program.

Topics

Related

![](https://media.datacamp.com/legacy/v1649271684/download_e63daf5fcf.png?w=750)SQL Basics Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/sql-basics-cheat-sheet)

With this SQL cheat sheet, you'll have a handy reference guide to basic querying tables, filtering data, and aggregating data

Richie Cotton

![](https://media.datacamp.com/legacy/v1698139537/My_SQL_Cheat_Sheet_f2d0d7da20.png?w=750)MySQL Basics Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/my-sql-basics-cheat-sheet)

With this MySQL basics cheat sheet, you'll have a handy reference guide to basic querying tables, filtering data, and aggregating data

Richie Cotton

![](https://media.datacamp.com/cms/joining-data-in-sql-updated-0425.png?w=750)SQL Joins Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/sql-joins-cheat-sheet)

With this SQL Joins cheat sheet, you'll have a handy reference guide to joining data in SQL.

Richie Cotton

Beginner's Guide to PostgreSQL

Tutorial

[View original](https://www.datacamp.com/tutorial/beginners-introduction-postgresql)

In this tutorial, you will learn how to write simple SQL queries in PostgreSQL.

Sayak Paul

Working with Spreadsheets in SQL

Tutorial

[View original](https://www.datacamp.com/tutorial/working-spreadsheets-sql)

In this tutorial, learn how to import a spreadsheet into PostgreSQL and perform analysis on it.

Sayak Paul

10 Command-line Utilities in PostgreSQL

Tutorial

[View original](https://www.datacamp.com/tutorial/10-command-line-utilities-postgresql)

In this tutorial, learn about 10 handy command-line utilities in PostgreSQL which can enable you to interact with databases efficiently.

Sayak Paul