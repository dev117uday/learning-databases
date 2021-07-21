# Sequences

## Sequences

* Specify datatype \( SMALLINT \| INT \| BIGINT \)
* Default is BIGINT

## List all sequence

```sql
SELECT relname as seq_name 
    from pg_class where relkind = 'S';
```

```sql
CREATE SEQUENCE IF NOT EXISTS test_sequence as bigint;

SELECT NEXTVAL('test_sequence');

SELECT CURRVAL('test_sequence');

SELECT setval('test_sequence',1);

SELECT setval('test_sequence',100);

-- set this value after the nextval is called, 
-- check using the currval cmd
SELECT setval('test_sequence',300,false);

ALTER SEQUENCE test_sequence RESTART WITH 100;

CREATE SEQUENCE IF NOT EXISTS test_seq3
INCREMENT 50
minvalue 100
MAXVALUE 1000
START WITH 150;

SELECT nextval('test_seq3');

CREATE SEQUENCE IF NOT EXISTS seq_des
INCREMENT -1
MINVALUE 1
MAXVALUE 999
START 99
NO CYCLE | CYCLE ;

SELECT nextval('seq_des');

-- DROP SEQUENCE

DROP SEQUENCE if exists seq_des;

create table if not exists table_seq (
    id INT primary key ,
    name varchar(10)
);

create sequence if not exists table_seq_id_seq
start with 1 owned by table_seq.id;

ALTER TABLE table_seq
ALTER COLUMN id SET DEFAULT nextval('table_seq_id_seq')


create sequence if not exists common_seq start with 100;

create table table_seq_share (
    id int default nextval('common_seq') not null ,
    name varchar(20)
);

create table table_seq_share2 (
    id int default nextval('common_seq') not null ,
    name varchar(20)
);
```

## Alpha-Numeric Sequence

```sql
create sequence table_text_seq;

create table contacts (
    id text not null default ('ID' || nextval('table_text_seq')),
    name varchar(150) not null
);

insert into  contacts (name) values ('uday 1'),('uday 2'),('uday 3');

select * from contacts;

alter sequence table_text_seq owned by contacts.id
```

