

## SQL Medium Problems

#### [Problem 1](#1)
#### [Problem 2](#2)
#### [Problem 3](#3)




#### <a name="1"></a>Problem 1

Find the employee with the highest salary per department.
Output the department name, employee's first name along with the corresponding salary.

**employee** table
- id: int
- first_name: varchar
- last_name: varchar
- department: varchar
- salary: int

Solution:

```sql
WITH t1 AS
  (SELECT first_name,
          department,
          salary,
          rank() over(PARTITION BY department
                      ORDER BY salary DESC) AS rank_sal
   FROM employee)
SELECT first_name,
       department,
       salary
FROM t1
WHERE rank_sal = 1
```

####  <a name="2"></a>Problem 2

Find the top business categories based on the total number of reviews. Output the category along with the total number of reviews. Order by total reviews in descending order.

**yelp_business** table
- business_id: varchar
- name: varchar
- categories: varchar
- review_count: int

Solution:

```sql
SELECT UNNEST(STRING_TO_ARRAY(categories, ';')) AS category,
       SUM(review_count) AS total_reviews
FROM yelp_business
GROUP BY 1
ORDER BY 2 DESC
```

####  <a name="3"></a>Problem 3

Calculate each user's average session time. A session is defined as the time difference between a page_load and page_exit. For simplicity, assume a user has only 1 session per day and if there are multiple of the same events on that day, consider only the latest page_load and earliest page_exit. Output the user_id and their average session time.

**facebook_web_log** table
- user_id: int
- timestamp: datetime
- action: varchar

Solutions:
```sql
WITH EXIT AS
  (SELECT user_id ,
          date(timestamp) AS DAY ,
          min(timestamp) AS EXIT
   FROM facebook_web_log
   WHERE action = 'page_exit'
   GROUP BY 1,
            2) , LOAD AS
  (SELECT user_id ,
          date(timestamp) AS DAY ,
          max(timestamp) AS LOAD
   FROM facebook_web_log
   WHERE action = 'page_load'
   GROUP BY 1,
            2)
SELECT exit.user_id ,
       EXIT ,
       LOAD ,
       EXIT-LOAD
FROM EXIT
LEFT JOIN LOAD ON exit.user_id = load.user_id
AND exit.day = load.day
GROUP BY 1
```