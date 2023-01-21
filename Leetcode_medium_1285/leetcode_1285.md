   Create Table 

```sql
create table Logs
(log_id int(5))

insert into Logs
values

( 1 ),
( 2 ),
( 3 ), 
( 7 ),         
( 8 ),         
( 10)      
```

  Solution 

```sql
with cte as (select log_id, log_id - row_number() over ( order by log_id ) as diff
from Logs)

select min(log_id) as start_id , max(log_id), diff as end_id 
from cte 
group by diff
```