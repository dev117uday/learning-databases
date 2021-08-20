# Advance Tables

### Generated Columns

* faster than triggers

```sql
create table if not exists area (
    w real,
    h real,
    area real GENERATED ALWAYS AS ( w*h ) STORED
);

INSERT INTO area (w, h) values (2,3),(4,7);

select * from area;

update area
set w = 10
where w = 4

select * from area;
```

