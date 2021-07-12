---
description: better excel sheet
---

# Tables

## Creating Table 

![](../.gitbook/assets/output%20%281%29.gif)

## Altering Table

![](../.gitbook/assets/output%20%282%29.gif)

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

