# OPERATORS

## Logical

```sql
SELECT 1=1;
SELECT 1<1;
SELECT 1>1;
SELECT 1<=1;
SELECT 1>=1;

-- not
SELECT 1<>1; 

-- or
SELECT 1 = 1 or 1 / 0 ;
```

```sql
SELECT * FROM person LIMIT 10;
SELECT * FROM person LIMIT 10 OFFSET 5;
SELECT * FROM person WHERE gender IN ('Male');
SELECT * FROM person WHERE gender IN ('Male') AND dob > '2000-01-01' ORDER BY dob ASC;
SELECT * FROM person WHERE email LIKE '%gmail.com';
SELECT * FROM person WHERE email LIKE '%gmail.%;
```

```sql
SELECT * FROM person WHERE last_name LIKE '__i%';

-- ILIKE
-- to ignore the case
SELECT * FROM person WHERE last_name ILIKE '__i%'; 

select 'hello' like 'hello';
select 'hello' like 'h%';
select 'hello' like '%e%';
select 'hello' like '%lo';
select 'hello' like '_ello';
select 'hello' like '__llo';
select 'hello' like '%ll_';

-- like with length of characters
select * from table where name like '____';
```

### Tips

* When using AND and OR in sample SQL statement, use brackets to differentiate between statements
* AND  operator is processed before OR operator
* SQL treats AND operator like multiplication and OR like divide

### Fetch

```sql
-- OFFSET start { ROW | ROWS }
-- FETCH { FIRST | NEXT } { ROW_COUNT } { ROWS|ROW } ONLY

SELECT
    *
FROM movies
fetch first row only ;


SELECT
    *
FROM movies
offset 3
fetch next 10 row only ;
```

## IS NULL or IS NOT NULL

```sql
select * from actors where date_of_birth is null;

select * from actors where date_of_birth is not null;
```

