# Full, Multiple & Self Joins

## Full Join

```sql
select * 
from right_product 
full join left_product 
on right_product.product_id = left_product.product_id;

select
    dir.first_name || ' ' || dir.last_name as "name",
    mv.movie_name
from directors dir full join movies mv on mv.director_id = dir.director_id
```

## Multiple Join

```sql
select
*
from movies mv
JOIN movies_revenues r on r.movie_id = mv.movie_id
JOIN directors dir on dir.director_id = mv.director_id

-- same result even after re-arranging
select
*
from movies mv
JOIN directors dir on dir.director_id = mv.director_id
JOIN movies_revenues r on r.movie_id = mv.movie_id
```

## Self Join

```sql
select * 
from left_product t1 
INNER JOIN left_product t2 
ON t1.product_id = t2.product_id;
```

