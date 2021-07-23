# Arrays

## Sample

```sql
create table table_array (
    id SERIAL,
    name varchar(100),
    phones text[]
);

insert into table_array (name, phones) 
    values ('uday',array ['999999999','000000000']);
insert into table_array (name, phones) 
    values ('uday1',array ['9999999990','0000000009']);

select name, phones[1] from table_array;
```

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

## Array in Tables

### Insert

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

### Query

```sql
select class[1] from teachers;

select * from teachers where class[1] = 'english';

select * from teachers where  'english' = any (class);
```

### Update

```sql
update teachers
set class[1] = 'dutch'
where id = 1;

select * from teachers;

update teachers
set class[3] = 'science'
where id = 1;

select * from teachers;
```

### Dimensionless

```sql
create table teacher2 (
    id serial primary key ,
    class text array[1]
);

insert into teacher2 (class) values (array ['english']);

select * from teacher2;

-- dimensions doesnt matter
insert into teacher2 (class) values (array ['english','hindi']);

```

### Unnest

```sql
select id, class, unnest(class) from teacher2;
```

### Multi Dimensional Array

```sql
create table students (
    id serial primary key ,
    name varchar(50) not null ,
    grade integer[][]
);

insert into students (name, grade) 
    values ('s1','{90,2020}'),('s1','{70,2020}'),('s1','{60,2020}');

select * from students;

select * from students where grade @> '{90}';

select * from students where '2020' = any (grade);

select  * from students where grade[1] < 80;

```

## Array vs JSONB

#### Advantages to Array

* It's pretty easy to setup
* Requires less storage than jsonb
* It has multi dimensional support 
* Indexing through GIN, greatly speeds up query
* The PostgreSQL planner is likely to make better decisions with PostgreSQL array, as it collects statistics on its content, but not with JSONB.

#### Disadvantages to Array

* Its main advantages is that you are limited to one data type
* Have to follow strict order of the array data input.

#### Advantages to JSONB

* Provides additional operators for querying
* Support for indexing

#### Disadvantages to JSONB

* Has to parse the json data to binary format
* slow in writing, but faster in reading
* Doesn't maintain order



