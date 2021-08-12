---
description: The Best
---

# Getting Started with PostgreSQL

## Starting Database with Docker

```bash
sudo docker run  \
 --name <docker_name> \ 
 -e POSTGRES_PASSWORD=<password> \ 
 -d -p 5432:5432 postgres:13.3

sudo docker exec -it <docker_name> bash

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

**Switch to default user**

```text
sudo su postgres
```

## Getting Started

### Connect to a database

```bash
# for help 
psql --help

# to connect to DB using credentials
psql -h [host] -p [port] -U [user] [DB]
```

1. Here port : 5432 is default and can be get from `psql --help`
2. `postgres` is the **super user**. _Create another user and connect using that._

