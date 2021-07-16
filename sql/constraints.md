# Constraints

* Constraints are like Gate Keepers. Control the type of data that goes into the table. These are used to prevent invalid data.
* Constraints can be added on
  * Table 
  * Column

### Types of Constraints

| Type | Note |
| :--- | :--- |
| NOT NULL | Field must have values |
| UNIQUE | Only unique value are allowed |
| DEFAULT | Ability to SET default  |
| PRIMARY KEY | Uniquely identifies each row/record |
| FOREIGN KEY | Constraint based on Column in other table |
| CHECK | Check all values must meet specific criteria |

### NOT NULL

```sql
CREATE TABLE table_nn (
    id SERIAL PRIMARY KEY,
    tag text NOT NULL
);

INSERT INTO table_nn ( tag ) 
    VALUES ('TAG 1'), ('TAG 2'), ('TAG 3'), ('');

-- NULL value wont be accepted
INSERT INTO table_nn ( tag ) VALUES (NULL);

-- empty string aren't NULL values
INSERT INTO table_nn ( tag ) VALUES ('');

SELECT * FROM table_nn;

-- adding another column TEXT;
ALTER TABLE table_nn 
ADD COLUMN is_enable TEXT;

-- Updating values in new column to '' 
-- so that NULL constrain can be added
UPDATE table_nn
SET is_enable = '' WHERE is_enable IS NULL;

-- adding null constraint on table
ALTER TABLE public.table_nn
    ALTER COLUMN is_enable SET NOT NULL;
```

### UNIQUE

```sql
CREATE TABLE table_unique (
    id SERIAL PRIMARY KEY,
    emails TEXT UNIQUE
);

INSERT INTO table_unique ( emailS ) 
    VALUES ('A@B.COM'),('C@D.COM');

INSERT INTO table_unique ( emailS ) 
    VALUES ('A@B.COM');

ALTER TABLE public.table_unique
    ADD COLUMN is_enable text;

ALTER TABLE table_unique
    ADD CONSTRAINT unique_is_enable UNIQUE (is_enable);


ALTER TABLE public.table_unique
    ADD COLUMN code text,
    ADD COLUMN code_key text;

ALTER TABLE table_unique
    ADD CONSTRAINT unique_code_key 
        UNIQUE (code,code_key);
```

### DEFAULT CONSTRAINT

```sql
CREATE TABLE table_default (
    id SERIAL PRIMARY KEY,
    is_enable TEXT DEFAULT 'Y'
);

INSERT INTO table_default ( id ) VALUES (1),(2);

select * from table_default;

ALTER TABLE public.table_default
    ADD COLUMN tag text;

ALTER TABLE public.table_default
    ALTER COLUMN tag SET DEFAULT 'N';

INSERT INTO table_default ( id ) VALUES (5);

select * from table_default;

ALTER TABLE public.table_default
    ALTER COLUMN tag DROP DEFAULT;
```

