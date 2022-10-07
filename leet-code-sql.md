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


