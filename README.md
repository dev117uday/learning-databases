---
description: The Best
---

# Learning PostgreSQL

## Starting Database with Docker

```sql
sudo docker pull postgres:13.3
sudo docker run --name bootcamp -e POSTGRES_PASSWORD=<password> -d -p 5432:5432 postgres:13.3
sudo docker exec -it bootcamp bash
psql -U postgres
```

## Setup and Basics : using apt

**Installation**

```bash
sudo apt-get install postgresql
```

**Usage commands**

```bash
service postgresql
```

**Directories**

```bash
etc/postgresql/12/main/
```

**Switch to default user**

```text
sudo su postgres
```

**Launch PostgreSQL shell**

```bash
psql
```

## Getting Started

#### Connect to a database

```sql
psql --help
psql -h localhost -p 5432 -U postgres test
```

1. Here port : 5432 is default and can be get from psql --help
2. PostgreSQL is the super user, replace with your user
3. test is the name of database

