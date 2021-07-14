# Miscellaneous

**Order of execution of SQL statements**

1. FROM
2. WHERE
3. SELECT
4. ORDER BY

## Concatenation Operator

```sql
select concat(first_name,last_name) as full_name 
    from actors limit 10;

select concat_ws(' ',first_name,last_name) as full_name 
    from actors limit 10;
```

* if you can have a null value in column, always use `concat_ws` because it will place nothing in that and and also not place the spacer like **\|** or a space

## Type Conversion

| Type of Conversion | Notes |
| :--- | :--- |
| Implicit | data conversion is done AUTOMATICALLY |
| Explicit  | data conversion is done via 'conversion functions' eg. `CAST` or `::` |

```sql
SELECT * FROM movies;

-- exact datatype match : no conversion
SELECT * FROM movies WHERE movie_id = 1;

-- Implicit conversion : conversion
SELECT * FROM movies WHERE movie_id = '1';

-- Explicit conversion : conversion
SELECT * FROM movies WHERE movie_id = integer '2';
```

## Casting

```sql
-- CAST function
-- Syntax : CAST ( expression as target_data_type );

SELECT CAST ( '10' AS INTEGER );

SELECT 
	CAST ('2020-02-02' AS DATE), 
	CAST('01-FEB-2001' AS DATE);

SELECT 
	CAST ( 'true' AS BOOLEAN ), 
	CAST ( '1' AS BOOLEAN ), 
	CAST ( '0' AS BOOLEAN );

SELECT CAST ( '14.87789' AS DOUBLE PRECISION );

SELECT '2020-02-02'::DATE , '01-FEB-2001'::DATE;

SELECT '2020-02-02 10:20:10.23'::TIMESTAMP;

SELECT '2020-02-02 10:20:10.23 +05:30'::TIMESTAMPTZ;

SELECT 
	'10 minute'::interval, 
	'10 hour'::interval, 
	'10 day'::interval, 
	'10 week'::interval, 
	'10 month'::interval;
	
SELECT 20! AS "result 1" , CAST( 20 AS bigint ) ! AS "result 2";

SELECT 
	ROUND(10,4) AS "result 1", 
	ROUND ( CAST (10 AS NUMERIC) ) AS "result 2", 
	ROUND ( CAST (10 AS NUMERIC) , 4 ) AS "result 3";
	
SELECT 
	SUBSTR('12345',2) AS "RESULT 1", 
	SUBSTR( CAST('12345' AS TEXT) ,2) AS "RESULT 2";
	
	
	
CREATE TABLE ratings (
	rating_id SERIAL PRIMARY KEY,
	rating VARCHAR(2) NOT NULL
);

SELECT * FROM ratings;

INSERT INTO ratings ( rating ) 
VALUES ('A'), ('B'), ('C'), ('D'), (1), (2), (3), (4);

SELECT 
	rating_id,
	CASE 
		WHEN rating~E'^\\d+$' THEN
			CAST ( rating as INTEGER )
		ELSE
			0
		END AS rating
FROM
	ratings;
```

## Formatting Functions

[https://www.postgresql.org/docs/12/functions-formatting.html](https://www.postgresql.org/docs/12/functions-formatting.html)

### to\_char\(\)

Refer to the documentation 

{% embed url="https://www.postgresqltutorial.com/postgresql-to\_char/" %}

```sql
SELECT TO_CHAR (
	100870,
	'9,999999'
);	

SELECT 
	release_date, 
	TO_CHAR(release_date,'DD-MM-YYYY'),
	TO_CHAR(release_date,'Dy, MM, YYYY') 
FROM 
	movies;
	
SELECT 
	TO_CHAR ( 
		TIMESTAMP '2020-01-01 13:32:30', 
		'HH24:MI:SS' 
	);
	
SELECT 
	movie_id, 
	TO_CHAR ( revenues_domestic, '$99999D99' ) 
FROM 
	movies_revenues;
```

### to\_number\(\)

{% embed url="https://www.postgresqltutorial.com/postgresql-to\_number/" %}

```sql
SELECT TO_NUMBER(
    '1420.89', '9999.'
);

SELECT TO_NUMBER(
    '10,625.78-', '99G999D99S'
);

SELECT TO_NUMBER(
    '$1,625.78+', '99G999D99S'
);

SELECT to_number(
    '$1,420.65' , 'L9G999D99'
);

SELECT to_number(
    '21,420.65' , '99G999D99'
);
```

### to\_date\(\)

{% embed url="https://www.postgresqltutorial.com/postgresql-to\_date/" %}

```sql
SELECT TO_DATE( '2020/10/22' , 'YYYY/MM/DD' );

SELECT to_date( '022199' , 'MMDDYY' );

SELECT to_date( 'March 07, 2019' , 'Month DD, YYYY' );
```

### to\_timestamp\(\)

{% embed url="https://www.postgresqltutorial.com/postgresql-to\_timestamp/" %}

```sql
SELECT TO_TIMESTAMP(
    '2017-03-31 9:30:20',
    'YYYY-MM-DD HH:MI:SS'
);

SELECT
    TO_TIMESTAMP('2017     Aug','YYYY MON');
```



