---
description: not file system
---

# Databases

## Creating a database

![creating database](../.gitbook/assets/create-database%20%281%29%20%281%29%20%281%29%20%282%29%20%281%29.gif)

### Options

* **Definition**

  // TODO : complete all

  * Template 
  * Table-space
  * Collation
  * Character Type

## Dropping Database

![drop database](../.gitbook/assets/output%20%284%29.gif)

## Commands

```sql
create database testdb;

drop database if exists testdb;
```

### Connect to DB

```sql
\c test
```

### See list of tables

```sql
\d                       #to see tables
\d <name of table>       #to see details about table
```

