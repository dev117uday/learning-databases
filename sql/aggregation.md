# Aggregation

* `COUNT (column)`
* `SUM (column)`
* `MIN & MAX`
* `LEAST & GREATEST`
* `AVG`

```sql
SELECT 
    COUNT(movie_length) 
from 
    movies;

SELECT 
    COUNT(movie_lang) 
from 
    movies;

SELECT 
    COUNT(DISTINCT(movie_lang)) 
from 
    movies;

SELECT 
    COUNT(movie_lang) 
FROM 
    movies 
where 
    movie_lang = 'English';

select SUM(revenues_domestic) 
    from movies_revenues;
select SUM(revenues_domestic) 
from movies_revenues 
    where revenues_domestic > 200;
SELECT 
    SUM(DISTINCT revenues_domestic) 
FROM 
    movies_revenues ;


SELECT min(movie_length), MAX(movie_length) 
FROM movies;

SELECT GREATEST(10,20,30,40), LEAST(10,20,30,40);

SELECT GREATEST('A','B','C','D'), LEAST('A','B','C','D');
SELECT GREATEST('A','B','C',1);

SELECT AVG(movie_length) FROM movies;
```

