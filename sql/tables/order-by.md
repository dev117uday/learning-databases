---
description: a clause
---

# ORDER BY and DISTINCT

```sql
SELECT
    contactLastname,
    contactFirstname
FROM
    customers
ORDER BY
    contactLastname DESC,
    contactFirstname ASC LIMIT 10;


SELECT 
    orderNumber, 
    orderlinenumber, 
    quantityOrdered * priceEach as final_price
FROM
    orderdetails
ORDER BY 
   final_price DESC LIMIT 10;

-- using alias in order by
SELECT
    first_name,
    last_name as surname
FROM
    actors
ORDER BY
    surname DESC ;


SELECT 
    orderNumber, 
    status
FROM
    orders
ORDER BY 
    FIELD(status,
        'In Process',
        'On Hold',
        'Cancelled',
        'Resolved',
        'Disputed',
        'Shipped') LIMIT 10;

-- NULLS FIRST AND LAST
select * from actors order by gender NULLS LAST | FIRST ;
```

**Using PostgreSQL ORDER BY clause to sort rows by multiple columns**

The following statement selects the first name and last name from the customer table and sorts the rows by the first name in ascending order and last name in descending order:

```sql
SELECT
    first_name,
    last_name
FROM
    customer
ORDER BY
    first_name ASC,
    last_name DESC;
```

| First Name | Last Name |
| :--- | :--- |
| Kelly | Torres |
| Kelly | Knott |

In this example, the ORDER BY clause sorts rows by values in the first name column first. And then it sorts the sorted rows by values in the last name column.

As you can see clearly from the output, two customers with the same first name Kelly have the last name sorted in descending order.

## DISTINCT

```sql
SELECT 
    DISTINCT state
FROM
    customers
WHERE
    country = 'USA';
```

