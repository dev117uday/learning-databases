# More on Triggers

## Getting Internal Information

```sql

CREATE OR REPLACE FUNCTION fn_trigger_variables_display()
RETURNS TRIGGER
LANGUAGE PLPGSQL
AS
$$

	BEGIN

		RAISE NOTICE 'TG_NAME: %', TG_NAME;
		RAISE NOTICE 'TG_RELNAME: %', TG_RELNAME;
		RAISE NOTICE 'TG_TABLE_SCHEMA: %', TG_TABLE_SCHEMA;
		RAISE NOTICE 'TG_WHEN: %', TG_WHEN;
		RAISE NOTICE 'TG_LEVEL: %', TG_LEVEL;
		RAISE NOTICE 'TG_OP: %', TG_OP;
		RAISE NOTICE 'TG_NARGS: %', TG_NARGS;
		RAISE NOTICE 'TG_ARGV: %', TG_NAME;

		RETURN NEW;
	END;
$$

CREATE TRIGGER trg_trigger_variables_display
	AFTER INSERT
	ON t_temperature_log
	FOR EACH ROW
	EXECUTE PROCEDURE fn_trigger_variables_display();

INSERT INTO t_temperature_log  ( add_date, temperature ) values ('2020-02-02', -40);
```

## Disallow DELETE on table

```sql

-- create sample table

create table test_delete (
	id int
);

insert into test_delete (id ) values (1),(2),(3);

select * from test_delete;

-- creating function

create or replace function fn_generic_cancel_op()
returns trigger
language plpgsql
as
$$
	begin
	
	if TG_WHEN = 'AFTER' THEN
		raise exception 'you are not allowed to % rows in %.%', tg_op,tg_table_schema,tg_table_name;
	end if;
	
	raise notice '% on rows in %.% wont happen', tg_op, tg_table_schema, tg_table_name;
	return null; 
	
	end;
$$

-- creating trigger : AFTER

CREATE TRIGGER trg_disallow_delete
AFTER DELETE
ON test_delete
FOR EACH ROW
EXECUTE PROCEDURE fn_generic_cancel_op();

delete from test_delete where id = 1;

-- creating trigger : BEFORE

CREATE TRIGGER trg_disallow_delete_before
BEFORE DELETE
ON test_delete
FOR EACH ROW
EXECUTE PROCEDURE fn_generic_cancel_op();

delete from test_delete where id = 1;

-- checking

select * from test_delete;
```

## Disallow truncating

```sql
CREATE TRIGGER trg_disallow_truncate_after
AFTER TRUNCATE
ON test_delete
FOR EACH STATEMENT
EXECUTE PROCEDURE fn_generic_cancel_op();


CREATE TRIGGER trg_disallow_truncate_beforE
BEFORE TRUNCATE
ON test_delete
FOR EACH STATEMENT
EXECUTE PROCEDURE fn_generic_cancel_op();
```

## Creating Audit Trigger

- To log data changes to tables in a consistent and transparent manner.

```sql
CREATE TABLE  audit (
    id INT
);

CREATE TABLE audit_log (
    username TEXT,
    add_time TIMESTAMP,
    table_name TEXT,
    operation TEXT,
    row_before JSON,
    row_after JSON
);
```

- Please not that new OLD are not null for DELETE and INSERT triggers.

```sql
CREATE OR REPLACE FUNCTION fn_audit_trigger()
RETURNS TRIGGER 
LANGUAGE PLPGSQL
AS
$$
BEGIN

    DECLARE
        old_row json = NULL;
        new_row json = NULL;

    BEGIN

        IF TG_OP IN ('UPDATE','DELETE') THEN

            old_row = row_to_json(OLD);


        END IF;

        IF TG_OP IN ('INSERT','UPDATE') THEN

            new_row = row_to_json(NEW);

        END IF;
    
        INSERT INTO audit_log 
            ( username, add_time, table_name, operation, row_before, row_after )
        values
            (
                session_user,
                NOW(),
                TG_TABLE_SCHEMA || '.' || TG_TABLE_NAME,
                TG_OP,
                old_row,
                new_row
            );
        
        RETURN NEW;
    END;    

END;
$$

-- bind trigger

CREATE TRIGGER trg_audit_trigger
AFTER INSERT OR UPDATE OR DELETE
ON audit
FOR EACH ROW
EXECUTE PROCEDURE fn_audit_trigger();

-- Queries

insert into audit(id) values (1),(2),(3);

update audit
set id = '100'
where id = 1;

delete from audit
where id = 2;

select * from audit;
select * from audit_log;

```

