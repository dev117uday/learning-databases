# Arrays

## Ranges

![range in array](../../.gitbook/assets/image%20%286%29.png)

```sql
SELECT 
	INT4RANGE(1,6)													 	
		AS "DEFAULT [(",
	NUMRANGE(1.432,6.238,'[]') 										 	
		AS "[]",
	DATERANGE('20200101','20201222','()') 							 	
		AS "DATES ()",
	TSRANGE(LOCALTIMESTAMP,LOCALTIMESTAMP + INTERVAL '8 DAYS', '(]') 	
		AS "OPENED CLOSED";	
		
		
select
 ARRAY [1,2,3] AS "INT ARRAYS",
 ARRAY [2.123::FLOAT]  AS "FLOATING NUMBERS",
 ARRAY [CURRENT_DATE, CURRENT_DATE + 5]		
 
 
SELECT 
	ARRAY [1,2,3,4] = ARRAY[1,2,3,4],
	ARRAY [1,2,3,4] = ARRAY[1,1,3,4],
	ARRAY [1,2,3,4] <> ARRAY[1,2,3,4],
	ARRAY [1,2,3,4] < ARRAY[1,5,3,4],
	ARRAY [1,2,3,4] <= ARRAY[1,3,3,4],
	ARRAY [1,2,3,4] > ARRAY[1,2,3,4];

```

## Inclusion Operators

```sql
SELECT 
	ARRAY[1,2,3,4] @> ARRAY[2,3,4] AS "CONTAINS",
	ARRAY['A','B'] <@ ARRAY['A','B','C'] AS "CONTAINED BY",
	ARRAY[1,2,3,4] && ARRAY[2,3,4] AS "IS OVERLAP";
```

## Length and Dimensions

```sql
SELECT 
       ARRAY [1,2,3] || ARRAY [4,5,6] AS "COMBINED ARRAY";

SELECT 
       ARRAY_CAT(ARRAY [1,2,3], 
       ARRAY [4,5,6]) AS "COMBINED ARRAY VIA CAT";

SELECT 
       4 || ARRAY [1,2,3] AS "ADDING TO ARRAY";

SELECT 
       ARRAY [1,2,3] || 4 AS "ADDING TO ARRAY";

SELECT 
       ARRAY_APPEND(ARRAY [1,2,3], 4) AS "USING APPEND";

SELECT 
       ARRAY_PREPEND(4, ARRAY [1,2,3]) AS "USING APPEND";

SELECT 
       ARRAY_NDIMS(ARRAY [[1,2,3,4],[1,2,3,4],[1,2,3,4]]) AS "DIMENSIONS",
       ARRAY_DIMS(ARRAY [1,2,3,4,2,3,4])                  AS "DIMENSIONS";


SELECT ARRAY_LENGTH(ARRAY [-111,2,3,4], 1);

SELECT 
       ARRAY_UPPER(ARRAY [1,2,3,4000], 1), 
       ARRAY_LOWER(ARRAY [-100,2,3,4], 1);
```

## Positions

```sql
select array_position(array ['jan','feb','mar'], 'feb');

select array_position(array [1,2,2,3,4], 2, 3);

select array_positions(array [1,2,2,3,4], 2);
```

## Search, Replace, Remove

```sql
select array_cat(array [1,2], array [3,4]);

select array_append(array [1,2,3], 4);

select array_remove(array [1,2,3,4,4,4], 4);

select array_replace(array [1,2,3,4,4,4], 4, 5);
```

## IN, NOT IN, ANY

```sql
select 20 in (1, 2, 3, 20) as "result";

select 25 in (1, 2, 3, 20) as "result";

select 25 not in (1, 2, 3, 20) as "result";

select 20 = all (Array [20,22]), 20 = all (array [20,20]);

select 20 = any (Array [1,2,25]) as "result"
```

## STRING &lt;&gt; ARRAY

```sql
SELECT string_to_array('1,2,3,4,5', ',');

SELECT string_to_array('1,2,3,4,5,ABC', ',', 'ABC');

SELECT string_to_array('1,2,3,4,,6', ',', '');

SELECT array_to_string(ARRAY [1,2,3,4], '|');

SELECT array_to_string(ARRAY [1,2,3,4,NULL], '|', 'EMPTY');
```

## Inserting data into array

* for non text data , use `{value1,value2}` or `array['value1','value2']`
* for text data , use `{"value1","value2"}` or `array[value1,value2]`

```sql
CREATE TABLE teachers (
    id serial primary key ,
    class text []
);

create table teacher1 (
    id serial primary key ,
    class text array
);

insert into teachers (class) values (array ['english','maths']);

select * from teachers;
```

