    LeetCode Problem 177

```sql
select salary from employee as a
      where N-1 = (select count(distinct salary) from employee as b
                  where b.salary > a.salary)
      limit 1
```  
    
    LeetCode Problem 178

```sql

select score, dense_rank() over (order by score desc) as "Rank" from Scores
```


    LeetCode Problem 180

```sql

select distinct(L1.num) as consecutiveNums from Logs as L1, 
Logs as L2,
Logs as L3

where L1.id = L2.id+1 and L1.id = L3.id+2 and L1.num = L2.num and L2.num = L3.num 

```
    LeetCode Problem 184

```sql

select department, employee, salary from (select d.name as department , e.name as employee , e.salary as salary , rank() over(partition by d.name order by e.salary desc) as rnk from employee as e
inner join Department as d on e.departmentId = d.id
) as x
where x.rnk = 1

``` 


    LeetCode Problem 534

```sql
select player_id, event_date, sum(games_played) over (partition by player_id order by event_date) 
as games_played_so_far 
from Activity
```


    LeetCode Problem 550

```sql
with cte as (
select player_id, min(event_date) as first_login
from activity 
group by player_id

)


select round(sum(case when datediff(event_date- first_login)=1 then 1 else
0 end) /
count(distinct cte.player_id),2) as fraction
from activity as a 
inner join cte as c
on a.player_id = c.player_id
```


    LeetCode Problem 570

```sql

select e.name from employee as e 
inner join employee as m
on e.id = m.managerId
group by e.name
having count(*) >= 5
```


    LeetCode Problem 574

```sql
with cte as (
select candidateid, count(*) as number_of_votes
from vote
group by
candidateid
order by number_of_votes desc
limit 1)


select name from candidate as cand
inner join cte as c on cand.id = c.candidateId

```

    leetcode Problem 578


```sql
select question_id as survey_log from survey_log
group by question_id
order by count(answer_id)/ count(if(action='show',1,0))desc
limit 1

```


    leetcode Problem 580

```sql
select dept_name, count(s.student_id) as student_number 
from student as s left join as department on s.dept_id = d.dept_id
group by dept_name
order by student_number desc, dept_name asc
```


    Leetcode Probelm 585

```sql

select cast(sum(TIV_2016) as decimal(10,2)) as TIV_2016 from (
select TIV_2016
, count(*) over (partition by tiv_2015) as 2015_count,
count(*) over (partition by LAT, LON) as count_loca

from insurance
) x
where x.2015_count > 1 and x.count_loca = 1

 
# with subquery 


select cast(sum(TIV_2016) as decimal(10,2)) 
from insurance 

where tiv_2015 in (
select tiv_2015 from insurance 
group by tiv_2015 
having count(*)>1
) and 
concat(lat, _ ,lon) in (
select concat(lat,_,lon) from insurance
group by lat,lon 
having count(*) = 1
)


```



    Leetcode problem 602

```sql

select id, count(*) as num from
(select requester_id as ids
from request_accepted

union all

select acceptor_id from 
request_accepted)
as friend_count
group by id 
order by num as desc
limit 1

```

    


    leetcode problem 608

```sql

select distinct t1.id,
case when t1.p_id is null then 'Root'
when t2.P_id is null then 'Leaf'
else 'Inner' end as type
from tree as t1 left join tree as t2 on
t1.id = t2.p_id
order by t1.id

```


    leetcode Problem 612

```sql

select round(sqrt(min(pow(p1.x-p2.x,2) + pow(p1.y -p2.y,2))),2) as shortest
from point_2d as p1
inner join point_2d as p2
on p1.x != p2.x or p1.y != p2.y

```

    leetcode Problem 614

```sql
select a.follower, count(b.followee) as num from
follow as a
inner join follow as b
on a.follower = b.followee
group by a.follower

```


    leetcode Problem 626

```sql

select id, 
case when id % 2 = 0 then lag(student,1) over(order by id)
when id % 2 != 0 then lead(student,1,student) over(order by id)
end as student 
from seat


```

    leetcode problem 1045

```sql

select customer_id 
from Customer
group by customer_id
having count(distinct product_key) = (select count(distinct product_key) from Product)

```

    leetcode problem 1070

