# Cross & Natural Joins

## Cross Join

```sql
select * from left_product cross join  right_product;

select * from left_product , right_product;

select * from left_product inner join right_product on true;

```

## Natural Join

```sql
select *
from left_product
         NATURAL INNER JOIN right_product;

select *
from left_product
         NATURAL left JOIN right_product;


select *
from left_product
         NATURAL right join right_product;de
```







