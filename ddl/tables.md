---
description: better excel sheet
---

# Tables

## Creating Table 

![](../.gitbook/assets/output%20%281%29.gif)

## Altering Table

![](../.gitbook/assets/output%20%282%29.gif)

## Delete Table

```sql
drop table if exists temp;
```

## Insert 

```sql
INSERT INTO table (col1, col2) VALUES ( 'VALUE 1' , 'VALUE 2');

-- MULTIPLE
INSERT INTO table (col1, col2) VALUES ( 'VALUE 1' , 'VALUE 2','VALUE 3','VALUE 4');

-- STRING WITH QUOTES , add another ' before '
INSERT INTO table (col1) VALUES ( 'VALUE''S 1' );

-- RETURNING ROWS
INSERT INTO table (first_name, last_name, gender, date_of_birth) values ('Uday','Yadav','F',now()) RETURNING *;
```





