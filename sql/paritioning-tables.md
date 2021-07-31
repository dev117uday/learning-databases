# Paritioning Tables

**It is splitting table into**

* logical division
* multiple smaller pieces
* more manageable pieces table

Partition leads to a huge performance boost. 

**Types of ranges**

* `Range` : The table is partitioned into "range" defined by a key column or set of columns, with no overlap between the ranges of values assigned to different partitions
* `List` : According to a key in table, ex : country, sales
* `Hash` : The partition specifying a modulus and a reminder for each partition. 

## Table Inheritance

```sql
create table master (
    pk INTEGER primary key ,
    tag text,
    parent integer
);

create table master_child() inherits (master);

ALTER TABLE public.master_child
    ADD PRIMARY KEY (pk);


select * from master;
select * from master_child;

insert into master (pk, tag, parent) values (1,'pencil',0);
insert into master_child (pk, tag, parent) values (2,'pen',0);

update master set tag = 'monitor' where pk = 2;

select * from only master;
select * from only master_child;

-- error
drop table master;
drop table master cascade ;
```

## Range Partitioning

```sql
create table employees_range (
    id bigserial,
    birth_date DATE NOT NULL ,
    country_code VARCHAR(2) NOT NULL
) PARTITION BY RANGE (birth_date);

CREATE TABLE employee_range_y2000 PARTITION of
    employees_range for values from ('2000-01-01') to ('2001-01-01');

CREATE TABLE employee_range_y2001 PARTITION of
    employees_range for values from ('2001-01-01') to ('2002-01-01');


insert into employees_range (birth_date, country_code) values
('2000-01-01','US'),
('2000-01-02','US'),
('2000-12-31','US'),
('2000-01-01','US'),
('2001-01-01','US'),
('2001-01-02','US'),
('2001-12-31','US'),
('2001-01-01','US');

select * from employees_range;
select * from only employees_range; -- you will get nothing
select * from employee_range_y2000;
select * from employee_range_y2001;

explain analyze select * from employees_range 
where birth_date = '2001-01-01';
```

