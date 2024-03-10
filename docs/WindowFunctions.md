# Windowing Functions

A window function operates on a group ("window") of related rows.

Each time a window function is called, it is passed a row (the current row in the window) and the window of rows that contain the current row. The window function returns one output row for each input row. The output depends on the individual row passed to the function and the values of the other rows in the window passed to the function. 

## First Example

Let us have the table `films` given with the following schema:

|      films                  |
|-----------------------------|
| PK | id           | Integer |
|----|--------------|---------|
|    | release_year | Integer |
|    | rating       | Numeric |


