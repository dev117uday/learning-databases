---
description: The Best
---

# Learning PostgreSQL

#### Starting Database

```sql
sudo docker pull postgres:13.3
sudo docker run --name bootcamp -e POSTGRES_PASSWORD=<password> -d -p 5432:5432 postgres:13.3
sudo docker exec -it bootcamp bash
psql -U postgres
```



