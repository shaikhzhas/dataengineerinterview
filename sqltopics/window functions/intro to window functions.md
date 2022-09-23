
## Intro to window functions

A **window function** performs a calculation across a set of table rows that are somehow related to the current row. This is comparable to the type of calculation that can be done with an aggregate function. But unlike regular aggregate functions, use of a window function does not cause rows to become grouped into a single output row — the rows retain their separate identities. Behind the scenes, the window function is able to access more than just the current row of the query result.

The `ORDER` and `PARTITION` define what is referred to as the "window"—the ordered subset of data over which calculations are made.

**Note**: You can't use window functions and standard aggregations in the same query. More specifically, you can't include window functions in a `GROUP BY` clause.

### [Query 1](#1)
### [Query 2](#2)
### [Query 3](#3)
### [Query 4](#4)
### [Query 5](#5)
### [Query 6](#6)
### [Query 7](#7)
### [Query 8](#8)
### [Query 9](#9)



### <a name="1"></a>Query 1
```sql
SELECT
  start_time,
  duration_seconds,
  SUM(duration_seconds) OVER (
    ORDER BY
      start_time
  ) AS running_total
FROM
  tutorial.dc_bikeshare_q1_2012
ORDER BY
  start_time
```
![alt text](../../images/w_q1.png "Window Q1")

### <a name="2"></a>Query 2

```sql
SELECT
  start_terminal,
  start_time,
  duration_seconds,
  SUM(duration_seconds) OVER (
    PARTITION BY start_terminal
    ORDER BY
      start_time
  ) AS running_total
FROM
  tutorial.dc_bikeshare_q1_2012
WHERE
  start_time < '2012-01-08'
ORDER BY
  start_terminal,
  start_time
```
![alt text](../../images/w_q2.png "Window Q2")

### <a name="3"></a>Query 3
```sql
SELECT
  start_terminal,
  duration_seconds,
  SUM(duration_seconds) OVER (PARTITION BY start_terminal) AS start_terminal_total
FROM
  tutorial.dc_bikeshare_q1_2012
WHERE
  start_time < '2012-01-08'
```
![alt text](../../images/w_q3.png "Window Q3")

### <a name="4"></a>Query 4
Write a query modification of the above example query that shows the duration of each
ride as a percentage of the total time accrued by riders from each start_terminal
```sql
SELECT
  start_time,
  start_terminal,
  duration_seconds,
  SUM(duration_seconds) OVER (PARTITION BY start_terminal) AS start_terminal_sum,
  (
    duration_seconds / SUM(duration_seconds) OVER (PARTITION BY start_terminal)
  ) * 100 AS pct_of_total_time
FROM
  tutorial.dc_bikeshare_q1_2012
WHERE
  start_time < '2012-01-08'
ORDER BY
  2,
  1,
  4 DESC
```
![alt text](../../images/w_q4.png "Window Q4")

### <a name="5"></a>Query 5
```sql
SELECT
  start_terminal,
  duration_seconds,
  SUM(duration_seconds) OVER (PARTITION BY start_terminal) AS running_total,
  COUNT(duration_seconds) OVER (PARTITION BY start_terminal) AS running_count,
  AVG(duration_seconds) OVER (PARTITION BY start_terminal) AS running_avg
FROM
  tutorial.dc_bikeshare_q1_2012
WHERE
  start_time < '2012-01-08'
```
![alt text](../../images/w_q5.png "Window Q5")

### <a name="6"></a>Query 6
```sql
SELECT
  start_terminal,
  start_time,
  duration_seconds,
  SUM(duration_seconds) OVER (
    PARTITION BY start_terminal
    ORDER BY
      start_time
  ) AS running_total,
  COUNT(duration_seconds) OVER (
    PARTITION BY start_terminal
    ORDER BY
      start_time
  ) AS running_count,
  AVG(duration_seconds) OVER (
    PARTITION BY start_terminal
    ORDER BY
      start_time
  ) AS running_avg
FROM
  tutorial.dc_bikeshare_q1_2012
WHERE
  start_time < '2012-01-08'
ORDER BY
  1,
  2
```
![alt text](../../images/w_q6.png "Window Q6")

### <a name="7"></a>Query 7
Write a query that shows a running total of the duration of bike rides (similar to the last example), but grouped by end_terminal, and with ride duration sorted in descending order
```sql
SELECT
  end_terminal,
  duration_seconds,
  SUM(duration_seconds) OVER (
    PARTITION BY end_terminal
    ORDER BY
      duration_seconds DESC
  ) AS running_total
FROM
  tutorial.dc_bikeshare_q1_2012
WHERE
  start_time < '2012-01-08'
```
![alt text](../../images/w_q7.png "Window Q7")
