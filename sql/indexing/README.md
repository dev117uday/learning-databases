# Indexing

* Index is a structure relation. 
* An index help improve the access of data in our database 
* Indexed tuple point to the table page where the tuple is stored.
* An Index is a data structure that allows faster access to the underlying table so that specific tuples can be found quickly . Here "quickly" means much faster than scanning the entire table and analysing every single tuple.
* They add a cost to running a query.

```sql
INDEX        : idx_table_name_column_name 
UNIQUE INDEX : idx_u_table_name_column_name 
```

```sql
create index index_name on table_name (col1,col2,.....)

-- create index for only unique values in column
create unique index index_name on table_name (col1,col2,.....)


create index index_name on table_name [USING method]
(
	column_name [ASC|DESC] [NULLS {FIRST | LAST}],
	...
);


create index idx_orders_order_date 
 on orders (order_date);

create index idx_orders_ship_city 
 on orders (ship_city);

create index idx_orders_customer_id_order_id 
 on orders (customer_id,order_id);

CREATE INDEX idx_shippers_company_name
    ON public.shippers USING btree
    (company_name ASC NULLS LAST);
```

## Get All Indexes

```sql
select * from pg_indexes;

select * from pg_indexes where schemaname = 'public';
```

## Size of Indexes

```sql
select pg_size_pretty(pg_indexes_size('tablename'));
```

## Stats about indexes

```sql
select * from postgres.pg_catalog.pg_stat_all_indexes;
```

## Drop Index

```sql
DROP INDEX [ concurrently ] 
[ IF EXISTS  ] INDEX_NAME  [ CASCADE | RESTRICT ];
```

* `CASCADE`: If object has dependent objects, you will also drop the dependent ones after dropping it.
* `RESTRICT` : It denies the user to drop the index if a dependency exists
* `CONCURRENTLY` : PostgreSQL will require exclusive lock over the whole table and block access until index is removed

## vacuum analyze

```sql
vacuum analyze table_name;
```

