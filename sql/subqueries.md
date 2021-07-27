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
```



