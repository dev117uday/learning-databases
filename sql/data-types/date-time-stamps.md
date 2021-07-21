# Date/Time/Stamps

## Set Date Time Style

```sql
-- show system date style
SHOW datestyle;

-- set new datestyle
SET datestyle = 'ISO, DMY';
SET datestyle = 'ISO, MDY';
```

## Make

```sql
SELECT MAKE_DATE (2020,01,01);

SELECT MAKE_TIME(2,3,14.65);

SELECT MAKE_TIMESTAMP (2020,02,02,10,20,45.44);
```

## Make\_interval

```sql
SELECT MAKE_INTERVAL (2020,01,02,10,20,33);
SELECT MAKE_INTERVAL (days => 10);
SELECT MAKE_INTERVAL (months => 7, days => 10, mins=>35); 
SELECT MAKE_INTERVAL (weeks => 10);
```

## Make\_timestamptz

```sql
SELECT make_timestamptz(2020,02,02,10,30,45.55,'Asia/Calcutta');

SELECT pg_typeof(make_timestamptz(2020,02,02,10,30,45.55));
```

## Date Value Extractor

{% embed url="https://www.postgresql.org/docs/8.1/functions-datetime.html" caption="" %}

{% embed url="https://www.postgresqltutorial.com/postgresql-extract/" caption="" %}

## Extract

```sql
select
    extract ('day' from current_timestamp),
    extract ('month' from current_timestamp),
    extract ('year' from current_timestamp);

select extract('epoch' from current_timestamp);

select extract('century' from current_timestamp);
```

## Maths Operations on Date Time

```sql
select '2020-02-02'::date + 04;

select '23:59:59' + INTERVAL '1 SECOND';
select '23:59:59' + INTERVAL '2 SECOND';

SELECT CURRENT_TIMESTAMP + '01:01:01';

SELECT DATE '20200101' + TIME '10:25:10';

SELECT '10:10:10' + TIME '10:25:10';

SELECT DATE '20200101' - INTERVAL '1 HOUR';

SELECT INTERVAL '30 MINUTES' + '2 HOUR';
```

## Overlap

```sql
select
    ( DATE '2020-01-01' , DATE '2020-12-31' )
    OVERLAPS
    ( DATE '2020-12-30', DATE '2020-12-01' );
```

## Current

```sql
select
    current_date,
    current_time,
    current_time(2),
    current_timestamp,
    localtime,
    localtimestamp,
    localtimestamp(2);

select
   now(),
   transaction_timestamp(),
   clock_timestamp(),
   statement_timestamp(),
   timeofday();
```

## Age

```sql
select age('2020-01-01', '2019-10-01');

select age(timestamp '2020-01-01');

select age(current_date, '2020-01-01');
```

## Epochs

```sql
select age ( timestamp '2020-12-20', timestamp '2020-10-20' );

SELECT 
    EXTRACT (EPOCH FROM TIMESTAMPTZ '2020-10-20')
    - EXTRACT (EPOCH FROM TIMESTAMPTZ '2020-08-20') 
        AS "DIFFERENCE IN SECONDS"
```

## Timezone

```sql
select * from pg_timezone_names;

select * from pg_timezone_abbrevs;

SHOW TIME ZONE;

SET TIME ZONE 'Asia/Calcutta';
```

## date\_part and date\_trunc

```sql
select date_part ('day', date '2021-11-07');

select date_trunc('hour', 
    timestamptz '2021-07-16 23:38:40.775719 +05:30');
```

