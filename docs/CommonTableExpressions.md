# Common Table Expressions (CTEs)

CTEs are a standard SQL feature supported across many platforms. With CTE complex queries can be broken into simple, modular parts that are easy to read and maintain. Snowflake supports CTEs and provides additional optimizations to enhance their performance.

## CTEs in SnowSQL (Snowflake) 
This exposition is about SnowSQL CTEs and how to optimize those for best performance.  

CTE is a temporary named result set that exist within the scope of a single statement and that can be referred to later within that statement, possibly multiple times. 

SnowSQL CTE is defined using the `WITH` clause followed by the CTE name and a query that defines the CTE. The CTE can then be used like a regular table or view in a `SELECT`, `INSERT`, or `DELETE` statement.

SnowSQL CTE is particularly useful when working with recursive queries - queries that reference themselves. Recursive CTEs can be used to perform complez data processing tasks that would otherwise require multiple separate SQL statements. In Snowflake, CTEs are not materialized - they are not stored in the database but are re-evaluated upon each reference, offering a flexible approach to deconstructing complex queries.

Here is a quick example of a Snowflake CTE:

```sql
/** Snowflake Common Table Expression example **/
WITH CTEs_students AS (
   SELECT student_id, first_name, last_name
   FROM students
)
SELECT *
FROM CTE_students
WHERE class_id = 100;
```
