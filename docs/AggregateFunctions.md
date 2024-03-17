# Aggregate Functions 

Aggregate functions operate on values across rows to perform mathematical calculations (e.g. sum, average, counting, min/max value, standard deviation) or non-mathematical calculations.

An aggregate function takes multiple rows (zero or more rows) as input and produces a scalar output. In contrast, scalar functions take one row as input and produce scalar value.

An aggregate function always returns exactly one row, **_even if the input contains zero rows_**. Typically, if the input contained zero rows, the output is `NULL`. However, an aggregate function could return 0, an empty string, or some other value when passed zero rows. 

## ANY_VALUE

Returns some value of the expression from the group. The result is non-deterministic.

### Syntax (specific to SnowSQL)

#### Aggregated function ANY_VALUE

```sql
ANY_VALUE( [ DISTINCT ] <expr1> )
```

#### Window function ANY_VALUE

```sql
ANY_VALUE( [ DISTINCT ] <expr1> ) OVER ( [ PARTITION BY <expr2> ] )
```

### Usage Notes (specific to SnowSQL)

* The `DISTINCT` keyword can be specified for this function, but it does not have any effect.
* when used as a window function this function does not support:
  ** `ORDER BY` sub-clause in the `OVER()` clause
  ** window frames 

