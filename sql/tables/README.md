---
description: better excel sheet
---

# Tables

## Creating Table 

![](../../.gitbook/assets/output%20%281%29%20%281%29.gif)

## Altering Table

![](../../.gitbook/assets/output%20%282%29.gif)

```sql
ALTER TABLE public.accounts
    ADD COLUMN is_enable boolean;
    
ALTER TABLE public.accounts
    RENAME username TO user_name;
```

## Delete

### Table

```sql
drop table if exists temp;
```

### Row

```sql
delete from actors where actor_id = 150 ;
```

### Deleting Column/Constraint

```sql
ALTER TABLE users DROP column created_at;
ALTER TABLE person DROP CONSTRAINT person_ pkey;
```

## Select

The `SELECT` statement has the following clauses:

* Select distinct rows using **DISTINCT** operator.
* Sort rows using **ORDER BY** clause.
* Filter rows using **WHERE** clause.
* Select a subset of rows from a table using **LIMIT** or **FETCH** clause.
* Group rows into groups using **GROUP BY** clause.
* Filter groups using **HAVING** clause.
* Join with other tables using joins such as INNER JOIN, LEFT JOIN, FULL OUTER JOIN, CROSS JOIN clauses.
* Perform set operations using **UNION**, **INTERSECT**, and **EXCEPT**.

```sql
SELECT first_name,second_name FROM person;
```

| Representation | Function |
| :--- | :--- |
| ASC | Ascending |
| DESC | Descending |

```sql
SELECT * FROM person;

SELECT * FROM person ORBER BY dob ASC|DESC;
SELECT DISTINCT gender FROM person;

-- to select one column with condtion
SELECT gender FROM person WHERE gender = 'Female'; 

-- to select all row with condition on one column
SELECT * FROM person WHERE gender = 'Female'; 

SELECT * FROM person 
    WHERE gender = 'Male' AND last_name='England';!
```

## Column Alias

```sql
SELECT 
   first_name || ' ' || last_name as name,
   email
FROM 
   customer;

-- with spaces
SELECT
    first_name || ' ' || last_name "full name"
FROM
    customer;
```

## Insert 

```sql
INSERT INTO table (col1, col2) 
    VALUES ( 'VALUE 1' , 'VALUE 2');

-- MULTIPLE
INSERT INTO table (col1, col2) 
    VALUES ( 'VALUE 1' , 'VALUE 2','VALUE 3','VALUE 4');

-- STRING WITH QUOTES , add another ' before '
INSERT INTO table (col1) 
    VALUES ( 'VALUE''S 1' );

-- RETURNING ROWS
INSERT INTO table (first_name, last_name, gender, 
    date_of_birth) values 
    ('Uday','Yadav','F',now()) 
    RETURNING *;
```

## Updating

```sql
update table1 set col1 = 'M' where id = 150;

-- returning updated row
update actors set gender = 'M' where actor_id = 150 
    returning *;

-- update all recors
update actors set paid = 'N';


UPDATE person
SET email = 'not found'
WHERE
    email IS NULL;
```

## Upsert

```sql
INSERT INTO tablename ( col_list ) VALUES 
    ( value_list ) ON CONFLICT (COL_NAME)
    DO
        NOTHING 
        -- OR
        UPDATE SET col = val where condition;
        
        
INSERT INTO tablename ( COL_NAME ) VALUES 
    ( value_list ) ON CONFLICT (COL_NAME)
    DO
        UPDATE SET COL_NAME = EXCLUDED.COL_NAME;
```

