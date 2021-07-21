# Left and Right JOIN

## Left Join

![](../../.gitbook/assets/image%20%284%29.png)

### Basic

* Returns every row from **LEFT** table plus rows that match values in the joined column from the **RIGHT** table

```sql
SELECT 
    table1.column1,
    table2.column1
FROM 
    table1
LEFT JOIN 
    table2 
ON
    table1.column1 = table2.column2
    
create table left_product (
    product_id INT PRIMARY KEY ,
    product_name VARCHAR(100)
);

CREATE TABLE right_product (
    product_id INT PRIMARY KEY ,
    product_name VARCHAR(100)
);

INSERT INTO left_product ( PRODUCT_ID, PRODUCT_NAME )
    VALUES (1,'a'),('2','B'),('3','C'),('5','E');

INSERT INTO right_product ( PRODUCT_ID, PRODUCT_NAME )
    VALUES (1,'a'),('2','B'),('3','C'),('4','d'),(7,'E1');

SELECT
    *
from left_product
left join
    right_product 
    on left_product.product_id = right_product.product_id;
    
select
    dir.first_name,
    dir.last_name,
    mv.movie_name
from
     directors dir
left join movies mv
    ON mv.director_id = dir.director_id;


select
    dir.first_name || ' ' || dir.last_name as "Directors Name",
    mv.movie_name,
    mv.movie_lang
from
     directors dir
left join movies mv
    ON mv.director_id = dir.director_id
where mv.movie_lang in ('English','Chinese');
```

## Right Join



