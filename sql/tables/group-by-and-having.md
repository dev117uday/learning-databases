# GROUP BY and HAVING

## ORDER OF EXECUTION

```sql
FROM
WHERE
GROUP BY
HAVING
SELECT
DISTINCT
ORDER BY
LIMIT
```

## GROUP BY

* `GROUP BY` clause divide the rows returned from `SELECT` statement into groups
* For each group, you can apply aggregate functions like `COUNT`, `SUM`, `MIN`, `MAX` etc.

```sql
-- syntax

-- SELECT 
--     column1, 
--     AGGREGATE_FUNCTION(column2) 
-- FROM 
--     tablename 
-- GROUP BY 
--     column1;

SELECT 
    movie_lang,
    COUNT(movie_lang)
FROM
    movies
GROUP BY
    movie_lang
ORDER BY
    movie_lang;


SELECT 
    age_certificate,
    SUM(movie_length) 
FROM 
    movies 
GROUP BY 
    age_certificate;


SELECT 
    movie_lang,
    MIN(movie_length),
    MAX(movie_length)
FROM
    movies
GROUP BY
    movie_lang



SELECT 
    movie_lang,
    MIN(movie_length),
    MAX(movie_length)
FROM
    movies
GROUP BY
    movie_lang
ORDER BY MAX(movie_length) DESC
```

## HAVING

* We use `HAVING` clause to specify a search condition for a group or an aggregate
* The `HAVING` clause is often used with the `GROUP BY` clause to filter rows based on filter condition
* cannot use column alias with having clause because it is evaluated before the `SELECT` statement

```sql
SELECT 
    column1,
    AGGREGATE_FUNCTION(column2),
FROM tablename
GROUP BY column1
HAVING 
    condition;
```

* `HAVING AGGREGATE_FUNCTION(column2) = value`
* `HAVING AGGREGATE_FUNCTION(column2) >= value`

```sql
SELECT 
    movie_lang, 
    SUM(movie_length) 
FROM 
    movies 
GROUP BY 
    movie_lang 
HAVING SUM(movie_length) > 200 
ORDER BY SUM(movie_length);
```

### HAVING vs WHERE

* `HAVING` works on result group
* **`WHERE` works on `SELECT` columns and not on the result group**

```sql
SELECT
    movie_lang,
    SUM(movie_length)
FROM 
    movies
GROUP BY movie_lang
ORDER BY 2 DESC;
```

