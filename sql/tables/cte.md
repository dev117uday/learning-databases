# Common Table Expression

* `CTE` is a temporary result take from a SQL statement
* A second approach to create temporary tables for query data instead of using sub queries in a `FROM` clause 
* Good alternative to sub queries.
* `CTE` can be referenced multiple times in multiple places in query statement
* Lifetime of `CTE` is equal to the lifetime of a query.
* Types of `CTE`
  * Materialized
  * Not materialized

## Syntax

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

WITH cte_movie_count AS
         (
             SELECT d.director_id,
                    SUM(COALESCE(r.revenues_domestic, 0) + COALESCE(r.revenues_international, 0)) AS total_revenues
             FROM directors d
                      INNER JOIN movies mv ON mv.director_id = d.director_id
                      INNER JOIN movies_revenues r ON r.movie_id = mv.movie_id
             GROUP BY d.director_id
         )
SELECT d.director_id,
       d.first_name,
       d.last_name,
       cte.total_revenues
FROM cte_movie_count cte
         INNER JOIN directors d ON d.director_id = cte.director_id;


create table articles (
    id  serial,
    article text
);

create table deleted_articles (
    id serial,
    article text
);

insert into articles (article)
values ('article 1'),
       ('article 2'),
       ('article 3'),
       ('article 4'),
       ('article 5');

select * from articles;
select *
from deleted_articles;

with cte_delete_article as (
    delete from articles
    where id = 1
    returning *
)
insert into deleted_articles select * from cte_delete_article;
```

