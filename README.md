# Basic of SQL
---
## Introduction
SQL(Structured Query Language), is a language designed to allow both technical and non-technical users query, manipulate, and transform data from a relational database.

## Relational Databases
A relational database represents a collection of related (two-dimensional) tables.
for ex: 
**TableName:- Inventory**
| id | name | quantity | price|
|----|------|----------|------|
| 1  | ASUS | 10       | 50000|
| 2 | HP |  15       | 50000| 
<br>

## SQL Queries
---
### What is queries
- Statement which declares what data we are looking for
- Where to find it in the database.
- How to transform it before it is returned(optional)
### SELECT
- To retrieve all the data from a SQL Table.

```sql
SELECT * FROM tablename;
```

- To retrieve the data from a Specific columns.
```sql
SELECT columname1, columname2, …
FROM tablename;
```
### SELECT DISTINCT
- To retrive all the distinct element
```sql
SELECT DISTINCT columnname FROM tablename;
```

##### Questions:
1. Find the name of each product.
2. Retrive all the data from table.

### Queries with constraints :
#### 1. WHERE clause
- To filter certain results.
```sql
SELECT columname1, columnname2, …
FROM tableName
WHERE condition
    AND/OR another_condition
    AND/OR …;
```
- Important Operators

| Operator | Condition | SQL Example
|----|------|----------|
|=, !=, < <=, >, >= | Standard numerical operators | col_name != 4  |
| BETWEEN … AND … | Number is within range of two values (inclusive) |  col_name BETWEEN 1 AND 10    | 
|NOT BETWEEN … AND …| Number is not within range of two values (inclusive) | col_name NOT BETWEEN 1 AND 10|
|IN (…)| Number exists in a list| col_name IN (2, 4, 6)|
|NOT IN (…)	| Number does not exist in a list| col_name NOT IN (1, 3, 5)|
| LIKE |Case insensitive exact string comparison|col_name LIKE "ABC"|
| NOT LIKE | Case insensitive exact string inequality comparison| col_name NOT LIKE "ABCD" |
|%|Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE)| col_name LIKE "%AT%"(matches "AT", "ATTIC", "CAT" or even "BATS")|
|_|Used anywhere in a string to match a single character (only with LIKE or NOT LIKE)| col_name LIKE "AN_" (matches "AND", but not "AN")|

##### Questions :- 
- Find out the price of Asus.
- Find out total sale for a date '20-08-2021'.
- Find out all the product in price range from 30000 to 80000.

#### 2. ORDER BY clause.
- To sort the result in ascending or descending order.
```sql
SELECT columname
FROM mytable
ORDER BY columname ASC/DESC;
```
##### Questions :-
- Sort all the product by price(ASC and DESC both)

#### 3. LIMIT clause and  OFFSET clause
- The **LIMIT** is used to reduce the number of rows to return. 
- **OFFSET** will specify where to begin counting the number rows from.
```sql
SELECT columname
FROM mytable
ORDER BY columname ASC/DESC;
LIMIT num_limit OFFSET num_offset;
```
##### Questions :-
- Find out the most expensive product.
- Find out 2nd and 3rd most expensive product.

#### 4. FETCH clause
- The FETCH clause is functionally equivalent to the LIMIT clause.
- But it followed SQL standard, so it makes application compatible with other database
- ROW is synonyms for ROWS.
- FIRST is synonyms for NEXT.
```sql
OFFSET start { ROW | ROWS }
FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY

```
### SQL Joins
---
A JOIN clause is used to combine rows from two or more tables, based on a related column between them.
Types of joins:-
- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN

#### 1. INNER JOIN
The INNER JOIN keyword selects records that have matching values in both tables.

```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;
```
##### Questions :-
- Perform inner join on inv.products and inv.sales.
- Find out sales for each product.

#### 2. LEFT JOIN
The LEFT JOIN keyword returns all records from the left table (table1), and the matching records from the right table (table2). The result is nulll records from the right side, if there is no match.
```sql
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;
```
##### Questions :-
- Find out the unsold products.

#### 3. RIGHT JOIN
The RIGHT JOIN keyword returns all records from the right table (table2), and the matching records from the left table (table1). The result is null records from the left side, if there is no match.
```sql
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;
```
#### 4. FULL JOIN
The FULL OUTER JOIN keyword returns all records when there is a match in left (table1) or right (table2) table records.
```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;
```
#### 5. SELF JOIN
- A self join is a regular join, but the table is joined with itself.


