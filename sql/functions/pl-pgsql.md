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

CREATE OR REPLACE FUNCTION function_name(int, int) returns int as

$$

DECLARE
    x alias for $1;
    y alias for $2;

BEGIN
    --
end;
$$







```

