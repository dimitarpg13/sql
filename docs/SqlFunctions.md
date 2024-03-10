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
