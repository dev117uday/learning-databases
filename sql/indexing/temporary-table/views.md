# Views

## Plain View

A **view** is a database object that is a stored query. A view is a virtual table you can create dynamically using a saved query acting as a `virtual table.`

* You can join a view to another table or view
* You can query a view
* Regular views don't store any data, but materialised view does

```sql
-- syntax
-- CREATE OR REPLACE VIEW view_name AS query

CREATE OR REPLACE VIEW v_movie_quick AS
SELECT movie_name,
       movie_length,
       release_date
from movies mv;

select * from v_movie_quick;

CREATE OR REPLACE VIEW v_movie_d_name as
SELECT movie_name,
       movie_length,
       release_date,
       d.first_name || ' ' || d.last_name as "full name"
from movies mv
         inner join directors d on mv.director_id = d.director_id;

select * from v_movie_d_name;

ALTER VIEW v_movie_d_name RENAME TO v_movie_with_names;

select * from v_movie_with_names;

DROP VIEW v_movie_quick;

-- CONDITIONAL VIEW

CREATE OR REPLACE VIEW v_movie_after_1997 as
select *
from movies
where release_date >= '1997-12-31'
  and movie_lang = 'English'
order by release_date desc;

select * from v_movie_after_1997;

-- you cannot add, update, delete columns from a view once created, for that create a new view
-- drop the old view and rename the new view back to original

CREATE OR REPLACE VIEW v_movie as
select movie_name
from movies;

select * from v_movie;

select * from movies;

delete from movies where movie_id = 54;
```

## Materialized View

Allows you to

* store result of a query
* update data periodically :: manual
* used to cache result of heavy data

  syntax

```sql
CREATE MATERIALIZED VIEW IF NOT EXISTS view_name AS query WITH [ NO ] DATA;
```

If you want to load data into the materialised view at the creation time, you will use WITH DATA

```sql
create materialized view if not exists mv_dir as
select first_name, last_name
from directors with no data ;

select * from mv_dir;

refresh materialized view concurrently mv_dir;

select * from mv_dir;

drop materialized view mv_dir;
```

* cannot change data in `materialised view` : _insert, update and delete_ 
* **advantage** of using`materialised view` : access and update _mv_ without locking everyone else out 
* **disadvantage** of using _mv_ : if you alter the base table , _mv_ must also me alter : delete the old _mv_ and create a new one

