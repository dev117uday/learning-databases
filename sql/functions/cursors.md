---
description: '// TODO : Complete this section'
---

# Cursors

* Rows returned by the SQL query are those which match the condition. It can either be zero or more at a time.
* Sometimes you need to traverse through the rows one by one, forward or backwards
* Life Cycle of Cursor
  * DECLARE 
  * OPEN
  * FETCH
  * CLOSE

```sql

DO
$$
    DECLARE
        output_text text DEFAULT '';
        rec_movie orders;
        cur_all_movies CURSOR FOR SELECT * FROM orders WHERE order_date > date '1998-01-01';

    BEGIN
        OPEN cur_all_movies;
        LOOP
            FETCH cur_all_movies INTO rec_movie;
            EXIT WHEN NOT FOUND;
            RAISE NOTICE 'Output : % \n', rec_movie.order_date;
        end loop;
    end;
$$
```

