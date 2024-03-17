# Aggregate Functions 

Aggregate functions operate on values across rows to perform mathematical calculations (e.g. sum, average, counting, min/max value, standard deviation) or non-mathematical calculations.

An aggregate function takes multiple rows (zero or more rows) as input and produces a scalar output. In contrast, scalar functions take one row as input and produce scalar value.

An aggregate function always returns exactly one row, **_even if the input contains zero rows_**. Typically, if the input contained zero rows, the output is `NULL`. However, an aggregate function could return 0, an empty string, or some other value when passed zero rows. 

## ANY_VALUE

Returns some value of the expression from the group. The result is non-deterministic.

### Syntax (SnowSQL)

#### Aggregated function ANY_VALUE

```sql
ANY_VALUE( [ DISTINCT ] <expr1> )
```

#### Window function ANY_VALUE

```sql
ANY_VALUE( [ DISTINCT ] <expr1> ) OVER ( [ PARTITION BY <expr2> ] )
```

### Usage Notes (SnowSQL)

* The `DISTINCT` keyword can be specified for this function, but it does not have any effect.
* when used as a window function this function does not support:
    * `ORDER BY` sub-clause in the `OVER()` clause
    * window frames 

### Using ANY_VALUE with GROUP BY Statements (SnowSQL)

`ANY_VALUE` can simplify and optimize the performance of `GROUP BY` statements. A common problem for many queries is that the result of a query with a `GROUP BY` clause can only contain expressions used in the `GROUP BY` clause itself, or results of aggregate functions. For example:

```sql
SELECT customer.id, customer.name, SUM(orders.value)
    FROM customer
    JOIN orders ON customer.id = orders.customer_id
    GROUP BY customer.id, customer.name;
```
In this query, the `customer.name` attribute needs to be in the `GROUP BY` to be included in the result. This is unnecessary (e.g. when `customer.id` is known to be unique) and makes the computation possibly more complex and slower. Another option is to use an aggregate function :

```sql
SELECT customer.id, MIN(customer.name), SUM(orders.value)
    FROM customer
    JOIN orders ON customer.id = orders.customer_id
    GROUP BY customer.id;
```
This simplifies the `GROUP BY` clause, but still requires computing the `MIN` function, which incurs an extra cost.

With `ANY_VALUE`, you can execute the following query:
```sql
SELECT customer.id, ANY_VALUE(customer.name), SUM(orders.value)
    FROM customer
    JOIN orders ON customer.id = orders.customer_id
    GROUP BY customer.id;
```
