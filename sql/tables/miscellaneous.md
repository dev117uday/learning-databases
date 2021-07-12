# Miscellaneous

## Operators

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
SELECT * FROM person OFFSET 5 LIMIT 10;
SELECT * FROM person OFFSET 5 FETCH FIRST 5 ROW;
SELECT * FROM person WHERE gender IN ('Male');
SELECT * FROM person WHERE gender IN ('Male') AND dob > '2000-01-01' ORDER BY dob ASC;
SELECT * FROM person WHERE email LIKE '%gmail.com';
SELECT * FROM person WHERE email LIKE '%gmail.%;
```

```sql
SELECT * FROM person WHERE last_name LIKE '__i%';
SELECT * FROM person WHERE last_name ILIKE '__i%'; 
-- to ignore the case
```

