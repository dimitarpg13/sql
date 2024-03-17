# Common Table Expressions (CTEs)

CTEs are a standard SQL feature supported across many platforms. With CTE complex queries can be broken into simple, modular parts that are easy to read and maintain. Snowflake supports CTEs and provides additional optimizations to enhance their performance.

## CTEs in SnowSQL (Snowflake) 
This exposition is about SnowSQL CTEs and how to optimize those for best performance.  

CTE is a temporary named result set that exist within the scope of a single statement and that can be referred to later within that statement, possibly multiple times. 

SnowSQL CTE is defined using the `WITH` clause followed by the CTE name and a query that defines the CTE. The CTE can then be used like a regular table or view in a `SELECT`, `INSERT`, or `DELETE` statement.

SnowSQL CTE is particularly useful when working with recursive queries - queries that reference themselves. Recursive CTEs can be used to perform complez data processing tasks that would otherwise require multiple separate SQL statements. In Snowflake, CTEs are not materialized - they are not stored in the database but are re-evaluated upon each reference, offering a flexible approach to deconstructing complex queries.

Here is a quick example of a Snowflake CTE:

```sql
WITH CTEs_students AS (
   SELECT student_id, first_name, last_name
   FROM students
)
SELECT *
FROM CTE_students
WHERE class_id = 100;
```

The SnowSQL CTE first creates a temporary view named `CTE_students` that contains the `student ID`, `first name`, and `last name` of all students in the `students` table. Then, the CTEs are used in the outer `SELECT` statement to select all of the rows from the temporary view where the student ID is 100.

Keep in mind that Snowflake CTE can be separated by commas - allowing us to define multiple by simply using comma (,) delimiters.

```sql
WITH
   cte1 AS (
      SELECT ...
   ),
   cte2 AS (
      SELECT ...
   )
SELECT *
FROM cte1
JOIN cte2
```
Also, one can nest Snowflake CTE within other CTEs:
```
WITH outer_cte AS (
   WITH inner_cte AS (
      SELECT 'Hello from Dimitar' AS greeting
   )
   SELECT greeting FROM inner_cte
)

SELECT *
FROM outer_cte;
```
