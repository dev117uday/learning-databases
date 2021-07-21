# Inner Join

## Basic

![](../../.gitbook/assets/image%20%283%29.png)

**All common column defined at ON must match values on both tables**

```sql
-- syntax

SELECT
 table_a.column1
 table_b.column2
FROM
 table_a
INNER JOIN table_b ON table1.column1 = table2.column2


SELECT
 mv.*,
 dir.*
FROM
 movies as mv
INNER JOIN directors as dir ON mv.director_id = dir.director_id


SELECT
 movie_name,
 dir.first_name || ' ' || dir.last_name as "Director Name",
 mv.movie_id,
 dir.director_id
FROM
 movies as mv
INNER JOIN directors as dir ON mv.director_id = dir.director_id

SELECT
 movie_name,
 mv.movie_lang,
 dir.first_name || ' ' || dir.last_name as "Director Name",
 mv.movie_id,
 dir.director_id
FROM
 movies as mv
INNER JOIN directors as dir ON mv.director_id = dir.director_id
WHERE mv.movie_lang = 'English'

SELECT
 movie_name,
 mv.movie_lang,
 dir.first_name || ' ' || dir.last_name as "Director Name",
 mv.movie_id,
 dir.director_id,
 dir.nationality
FROM
 movies as mv
INNER JOIN directors as dir ON mv.director_id = dir.director_id
WHERE dir.nationality = 'British'
```

## Inner Join with USING

* we use USING  only when joining tables have the SAME column name, rather then ON !

```sql
select
    table1.column1,
    table2.column1
from
    table1
INNER JOIN 
    table2 USING (column1);


select * from movies
    INNER JOIN directors USING (director_id);
```

