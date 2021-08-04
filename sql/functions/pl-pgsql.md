# PL/pgSQL

```sql
DO
$$

DECLARE
    mynum   integer := 89;
    first_name varchar(20) := 'Uday';
    hire_date date := '2020-01-01';
    start_time timestamp := NOW();
    emptyvar integer;

BEGIN

    RAISE NOTICE 'My variable % % % % %',
    mynum,
    first_name,
    hire_date,
    start_time,
    emptyvar ;

END;

$$

CREATE OR REPLACE FUNCTION function_name 
	(int, int) returns int as

$$

DECLARE
    x alias for $1;
    y alias for $2;

BEGIN
    --
end;
$$


select * from products into product_row	limit 1;

DO
$$
	DECLARE 
		product_title products.product_name%TYPE;
	BEGIN
		SELECT product_name FROM products 
		INTO product_title
		where product_id = 1 limit 1;
		
		RAISE NOTICE 'Your product name is %', product_title;
	END;
$$		

CREATE OR REPLACE FUNCTION fn_sum_using_inout 
	( IN x integer, IN y integer, OUT Z integer ) as 
$$

	BEGIN
		z := x+y;
	END;
	
$$
LANGUAGE PLPGSQL;

select fn_sum_using_inout(2,3);


CREATE OR REPLACE FUNCTION fn_sum_using_inouts 
	( IN x integer, IN y integer, OUT Z integer, OUT w integer ) as 
$$

	BEGIN
		z := x+y;
		w := x*y;
	END;
	
$$
LANGUAGE PLPGSQL;

select * from fn_sum_using_inouts(2,3);


DO
$$
	<< Parent >>
	
	DECLARE
		counter integer := 0;
	BEGIN
		counter := counter+1;
		RAISE NOTICE 'the current value of counter (IN PARENT) is %', counter;
		
		DECLARE 
			counter integer := 0;
		BEGIN
			counter := counter + 5;
			RAISE NOTICE 'The current value of counter at subblocks is %', counter;
			RAISE NOTICE 'The parent value of counter at subblocks is %', PARENT.counter;
		END;
		
		counter := counter + 5;
		RAISE NOTICE 'the current value of counter (IN PARENT) is %', counter;
		
	END;
$$
LANGUAGE PLPGSQL;


CREATE OR REPLACE FUNCTION fn_order_by_date_pro() RETURNS SETOF orders AS
$$

	BEGIN
		
		RETURN QUERY SELECT * FROM orders limit 10;
		
	END;

$$
LANGUAGE PLPGSQL;

SELECT * FROM fn_order_by_date_pro();


CREATE OR REPLACE FUNCTION fn_which_is_greater
	( x integer default 0, y integer default 0 ) RETURNS text AS
$$

		BEGIN
		
			IF x > y then 
				return ' x > y ';
			else 
				return ' x < y ';
			end if ;
			
		END;
		
$$ LANGUAGE PLPGSQL;

SELECT  fn_which_is_greater(4,3);

CREATE OR REPLACE FUNCTION fn_checker 
	( x integer default 0 ) RETURNS text AS
$$

		BEGIN
		
			CASE x
				when 10 then
					return 'value = 10';
				when 20 then
					return 'value = 20';
				else
					RETURN 'MORE';
			END CASE;
			
		END;
		
$$ LANGUAGE PLPGSQL;

SELECT fn_checker(30);


DO
$$

	DECLARE 
		i_counter integer = 0;
	BEGIN
		LOOP
			RAISE NOTICE '%', i_counter;
			i_counter := i_counter+1;
			EXIT WHEN
				i_counter = 5;
		END LOOP;
	END;
$$ LANGUAGE PLPGSQL;

DO
$$

	BEGIN
		
		FOR counter IN 1..5 BY 1
		LOOP
		
			RAISE NOTICE 'COUNTER : %', counter;
		
		END LOOP;		
	END;

$$
LANGUAGE PLPGSQL;

DO
$$

	BEGIN
		
		FOR counter IN REVERSE 5..1 BY 1
		LOOP
		
			RAISE NOTICE 'COUNTER : %', counter;
		
		END LOOP;		
	END;

$$
LANGUAGE PLPGSQL;


DO
$$
	DECLARE 
		rec record ;
	BEGIN
		FOR rec in 
			select order_id, customer_id from orders LIMIT 10
		LOOP
			RAISE NOTICE '% %', rec.order_id, rec.customer_id;
		END LOOP; 
	END;
$$ LANGUAGE PLPGSQL

```

