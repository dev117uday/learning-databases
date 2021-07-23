---
description: the usual
---

# JSON

## JSON vs JSONB

| JSON | JSONB |
| :--- | :--- |
| stores data in text format | stores data in binary format |
| stores data as-is  | trims of white spaces |
| slower in operations | faster in operations |
| doesn't support full text indexing | supports full text indexing  |

```sql
select '{
    "title":"book 1"}
'::json;

select '
  {"title":"book 1"}
  '::jsonb
```

## Operations

```sql
create table books_jsonb
(
    id        serial primary key,
    book_info jsonb
);

insert into books_jsonb (book_info)
values ('{
  "title": "Book 1"
}'),
       ('{
         "title": "Book 2"
       }'),
       ('{
         "title": "Book 3"
       }');

select id, book_info ->> 'title' as "title"
from books_jsonb;

select id, book_info ->> 'title' as "title"
from books_jsonb
where book_info ->> 'title' = 'Book 1';

insert into books_jsonb (book_info)
values ('{
  "title": "Book 10"
}');

select *
from books_jsonb;

update books_jsonb
set book_info = book_info || '{"title": "Book 4" }'
where book_info ->> 'title' = 'Book 10';

select *
from books_jsonb;

update books_jsonb
set book_info = book_info || '{"author": "author 1" }'
where book_info ->> 'title' = 'Book 1';

select *
from books_jsonb;

update books_jsonb
set book_info = book_info - 'author'
where book_info ->> 'title' = 'Book 1';

select *
from books_jsonb;

update books_jsonb
set book_info = book_info || '{"available":["new delhi","Tokyo","sydney"]}'
where book_info ->> 'title' = 'Book 1';

select *
from books_jsonb;

update books_jsonb
set book_info = book_info #- '{available,1}'
where book_info ->> 'title' = 'Book 1';

select *
from books_jsonb;
```

## ROW\_TO\_JSON\(\)

```sql
select row_to_json(directors)
from directors;

select row_to_json(t)
from (
         select *
         from directors
     ) as t;
```

## JSON\_AGG\(\)

```sql
select *
from movies;

select director_id, first_name, last_name, (
           select json_agg(x)
           from (
                    select movie_name
                    from movies mv
                    where mv.director_id = directors.director_id
                ) as x
       ) :: jsonb
from directors;
```

## JSON\_BUILD

```sql
select json_build_array(1, 2, 3, 4, 5, 6);

select json_build_array(1, 2, 3, 4, 5, 6, 'Hi');

-- error
select json_build_object(1, 2, 3, 4, 5);

select json_build_object(1, 2, 3, 4, 5, 6, 7, 'Hi');

select json_object('{name,email}', '{"adnan","a@b.com"}');
```

## Json Functions

```sql
CREATE TABLE directors_docs
(
    id   serial primary key,
    body jsonb
);


select director_id,
       first_name,
       last_name,
       (
           select json_agg(x) as all_movies
           from (
                    select movie_name
                    from movies mv
                    where mv.director_id = directors.director_id
                ) x
       ) :: jsonb
from directors;


INSERT INTO directors_docs (body)
select row_to_json(a)
from (
         select director_id,
                first_name,
                last_name,
                (
                    select json_agg(x) as all_movies
                    from (
                             select movie_name
                             from movies mv
                             where mv.director_id = directors.director_id
                         ) x
                ) :: jsonb
         from directors
) as a;

select *
from directors_docs;

select *, jsonb_array_length(body -> 'all_movies') as total_movies
from directors_docs
order by jsonb_array_length(body->'all_movies') DESC;

select *,jsonb_object_keys(body) from directors_docs;

select j.key, j.value
from directors_docs,
     jsonb_each(body) j;
```

## Existence Operators

```sql
select *
from directors_docs
where body -> 'first_name' ? 'John';
```

## Searching JSON

```sql
select *
from directors_docs
where body @> '{"first_name":"John"}';


select *
from directors_docs
where body @> '{"director_id":1}';

-- error
select *
from directors_docs
where body -> 'first_name' LIKE 'J%';


select *
from directors_docs
where body ->> 'first_name' LIKE 'J%';

select *
from directors_docs
where (body ->> 'director_id')::integer in (1,2,3,4,5,10);
```



