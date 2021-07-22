# Combining Tables

## UNION

* Combines result sets from two or more SELECT statements into a single result set
* The order and number of the columns in the select list of all queries must be the same

```sql
select * from left_product
union
select * from right_product
order by product_id;

select * from left_product
union all
select * from right_product
order by product_id;
```

## Intersect

```sql
select * from left_product
intersect 
select * from right_product
order by product_id;
```

## Except

```sql
select * from left_product
except 
select * from right_product
order by product_id;
```

