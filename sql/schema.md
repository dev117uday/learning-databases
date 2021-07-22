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

CREATE TABLE orders (
    id SERIAL PRIMARY KEY
);

ALTER TABLE public.orders
  SET SCHEMA marketing;

select current_schema();

show search_path;

-- order to search path is important
SET search_path to '$user', marketing, public;
```

