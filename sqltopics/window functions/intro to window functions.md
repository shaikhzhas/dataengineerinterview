
## Intro to window functions
### [Query 1](#1)
### [Query 2](#2)
### [Query 3](#3)


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