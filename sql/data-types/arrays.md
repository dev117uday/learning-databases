# Arrays

## Arrays

* Original Documentation : [here](https://www.postgresql.org/docs/current/arrays.html)

### Syntax

```sql
column_name DATATYPE[] {CONSTRAINT}
```

```sql
CREATE TABLE table_array
(
    id     SERIAL,
    name   varchar(100),
    grades text[]
);

INSERT INTO table_array (name, grades)
VALUES ('person 1', array ['100','45']);
INSERT INTO table_array (name, grades)
VALUES ('person 2', array ['100','90']);
INSERT INTO table_array (name, grades)
VALUES ('person 3', array ['100','97']);
INSERT INTO table_array (name, grades)
VALUES ('person 4', array ['100','94']);


SELECT name, grades[1]
FROM table_array;
```

## Array in Tables

### Insert

* for non text data , use `{value1,value2}` or `array ['value1','value2']`
* for text data , use `{"value1","value2"}` or `array [value1,value2]`

```sql
CREATE TABLE teachers
(
    id    serial primary key,
    class text[]
);

CREATE TABLE teacher1
(
    id    serial primary key,
    class text array
);

INSERT INTO teachers (class)
VALUES (array ['english','maths']);

SELECT *
FROM teachers;
```

### Query

```sql
SELECT class[1]
FROM teachers;

SELECT *
FROM teachers
WHERE class[1] = 'english';

SELECT *
FROM teachers
WHERE 'english' = any (class);
```

### Update

```sql
update teachers
set class[1] = 'dutch'
WHERE id = 1;

SELECT *
FROM teachers;

update teachers
set class[3] = 'science'
WHERE id = 1;

SELECT *
FROM teachers;
```

### Dimensionless

```sql
CREATE TABLE teacher2
(
    id    serial primary key,
    class text array[1]
);

INSERT INTO teacher2 (class)
VALUES (array ['english']);

SELECT *
FROM teacher2;

-- dimensions doesnt matter
INSERT INTO teacher2 (class)
VALUES (array ['english','hindi']);
```

### Unnest

```sql
SELECT id, class, unnest(class)
FROM teacher2;
```

### Multi Dimensional Array

```sql
CREATE TABLE students
(
    id    serial primary key,
    name  varchar(50) not null,
    grade integer[][]
);

INSERT INTO students (name, grade)
VALUES ('s1', '{90,2020}'),
       ('s1', '{70,2020}'),
       ('s1', '{60,2020}');

SELECT *
FROM students;

SELECT *
FROM students
WHERE grade @> '{90}';

SELECT *
FROM students
WHERE '2020' = any (grade);

SELECT *
FROM students
WHERE grade[1] < 80;
```

### Array vs JSONB

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

## Ranges

![range in array](../../.gitbook/assets/image%20%286%29.png)

```sql
SELECT INT4RANGE(1, 6)                                                   AS "DEFAULT [(",
       NUMRANGE(1.432, 6.238, '[]')                                      AS "[]",
       DATERANGE('20200101', '20201222', '()')                           AS "DATES ()",
       TSRANGE(LOCALTIMESTAMP, LOCALTIMESTAMP + INTERVAL '8 DAYS', '(]') AS "OPENED CLOSED";


SELECT ARRAY [1,2,3]        AS "INT ARRAYS",
       ARRAY [2.123::FLOAT] AS "FLOATING NUMBERS",
       ARRAY [CURRENT_DATE, CURRENT_DATE + 5]


SELECT ARRAY [1,2,3,4] = ARRAY [1,2,3,4],
       ARRAY [1,2,3,4] = ARRAY [1,1,3,4],
       ARRAY [1,2,3,4] <> ARRAY [1,2,3,4],
       ARRAY [1,2,3,4] < ARRAY [1,5,3,4],
       ARRAY [1,2,3,4] <= ARRAY [1,3,3,4],
       ARRAY [1,2,3,4] > ARRAY [1,2,3,4];
```

### Inclusion Operators

```sql
SELECT ARRAY [1,2,3,4] @> ARRAY [2,3,4]       AS "CONTAINS",
       ARRAY ['A','B'] <@ ARRAY ['A','B','C'] AS "CONTAINED BY",
       ARRAY [1,2,3,4] && ARRAY [2,3,4]       AS "IS OVERLAP";
```

### Length and Dimensions

```sql
SELECT ARRAY [1,2,3] || ARRAY [4,5,6] AS "COMBINED ARRAY";

SELECT ARRAY_CAT(ARRAY [1,2,3],
                 ARRAY [4,5,6]) AS "COMBINED ARRAY VIA CAT";

SELECT 4 || ARRAY [1,2,3] AS "ADDING TO ARRAY";

SELECT ARRAY [1,2,3] || 4 AS "ADDING TO ARRAY";

SELECT ARRAY_APPEND(ARRAY [1,2,3], 4) AS "USING APPEND";

SELECT ARRAY_PREPEND(4, ARRAY [1,2,3]) AS "USING APPEND";

SELECT ARRAY_NDIMS(ARRAY [[1,2,3,4],[1,2,3,4],[1,2,3,4]]) AS "DIMENSIONS",
       ARRAY_DIMS(ARRAY [1,2,3,4,2,3,4])                  AS "DIMENSIONS";


SELECT ARRAY_LENGTH(ARRAY [-111,2,3,4], 1);

SELECT ARRAY_UPPER(ARRAY [1,2,3,4000], 1),
       ARRAY_LOWER(ARRAY [-100,2,3,4], 1);
```

### Positions

```sql
SELECT array_position(array ['jan','feb','mar'], 'feb');

SELECT array_position(array [1,2,2,3,4], 2, 3);

SELECT array_positions(array [1,2,2,3,4], 2);
```

### Search, Replace, Remove

```sql
SELECT array_cat(array [1,2], array [3,4]);

SELECT array_append(array [1,2,3], 4);

SELECT array_remove(array [1,2,3,4,4,4], 4);

SELECT array_replace(array [1,2,3,4,4,4], 4, 5);
```

### IN, NOT IN, ANY

```sql
SELECT 20 in (1, 2, 3, 20) as "result";

SELECT 25 in (1, 2, 3, 20) as "result";

SELECT 25 not in (1, 2, 3, 20) as "result";

SELECT 20 = all (Array [20,22]), 20 = all (array [20,20]);

SELECT 20 = any (Array [1,2,25]) as "result"
```

### STRING TO Array

```sql
SELECT string_to_array('1,2,3,4,5', ',');

SELECT string_to_array('1,2,3,4,5,ABC', ',', 'ABC');

SELECT string_to_array('1,2,3,4,,6', ',', '');

SELECT array_to_string(ARRAY [1,2,3,4], '|');

SELECT array_to_string(ARRAY [1,2,3,4,NULL], '|', 'EMPTY');
```

