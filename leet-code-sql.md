    **Q1: What is the total amount each customer spent at the restaurant?**

```sql
  
Select distinct customer_id, sum(price) from sales as s
inner join menu as m on s.product_id = m.product_id
group by customer_id
```