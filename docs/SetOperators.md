# Set Operators

Set operators allow queries to be combined.

## General Syntax

```sql
[ ( ] <query> [ ) ] ( INTERSECT  |  ( MINUS | EXCEPT ) | UNION [ ALL ] ) [ ( ] <query> [ ) ]
[ ORDER BY . . . ]
[ LIMIT . . . ]
```

## General Usage Notes

* Each query can itself contain query operators to allow any number of query expressions to be combined with set oeprators
* The `ORDER BY` and `LIMIT` / `FETCH` clauses are applied to the result of the set operator.
* When using these operators:
    * make sure that each query selects the same number of columns
    * make sure that the data type of each column is consistent across the rows from different sources.
    * in general, the semantics, as well as the data types, of the columns should match. The following will not produce the desired results:
      ```sql
      SELECT LastName, FirstName FROM employees
      UNION ALL
      SELECT FirstName, LastName FROM conractors;
      ```
      The risk of error increases when using the asterisk to specify all columns of a table:
      ```sql
      SELECT * FROM table1
      UNION ALL
      SELECT * FROM table2; 
      ```
      If the tables have the same number of columns, but the columns are not in the same order, the query results will probably be incorrect.
      