### Queries with expressions
---
- Transform the values when when the query is executed
- For ex.
  - Add 10% discount on price
```sql
SELECT price,(price/10) as discount
FROM inv.products

```
### GROUP by 
The GROUP BY statement groups rows that have the same values into summary rows, like "find the number of customers in each country".

The GROUP BY statement is often used with aggregate functions (COUNT(), MAX(), MIN(), SUM(), AVG()) to group the result-set by one or more columns.
```sql
SELECT employee
FROM emp
GROUP BY employee;

```
### Aggregation
List of some common aggregate functions that we are going to use in our examples:

| Function  | Description |
|--------|-----|
|COUNT(*), COUNT(column)|	A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column.|
|MIN(column)|	Finds the smallest numerical value in the specified column for all rows in the group.|
|MAX(column) |	Finds the largest numerical value in the specified column for all rows in the group.|
|AVG(column)|	Finds the average numerical value in the specified column for all rows in the group.|
|SUM(column)|	Finds the sum of all numerical values in the specified column for the rows in the group.|
- find the total sale of all products
```sql
    SELECT name,sum(inv.sales.sales_quantity) as total 
    FROM inv.products
    INNER JOIN inv.sales
    ON inv.products.id = inv.sales.pid 
    GROUP BY name

```
- Total sale for Asus
```sql
    SELECT name,sum(inv.sales.sales_quantity) as total 
    FROM inv.products
    INNER JOIN inv.sales
    ON inv.products.id = inv.sales.pid 
    GROUP BY name
    HAVING name like 'Asus';
```
##### Questions :- 
- Find minimum price 
- Find the product name with maximum price.

### Modifying data
#### 1. Create Database
```sql
CREATE DATABASE databasename;
```
#### 2. Drop Database
- The DROP DATABASE statement is used to drop an existing SQL database.
```sql
DROP DATABASE databasename;
```
#### 3. Create Table
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
```
##### List of Datatype
| Data Type | Description |
|----|----|
|INTEGER, BOOLEAN|	The integer datatypes can store whole integer Values.The boolean value is just represented as an integer value of just 0 or 1.|
| FLOAT, DOUBLE, REAL|	The floating point datatypes can store fractional values. |
|CHARACTER(num_chars), VARCHAR(num_chars), TEXT	| The text based datatypes can store strings and text in all sorts of locales.|
|DATE, DATETIME	|SQL can also store date and time stamps to keep track of time series and event data.|
##### Table Constraints
|Constraint | Description |
|---|---|
|PRIMARY KEY|	This means that the values in this column are unique, and each value can be used to identify a single row in this table.|
|AUTOINCREMENT|	For integer values, this means that the value is automatically filled in and incremented with each row insertion. Not supported in all databases.|
|UNIQUE	|This means that the values in this column have to be unique, so you can't insert another row with the same value in this column as another row in the table. Differs from the `PRIMARY KEY` in that it doesn't have to be a key for a row in the table.|
|NOT NULL	|This means that the inserted value can not be `NULL`.|
|CHECK (expression)	|This allows you to run a more complex expression to test whether the values inserted are valid. For example, you can check that values are positive, or greater than a specific size, or start with a certain prefix, etc.|
|FOREIGN KEY	|This is a consistency check which ensures that each value in this column corresponds to another value in a column in another table.|

#### 4. DROP TABLE
- To delete the table
```sql
DROP TABLE table_name;
```
#### 5. TRUNCATE TABLE
- The TRUNCATE TABLE statement is used to delete the data inside a table, but not the table itself.

```sql
TRUNCATE TABLE table_name;
```
#### 6. INSERT INTO
- The INSERT INTO statement is used to insert new records in a table.

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```
#### 7. ALTER TABLE
- The ALTER TABLE statement is used to add, delete, or modify columns in an existing table.
- The ALTER TABLE statement is also used to add and drop various constraints on an existing table.

##### ALTER TABLE - ADD Column
```sql
ALTER TABLE table_name
ADD column_name datatype;
```
##### ALTER TABLE - DROP COLUMN

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```
##### ALTER TABLE - MODIFY COLUMN
- for psql
```sql
ALTER TABLE table_name
ALTER COLUMN  column_name Type datatype;
```

<h1 align="center"> Thank You </h1>
