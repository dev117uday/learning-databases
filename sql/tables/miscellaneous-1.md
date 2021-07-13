# Miscellaneous

**Order of execution of SQL statements**

1. FROM
2. WHERE
3. SELECT
4. ORDER BY

## Concatenation Operator

```sql
select concat(first_name,last_name) as full_name 
    from actors limit 10;

select concat_ws(' ',first_name,last_name) as full_name 
    from actors limit 10;
```

* if you can have a null value in column, always use concat\_ws becuz it willl place nothing in that and and also not place the spacer like **\|** or a space
* 
