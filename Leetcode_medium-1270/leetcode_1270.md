  Create Table 
```sql
create table Employees
(employee_id int(5), employee_name varchar(10), manager_id int(5))

insert into Employees
(employee_id, employee_name, manager_id)
values 

( 1 , 'Boss'  ,1),          
( 3 , 'Alice' , 3),          
( 2 , 'Bob' ,1),          
( 4 , 'Daniel',2),          
( 7 , 'Luis', 4),          
( 8 , 'Jhon' , 3),          
( 9  , 'Angela'  , 8),          
( 77 ,  'Robert', 1)          
```

  Solution 

```sql


select a.employee_id
from employees a,
 employees b,
 employees c
where a.manager_id = b.employee_id and 
    b.manager_id = c.employee_id and 
    c.manager_id = 1 and 
    a.employee_id != c.manager_id
```