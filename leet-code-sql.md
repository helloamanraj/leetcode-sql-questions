    **LeetCode Problem 177**

```sql
select salary from employee as a
      where N-1 = (select count(distinct salary) from employee as b
                  where b.salary > a.salary)
      limit 1
```  
    
    **LeetCode Problem 178**

```sql

select score, dense_rank() over (order by score desc) as "Rank" from Scores
```


    **LeetCode Problem 180**

```sql

select distinct(L1.num) as consecutiveNums from Logs as L1, 
Logs as L2,
Logs as L3

where L1.id = L2.id+1 and L1.id = L3.id+2 and L1.num = L2.num and L2.num = L3.num 

```
    **LeetCode Problem 184**

```sql

select department, employee, salary from (select d.name as department , e.name as employee , e.salary as salary , rank() over(partition by d.name order by e.salary desc) as rnk from employee as e
inner join Department as d on e.departmentId = d.id
) as x
where x.rnk = 1

``` 


    **LeetCode Problem 534**

```sql
select player_id, event_date, sum(games_played) over (partition by player_id order by event_date) 
as games_played_so_far 
from Activity
```


    **LeetCode Problem 550**

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


    **LeetCode Problem 570**

```sql

select e.name from employee as e 
inner join employee as m
on e.id = m.managerId
group by e.name
having count(*) >= 5
```


    **LeetCode Problem 574**

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

    **leetcode Problem 578**


```sql
select question_id as survey_log from survey_log
group by question_id
order by count(answer_id)/ count(if(action='show',1,0))desc
limit 1

```


    **leetcode Problem 580**

```sql
select dept_name, count(s.student_id) as student_number 
from student as s left join as department on s.dept_id = d.dept_id
group by dept_name
order by student_number desc, dept_name asc
```
-----------------------------------------------------------------------

    **Leetcode Probelm 585**

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



    **Leet code 602**

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

    


    **leetcode 608**

```sql

select distinct t1.id,
case when t1.p_id is null then 'Root'
when t2.P_id is null then 'Leaf'
else 'Inner' end as type
from tree as t1 left join tree as t2 on
t1.id = t2.p_id
order by t1.id

```


    **leetcode Problem 612**

```sql

select round(sqrt(min(pow(p1.x-p2.x,2) + pow(p1.y -p2.y,2))),2) as shortest
from point_2d as p1
inner join point_2d as p2
on p1.x != p2.x or p1.y != p2.y

```

    **leetcode Problem 614**

```sql
select a.follower, count(b.followee) as num from
follow as a
inner join follow as b
on a.follower = b.followee
group by a.follower

```


    **leetcode Problem 626**

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













