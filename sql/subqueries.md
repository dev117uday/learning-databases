# SubQueries

* Allows you to construct  a complex query
* A sub-query is nested inside another query
* can be nested inside `SELECT`, `INSERT`, `UPDATE`, `DELETE`

```sql
select movie_name,
       movie_length
from movies mv
where movie_length >= (
    select avg(movie_length)
    from movies
)

select movie_name,
       movie_length
from movies mv
where movie_length >= (
    select avg(movie_length)
    from movies
    where movie_lang = 'English'
);

select first_name, last_name, date_of_birth
from actors
where date_of_birth > (
    select date_of_birth
    from actors
    where first_name = 'Douglas' -- 1922-06-10
);

-- using IN operator
select movie_name,
       movie_length
from movies
where movie_id in (
    select movie_id
    from movies_revenues
    where revenues_domestic > 200
);

select movie_id, movie_name
from movies
where movie_id IN (
    select movie_id
    from movies_revenues
    where revenues_domestic > movies_revenues.revenues_international
);


-- with joins
select d.director_id,
       d.first_name || ' ' || d.last_name  as "Director Name",
       SUM(r.revenues_international + r.revenues_domestic) as "total_revenues"
from directors d
         INNER JOIN movies mv ON mv.director_id = d.director_id
         INNER JOIN movies_revenues r on r.movie_id = mv.movie_id
where (r.revenues_domestic + r.revenues_international) >
      (
          select avg(revenues_domestic + revenues_international) as "avg_total_revenue"
          from movies_revenues
      )
group by d.director_id
order by total_revenues;


SELECT d.director_id,
       SUM(COALESCE(r.revenues_domestic, 0) + COALESCE(r.revenues_international, 0)) AS "totaL_reveneues"
FROM directors d
         INNER JOIN movies mv ON mv.director_id = d.director_id
         INNER JOIN movies_revenues r ON r.movie_id = mv.movie_id
WHERE COALESCE(r.revenues_domestic, 0) + COALESCE(r.revenues_international, 0) >
      (
          SELECT AVG(COALESCE(r.revenues_domestic, 0) + COALESCE(r.revenues_international, 0)) as "avg_total_reveneues"
          FROM movies_revenues r
                   INNER JOIN movies mv ON mv.movie_id = r.movie_id
          WHERE mv.movie_lang = 'English'
      )
GROUP BY d.director_id
ORDER BY 2 DESC, 1 ASC;

-- as alias
SELECT *
from (
         select *
         from movies
     ) t1;
     
-- query without from
select (
           select avg(revenues_domestic) as "Average Revenue"
           from movies_revenues
       ),
       (
           select min(revenues_domestic) as "MIN Revenue"
           from movies_revenues
       ),
       (
           select max(revenues_domestic) as "MAX Revenue"
           from movies_revenues
       )
```



