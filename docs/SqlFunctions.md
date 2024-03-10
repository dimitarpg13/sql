# SQL Functions

## ANY_VALUE
returns some value of the expression from the group. The result is non-deterministic.

### Syntax

*Aggregate function*:

```sql
ANY_VALUE( [ DISTINCT ] <expr1> )
```

*Window function*:

```sql
ANY_VALUE( [ DISTINCT ] <expr1> ) OVER ( [ PARTITION BY <expr2> ] )
```

### Usage notes

The `DISTINCT` keyword can be specified for this function, but it does not have any effect.
When used as a window function this function does not support:
* `ORDER By` sub-clause in the `OVER()` clause
* Window frames
