

## SQL Medium Problems

####<a name="1"></a>Problem 1

[Problem 1](#1)

Find the employee with the highest salary per department.
Output the department name, employee's first name along with the corresponding salary.

**employee** table
id: int
first_name: varchar
last_name: varchar
department: varchar
salary: int

Solution:

```sql
WITH t1 AS
  (SELECT first_name,
          department,
          salary,
          rank() over(PARTITION BY department
                      ORDER BY salary DESC) AS rank_sal
   FROM employee
   GROUP BY first_name,
            department,
            salary)
SELECT first_name,
       department,
       salary
FROM t1
WHERE rank_sal = 1
```