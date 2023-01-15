  create table 

  ```sql


  create table friendship
(user1_id int(5) , user2_id int(5))

insert into friendship  
(user1_id, user2_id)

values

(1 , 2),       
(1 , 3),
( 1 ,4),
( 2 ,3 ),      
( 2 , 4 ),
( 2 , 5 ),
( 6  , 1 )


create table Likes
(user_id int(5), page_id int(5))

insert into Likes 
(user_id, page_id)
values
( 1 , 88 ),
( 2 , 23 ),
( 3 , 24 ),
( 4 , 56 ),  
( 5 , 11 ),    
( 6 , 33 ),
( 2 , 77 ),
( 3 , 77 ),   
( 6 , 88 ) 


```

    Solution 

```sql 


select distinct page_id as recommended_page
from likes as L
left join friendship as f on L.user_id = f.user2_id
where f.user1_id = 1 and page_id not in (
select page_id from Likes where user_id = 1)
  
 union 
 select distinct page_id as recommended_page
 from Likes as L 
 left join friendship as f on L.user_id = f.user1_id
 where f.user2_id = 1 and page_id not in (
 select page_id  from Likes where user_id = 1)
 

 ```
