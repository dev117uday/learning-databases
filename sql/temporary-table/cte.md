# CTE

* `CTE` is a temporary result take from a SQL statement
* A second approach to create temporary tables for query data instead of using sub queries in a `FROM` clause 
* Good alternative to sub queries.
* `CTE` can be referenced multiple times in multiple places in query statement
* Lifetime of `CTE` is equal to the lifetime of a query.
* Types of `CTE`
  * Materialized
  * Not materialized

### Syntax

```sql
with cte_table ( column_list ) as (
    cte_query_definition
)

with num as (
    select *
    from generate_series(1, 10) as id
)
select *
from num;


with cte_director as (
    select *
    from movies
             inner join directors d on d.director_id = movies.director_id
)
select *
from cte_director;

--

WITH cte_film AS (
    SELECT movie_name,
           movie_length
                    title,
           (CASE
                WHEN movie_length < 100 THEN 'Short'
                WHEN movie_length < 120 THEN 'Medium'
                ELSE 'Long'
               END) length
    FROM movies
)
SELECT *
FROM cte_film
WHERE length = 'Long'
ORDER BY title;
```





