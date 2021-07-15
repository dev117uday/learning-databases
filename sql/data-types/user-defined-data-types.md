---
description: yours truely
---

# User Defined Data Types

## CREATE DOMAIN

* Create user defined data type with a range, optional, DEFAULT, NOT NULL and CHECK Constraint.
* They are unique within schema scope.
* Helps standardise your database types in one place.
* Composite Type : Only single value return

```sql
CREATE DOMAIN name datatype constraint

-- ex 1
-- 'addr' with domain VARCHAR(100)
CREATE DOMAIN addr VARCHAR(100) NOT NULL


```

* 
