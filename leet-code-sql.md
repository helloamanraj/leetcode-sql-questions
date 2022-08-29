    **LeetCode Problem 177**

```sql
select salary from employee as a
      where N-1 = (select count(distinct salary) from employee as b
                  where b.salary > a.salary)
      limit 1
```  
    
    ** LeetCode Problem 178**

```sql

select score, dense_rank() over (order by score desc) as "Rank" from Scores
```


