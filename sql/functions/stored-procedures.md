# Stored Procedures

* They are compiled objects

```sql
CREATE TABLE t_accounts (
    recid SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    balance dec(15,2) NOT NULL
);

drop table t_accounts;

INSERT INTO t_accounts (name,balance) values ('Adam',100),('Linda',100);

select * from t_accounts;

CREATE OR REPLACE PROCEDURE pr_money_transfer (sender int, receiver int, amount dec)
AS
$$

    BEGIN

        UPDATE t_accounts
        SET balance = balance - amount
        WHERE recid = sender;

        UPDATE t_accounts
        SET balance = balance + amount
        WHERE recid = receiver;

        COMMIT;

    END;
$$
LANGUAGE PLPGSQL;

CALL pr_money_transfer(1,2,30);
select * from t_accounts;

CREATE OR REPLACE PROCEDURE pr_orders_count(INOUT total_count INTEGER DEFAULT 0 ) AS
$$
    BEGIN

        SELECT COUNT(*)    INTO total_count FROM orders;

    END;
$$ LANGUAGE PLPGSQL;

CALL pr_orders_count();
```

