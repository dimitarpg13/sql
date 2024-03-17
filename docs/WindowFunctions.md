# Window Functions

## First Example

Let us have the table `films` given with the following schema:
| PK |   name       |  type   |
|----|--------------|---------|
| yes | id           | Integer |
| no  | release_year | Integer |
| no   | rating       | Numeric |

and the following example data:

| id |  release_year  | rating |
|----|----------------|--------|
|  1 |   2015         | 8.00  |
|  2 |   2015         | 8.50   |
|  3 |   2015         | 9.00  |
|  4 |   2016         | 8.20  |
|  5 |   2016         | 8.40  |
|  6 |   2017         | 7.00  |

*First Question*: For each film find the average rating of all films in its release year.

This will result in the following table:

| id  | release_year  | rating  | year_avg  |
|-----|---------------|---------|-----------|
|  1  |   2015        |  8.00   |  8.50     |
|  2  |   2015        |  8.50   |  8.50     |
|  3  |   2015        |  9.00   |  8.50     |
|  4  |   2016        |  8.20   | 8.30      |
| 5   |   2016        |  8.40   | 8.30      |
|  6  |   2017        |  7.00   | 7.00      |

One way to approach this problem is to use subquery averages for all distinct years and joining the back with query fetching all films:

```sql
SELECT
  f.id, f.release_year, f.rating, years.year_avg
FROM films f
LEFT JOIN (
  SELECT f.release_year, AVG(rating) AS year_avg
  FROM films f
  GROUP BY f.release_year
) years ON f.release_year = years.release_year
```
*Second Question*: For each film find average ratings for all films released in the same year and separately in the same category

This will result in the following table:

| id | release_year | category_id | rating | year_avg  | category_avg |
|----|--------------|-------------|--------|-----------|--------------|
| 1  |     2015     |    1        |  8.00  |   8.50    |    8.20      |
| 2  |     2015     |    2        |  8.50  |   8.50    |    7.90      |
| 3  |     2015     |    3        |  9.00  |   8.50    |    9.00      |
| 4  |     2016     |    2        |  8.20  |   8.30    |    7.90      |
| 5  |     2016     |    1        |  8.40  |   8.30    |    8.20      |
| 6  |     2017     |    2        |  7.00  |   7.00    |    7.90      |

We have to count the category averages in a similar way and join them together with the previous query:

```sql
SELECT
  f.id, f.release_year, f.category_id, f.rating, years.year_avg, categories.category_avg
FROM films f
LEFT JOIN (
  SELECT f.release_year, AVG(rating) AS year_avg
  FROM films f
  GROUP BY f.release_year
) years ON f.release_year = years.release_year
LEFT JOIN (
   SELECT f.category_id, AVG(rating) AS category_avg
   FROM films f
   GROUP BY f.category_id
) categories ON f.category_id = categories.category_id
```
Note: the pattern so far has been - we select a set of rows from the table and then join them with aggregated versions of the same row set. But we can't just use the `GROUP BY` in the main query because we want to get the full list of films as a result. Thus we have to copy-paste the main query to each subquery. This looks like a sub-optimal arrangement - ideally we would like to have a way to do computations on a row set, but not altering it at the same time.

## Enter the Window Functions...

Definition of window functions from Postgres documentation:

> _A window function performs a calculation across a set of table rows that are somehow related to the current row. This is  comparable to the type of calculation that can be done with an aggregate function_
