
## SQL Hard Problems

### [Problem 1](#1) (Freemium vs. Premium)
### [Problem 2](#2) (Top 5 States With 5 Star Businesses)
### [Problem 3](#3) 

### <a name="1"></a>Problem 1

Find the total number of downloads for paying and non-paying users by date. Include only records where non-paying customers have more downloads than paying customers. The output should be sorted by earliest date first and contain 3 columns date, non-paying downloads, paying downloads.

**Tables**:

`ms_user_dimension`
 - user_id: int
 - acc_id: int

`ms_acc_dimension`
 - acc_id: int
 - paying_customer: varchar

`ms_download_facts`
 - date: datetime
 - user_id: int
 - downloads: int

**Solutions**:

Solution 1
```sql
with out AS(select date
, Sum (downloads) Filter(Where paying_customer = 'no') as non_paying
, Sum (downloads) Filter(Where paying_customer = 'yes') as paying
From  ms_download_facts fact
Left Join ms_user_dimension a
on fact.user_id = a.user_id
Join ms_acc_dimension acc
on a.acc_id = acc.acc_id
Group by date
order by date)
Select date , non_paying , paying
From out
Where non_paying > paying
```

Solution 2:
```sql
select 
date,
sum(case when paying_customer = 'yes' then downloads else 0 end ) as paying_downloads,
sum(case when paying_customer = 'no' then downloads else 0 end ) as non_paying_downloads
from
ms_user_dimension u
join 
ms_acc_dimension a
on u.acc_id = a.acc_id
join 
ms_download_facts d
on d.user_id = u.user_id
group by 1
having sum(case when paying_customer = 'no' then downloads else 0 end ) > sum(case when paying_customer = 'yes' then downloads else 0 end )
order by 1
```

Solution 3:
```sql
with t1 as (
    select date,paying_customer,downloads,u.user_id
    from ms_user_dimension u
    join ms_acc_dimension a on u.acc_id=a.acc_id
    join ms_download_facts d on u.user_id=d.user_id
),
paying as (
    select date, sum(downloads) paying
    from t1
    where paying_customer='yes'
    group by 1
),
non_paying as (
    select date, sum(downloads)  non_paying
    from t1
    where paying_customer='no'
    group by 1
)
select n.date,non_paying,paying from non_paying n
join paying p on p.date=n.date
where non_paying>paying
order by n.date
```

### <a name="2"></a>Problem 2
Find the top 5 states with the most 5 star businesses. Output the state name along with the number of 5-star businesses and order records by the number of 5-star businesses in descending order. In case there are ties in the number of businesses, return all the unique states. If two states have the same result, sort them in alphabetical order.

**Tables**:

`yelp_business`
 - business_id: varchar
 - name: varchar 
 - neighborhood: varchar
 - address: varchar
 - city: varchar
 - state: varchar
 - postal_code: varchar
 - latitude: float
 - longitude: float
 - stars: float
 - review_count: int
 - is_open: int
 - categories: varchar

```sql
SELECT state, total FROM (
    SELECT state, COUNT(stars) AS "total", RANK() OVER (ORDER BY COUNT(stars) DESC) AS "ranked"
    FROM yelp_business
    WHERE stars = 5
    GROUP BY state
    ORDER BY total DESC, state
) sub
WHERE sub.ranked <= 5;
```