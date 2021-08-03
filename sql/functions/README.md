# Functions

## Some title

```sql
CREATE OR REPLACE FUNCTION function_name() RETURNS return_type as 
'
	-- SQL COMMAND 
' LANGUAGE SQL


CREATE OR REPLACE FUNCTION fn_my_sum( int, int ) RETURNS int as 
'
	SELECT $1 + $2;
' LANGUAGE SQL

SELECT fn_my_sum(1,2);


CREATE OR REPLACE FUNCTION fn_printer( text ) RETURNS text as 
$$
	SELECT 'Hello ' || $1 ;
$$ LANGUAGE SQL

SELECT fn_printer( 'Uday' );

CREATE OR REPLACE FUNCTION fn_printer( text ) RETURNS text as 
$body$
	SELECT 'Hello ' || $1 ;
$body$ LANGUAGE SQL

SELECT fn_printer( 'Uday' );


update employees 
set country = null
where employee_id = 1

select * from employees where employee_id = 1;

CREATE OR REPLACE FUNCTION fn_employee_update_country () returns void AS
$$

	update employees 
	set country = 'n/a'
	where country is NULL
$$ 
LANGUAGE SQL

SELECT fn_employee_update_country();

CREATE OR REPLACE FUNCTION fn_api_order_latest() RETURNS orders AS
$$

select *
from orders
order by order_date DESC
limit 1

$$
    LANGUAGE SQL
	
select (fn_api_order_latest()).*;

select (fn_api_order_latest()).order_date;

select order_date(fn_api_order_latest())


CREATE OR REPLACE FUNCTION fn_employee_hire_bydate( p_year integer ) returns setof employees as 
$$

	select * from employees
	where extract('YEAR' from hire_date) = p_year
	
$$
LANGUAGE SQL

SELECT (fn_employee_hire_bydate('1992')).*;

SELECT (fn_employee_hire_bydate('1992')).*;


CREATE OR REPLACE FUNCTION fn_orders()
    returns table
            (
                order_id SMALLINT,
                employee_id SMALLINT
            )
as
$$

select order_id, employee_id
from orders;

$$
    LANGUAGE SQL;
	
SELECT (fn_orders()).*;


CREATE OR REPLACE FUNCTION function_name 
	( x int default 0, y int DEFAULT 10 ) returns int as
$$ 

select x+y;

$$
LANGUAGE SQL;

SELECT function_name();

DROP FUNCTION [ IF EXISTS ] function_name 
	( argument_list ) ( cascade | restrict );
```

