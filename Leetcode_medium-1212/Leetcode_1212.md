


    Create table 

```sql


create table teams 
(team_id int(5), team_name varchar(20))

insert into teams 
(team_id, team_name)

values 
( 10 , 'Leetcode FC'),
( 20 , 'NewYork FC'),
( 30 , 'Atlanta FC'),
( 40 , 'Chicago FC'), 
( 50 , 'Toronto FC')




create table Matches
( match_id int(5) ,  host_team int(5) , guest_team int(5) , host_goals int(4) , guest_goals int(2)) 

insert into Matches 
( match_id, host_team , guest_team, host_goals, guest_goals)
values
( 1 , 10 , 20 , 3, 0),
(2 ,30 , 10 , 2 , 2),            
 (3  ,10,  50 , 5, 1),             
( 4 , 20 , 30 , 1, 0),            
( 5, 50, 30, 1, 0)  

```
     SQL solution

```sql

select t.team_id, t.team_name, 
sum(case when t.team_id = m.host_team and m.host_goals > m.guest_goals then 3
when t.team_id = m.host_team and m.host_goals = m.guest_goals then 1 
when t.team_id = m.guest_team and m.host_goals < m.guest_goals then 3 
when t.team_id = m.guest_team and m.host_goals < m.guest_goals then 1
else 0 end) as num_points
from matches as m
right join
teams as t 
on 
m.host_team = t.team_id or m.guest_team = t.team_id
group by team_id, team_name
order by num_points desc, team_id

```