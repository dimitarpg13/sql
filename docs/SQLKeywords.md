# Relevant SQL Keywords


## QUALIFY

`QUALIFY` is a clause used to filter the results of a window function. Therefore, to successfully use the `QUALIFY` clause, there must be at least one `WINDOW` function in the `SELECT` list or `QUALIFY` clause - only the rows where the boolean expression evaluates to `TRUE` will be returned. 
