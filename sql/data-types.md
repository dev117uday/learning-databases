---
description: just like programming
---

# Data Types

## Boolean Data

* TRUE
* FALSE
* NULL

| TRUE | FALSE |
| :--- | :--- |
| TRUE | FALSE |
| 'true' | 'false' |
| 't' | 'f' |
| 'yes' | 'no' |
| 'y' | 'n' |
| '1' | '0' |

```sql
CREATE TABLE booltable (
    id SERIAL PRIMARY KEY ,
    is_enable BOOLEAN NOT NULL
);

INSERT INTO booltable (is_enable) VALUES (TRUE), ('true'), 
    ('y') , ('yes'), ('t'), ('1');
INSERT INTO booltable (is_enable) VALUES (FALSE), ('false'), 
    ('n') , ('no'), ('f'), ('0');
    
select * from booltable;

SELECT * FROM booltable WHERE is_enable = 'y';

SELECT * FROM booltable WHERE NOT is_enable;
```

## Character Data

| Character Type | Notes |
| :--- | :--- |
| CHARACTER \(N\), CHAR \(N\) | fixed-length, blank padded |
| CHARACTER VARYING \(N\), VARCHAR\(N\) | variable length with length limit |
| TEXT, VARCHAR | variable unlimited length, max 1GB |

* n is default to 1

```sql
select CAST('Uday' as character(10)) as "name";

-- output
"Uday      "

select 'Uday'::character(10) as "name";

-- output
"Uday      "

-- varchar
select 'uday'::varchar(10);
"uday"

-- text
select 'lorem ipsum'::text;
"lorem ipsum"
```

## Numeric Data

| Types | Notes |
| :--- | :--- |
| Integers | whole number, +ve and -ve |
| Fixed-point, floating point | for fractions of whole nu |

| type | size \(bytes\) | min | max |
| :--- | :--- | :--- | :--- |
| smallint | 2 | -32678 | 32767 |
| integer | 4 | -2,147,483,648 | 2,147,483,647 |
| bigint | 8 | -9223372036854775808 | 9223372036854775807 |

| type | size | range |
| :--- | :--- | :--- |
| smallserial | 2 | 1 to 32767 |
| serial | 4 | 1 to 2147483647 |
| bigserial | 8 | 1 to 9223372036854775807 |



