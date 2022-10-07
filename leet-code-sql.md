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
~