
## Intro to window functions

### Query 1

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
![alt text](https://github.com/shaikhzhas/dataengineerinterview/images/w_q1.png "Window Q1")
