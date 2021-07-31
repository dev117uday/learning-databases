# Window



```sql
create table trades
(
    region  text,
    country text,
    year    int,
    imports numeric(50, 0),
    exports numeric(50, 0)
);

select *
from trades
limit 10;

drop table trades;

select region, round(avg(imports), 2)
from trades
group by rollup (region);

select region, country, round(avg(imports), 2)
from trades
where country in ('USA', 'Argentina', 'Singapore', 'Brazil')
group by rollup (region, country)
order by 1;


select region, country, round(avg(imports / 1000000))
from trades
where country in ('USA', 'France', 'Germany')
group by cube (region, country);


select region, country, round(avg(imports) / 1000000, 2)
from trades
where country in ('USA', 'FRANCE', 'Germany')
group by
    grouping sets ( (), country, region );

explain
select region, country, round(avg(imports) / 1000000, 2)
from trades
where country in ('USA', 'FRANCE', 'Germany')
group by
    grouping sets ( (), country, region );


select region,
       avg(exports)                                     as avg_all,
       avg(exports) filter ( where trades.year < 1995 ) as avg_old,
       avg(exports) filter ( where trades.year >= 1995) as avg_latest
from trades
group by
    rollup (region);

SELECT AVG(imports), avg(exports)
FROM trades;

select country, year, imports, exports, avg(exports) over () as avg_exports
from trades;

select country, year, imports, exports, avg(exports) over (partition by country) as avg_exports
from trades;

select country, year, imports, exports, avg(exports) over (partition by year < 2000 ) as avg_exports
from trades;


select country, year, exports, min(exports) over (PARTITION BY country order by year)
from trades
where year > 2001
  and country in ('USA', 'France');
```

