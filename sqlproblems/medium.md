

## SQL Medium Problems

#### [Problem 1](#1)
#### [Problem 2](#2)

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