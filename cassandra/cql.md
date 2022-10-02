# CQL

### Create KeySpace&#x20;

( same as db in SQL )

```sql
CREATE KEYSPACE mykeyspace WITH REPLICATION = { 
		'class' : 'NetworkTopologyStrategy', 
		'replication_factor' : 3
};
	
use mykeyspace;

DESCRIBE mykeyspace;

CREATE TABLE users ( 
	user_id int, 
	fname text, 
	lname text, 
	PRIMARY KEY((user_id))
); 
```

### Create Table & Insert Data

```sql
CREATE TABLE users ( 
	user_id int, 
	fname text, 
	lname text, 
	PRIMARY KEY((user_id))
); 

insert into users(user_id, fname, lname) values (1, 'rick', 'sanchez'); 
insert into users(user_id, fname, lname) values (4, 'rust', 'cohle'); 

select * from users;

CONSISTENCY QUORUM | ALL
```

### More Queries

```sql
SELECT * from 
    heartrate_v1 
WHERE 
    pet_chip_id = 123e4567-e89b-12d3-a456-426655440b23;
    
DELETE FROM 
    heartrate_v1 
WHERE 
    pet_chip_id = 123e4567-e89b-12d3-a456-426655440b23;

```

