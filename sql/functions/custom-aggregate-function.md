# Custom Aggregate function

fix this

```sql
CREATE TABLE IF NOT EXISTS t_taxi
(
    id_trip int,
    km      numeric
);

INSERT INTO t_taxi (id_trip, km)
VALUES (1, 3.5),
       (1, 4.2),
       (1, 1.2),
       (1, 3.5),
       (2, 1.8),
       (2, 3.2);

CREATE OR REPLACE FUNCTION taxi_all_rows(numeric, numeric, numeric)
    returns numeric as
$$
    select $1 + $2*$3;
$$
language 'sql' strict;

CREATE OR REPLACE FUNCTION taxi_trip_sum(numeric)
    returns numeric as
$$ select $1;
    $$
    language 'sql'
    strict;

CREATE AGGREGATE agg_taxi(numeric,numeric)
(
    INITCOND = 5,
    STYPE = numeric,
    SFUNC = taxi_all_rows,
    finalfunc = taxi_trip_sum
);

select id_trip,km, agg_taxi(km,2.5)
from t_taxi group by id_trip;
```

