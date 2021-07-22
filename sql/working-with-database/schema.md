# Schema

* PostgreSQL schema's should be unique and different from each other
* Allows you to organise database objects
* Schema allow multiple users to interact with one database without interfering with each other
* Allow access and limit database objects to be accessed by the user.

```sql
CREATE SCHEMA sales;

CREATE SCHEMA hr;

ALTER SCHEMA sales RENAME TO marketing;

DROP SCHEMA hr;

select * from hr.public.jobs;

CREATE TABLE orders ( id SERIAL PRIMARY KEY );

ALTER TABLE public.orders SET SCHEMA marketing;

select current_schema();

show search_path;

-- order to search path is important 
SET search_path to '$user', marketing, public;
```

## PG\_CATALOG

* PostgreSQL stores the metadata information about the database and cluster in the schema `pg_catalog`. 
* This information is partially used by PostgreSQL itself to keep track things itself, but it also presented so external people/processes can understand whats inside the database.
* `pg_catalog` schema contains system tables and all the built-in types, functions and operators.
* `pg_catalog` is effectively part of the search path. Although if it is not named explicitly in the path then it is implicitly searched before searching the path's schema.

`select * from information_schema.schemata;`

