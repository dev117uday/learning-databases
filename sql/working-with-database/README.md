---
description: not file system
---

# Database

## Creating a database

![creating database](../../.gitbook/assets/create-database%20%281%29%20%281%29%20%281%29%20%282%29%20%283%29%20%283%29%20%284%29%20%285%29%20%286%29%20%287%29%20%282%29%20%281%29%20%281%29%20%281%29%20%287%29.gif)

## Dropping Database

![drop database](../../.gitbook/assets/output%20%284%29.gif)

## Commands

```sql
create database testdb;

drop database if exists testdb;
```

### Connect to DB

```sql
-- using psql
\c test
```

### See list of tables

```sql
-- using psql
\d                       #to see tables
\d <name of table>       #to see details about table
```