```sql
select product_id, min(years) as first_year, quantity
,price
from sales 
group by product_id

```


    leetcode 1077

```sql

with cte as(
select p.project_id, p.employee_id, dense_rank() over(partition by p.project_id order by e.experience_years desc) as experience_yr_rnk
from Project p join Employee e on p.employee_id = e.employee_id)

select project_id, employee_id from tmp 
where experience_yr_rnk = 1;

``` 

    leetcode 1098

```sql
WITH eligible_books AS (
  SELECT
    *
  FROM
    Books B
  WHERE B.available_from < DATE_SUB('2019-06-23', INTERVAL 1 MONTH)
), eligible_orders AS (
  SELECT
    *
  FROM
    Orders O
  WHERE O.dispatch_date > DATE_SUB('2019-06-23', INTERVAL 1 YEAR)
)
SELECT
  EB.book_id, EB.name
FROM
  eligible_books EB
  LEFT JOIN eligible_orders EO ON EB.book_id = EO.book_id
GROUP BY
  EB.book_id, EB.name
HAVING
  SUM(EO.quantity) IS NULL OR SUM(EO.quantity) < 10
```

  leetcode 1107

```sql


with login_date as
(select user_id, min(activity_date) as min_date from traffic
where activity = 'login'
group by user_id)

select min_date as login_date, count(user_id) as user_count from login_date
where datediff('2019-06-30', min_date) <= 90
group by min_date


```

leetcode 1112

```sql 
with temp as 
( select * , max(grade) over (partition by student_id ) as max_grade
from enrollments)


select student_id, min(course_id) as course_id , max(grade) as grade from temp
where grade = max_grade
group by student_id 
order by student_id


```
leetcode 1126

```sql

select buss_id from (select *, avg(occurances) over(partition by event_type) as average
from Events)
as x
where occurances > average
having count(event_type) > 1 

```


leetcode 1149

```sql


select viewer_id as id  from (select distinct(viewer_id), count(distinct article_id) from views 
group by view_date, viewer_id
having count(distinct article_id) > 1
) x


```

leetcode 1158

```sql

with 2019_orders as
(
select * , count(*) orders_in_2019 from orders
where YEAR(order_date) = 2019
group by buyer_id
)



select user_id as buyer_id, join_date, ifnull((orders_in_2019),0)
from users as u
left join 2019_orders as c
on u.user_id = c.buyer_id
```

leetcode 1164

```sql




with cte as  (select *, rank() over(partition by product_id order by change_date desc) as rnk
FROM products
where change_date <= '2019-08-16' 
)


select product_id, new_price as price
from cte
where rnk=1

union 

select product_id, 10 as price
from products 
where product_id not in (select product_id from cte)


```

leetcode 1174

```sql


with min_date as (
select customer_id, min(order_date) as first_ord_date, 
(case when min(order_date) = pref_del_date then "immediate" else "schedule" end ) as del_status 

from delivery
group by customer_id
)


select 
(round(100*sum( case when del_status = 'immediate' then 1 else 0 end) /
(select count(*) from min_date),2)) as immediate_percentage 
from min_date 

```

leetcode 1193

```sql



select trans_date as month , country, count(*) as trans_count ,
sum(case when state = 'approved' then 1 else 0 end)  as approved_count , sum(amount) as amount
from 
transactions
group by month(trans_date), country

```

leetcode 1204

```sql 


select person_name from (select person_name, weight, sum(weight) over (order by turn) as cul_sum 
from queue)x
where cul_sum <= 1000
order by cul_sum desc
limit 1

```

leetcode 1205

```sql 

with cte as (select  distinct(c.trans_id), t.country, 'chargeback' as state, t.amount, c.trans_date
    from Chargebacks as c join Transactions t on c.trans_id = t.id
    union all
    select *
    from Transactions
)

cte2 as (select  date_format(trans_date, '%Y-%m') as month, country, 
        sum(case when state = 'approved' then 1 else 0 end) as approved_count,
        sum(case when state = 'approved' then amount else 0 end) as approved_amount,
        sum(case when state = 'chargeback' then 1 else 0 end) as chargeback_count,
        sum(case when state = 'chargeback' then amount else 0 end) as chargeback_amount
from cte 
group by country, month
having approved_amount > 0 or chargeback_amount > 0

```