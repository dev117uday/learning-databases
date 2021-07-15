---
description: yours truely
---

# User Defined Data Types

## CREATE DOMAIN

* Create user defined data type with a range, optional, DEFAULT, NOT NULL and CHECK Constraint.
* They are unique within schema scope.
* Helps standardise your database types in one place.
* Composite Type : Only single value return

```sql
CREATE DOMAIN name datatype constraint

-- ex 1
-- 'addr' with domain VARCHAR(100)
CREATE DOMAIN addr VARCHAR(100) NOT NULL

CREATE TABLE locations (
	address addr
);

-- ex 2
-- 'positive_numeric' : value > 0

select * from locations;

CREATE DOMAIN positive_numeric 
	INT NOT NULL CHECK (VALUE > 0);

CREATE TABLE sample (
	number positive_numeric
);

INSERT INTO sample (NUMBER) VALUES (10);
INSERT INTO sample (NUMBER) VALUES (-10);

SELECT * FROM sample;

-- ex 3
-- check email domain

CREATE DOMAIN 
 	proper_email VARCHAR(150) 
CHECK 
	( VALUE ~* '^[A-Za-z0-9._%-]+@[A-Za-z0-9.-]+[.][A-Za-z]+$' );
	

CREATE TABLE email_check (
	client_email proper_email
);

insert into email_check (client_email) 
	values ('a@b.com') ;
insert into email_check (client_email) 
	values ('a@#.com') ;
	

-- enum based domain
CREATE DOMAIN valid_color VARCHAR(10)
CHECK (VALUE IN ('red','green','blue'))

CREATE TABLE color (
	color valid_color
);

INSERT INTO color (color) 
	VALUES ('red'),('blue'),('green')
INSERT INTO color (color) 
	VALUES ('yellow')
	
	
select typname from pg_catalog.pg_type join pg_catalog.pg_namespace on pg_namespace.oid = pg_type.typnamespace
where typtype = 'd' and nspname = 'public';

drop domain proper_email;	
drop domain proper_email CASCADE;	
```

## Composite Data Types

**Syntax :** `(composite_column).city`

```sql
--  ex1 
-- address type

CREATE TYPE address AS (
	city VARCHAR(50),
	country VARCHAR(100)
);

CREATE TABLE person (
	id SERIAL PRIMARY KEY,
	address address
);

INSERT INTO person ( address ) VALUES (ROW('London','UK')), (ROW('New York','USA'));

select * from person;

select (address).country from person;



CREATE TYPE currency AS ENUM(
	'USD','EUR','GBP','CHF'
);

SELECT 'USD'::currency

ALTER TYPE currency ADD VALUE 'CHF' AFTER 'EUR';

CREATE TABLE stocks (
	id SERIAL PRIMARY KEY,
	symbol currency
);

insert into stocks ( symbol ) VALUES ('CHF');

select * from stocks

-- DROP TYPE currency;
```



