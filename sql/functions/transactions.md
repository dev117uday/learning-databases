# Transactions

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
		
		SAVEPOINT first_update;
		
		UPDATE t_accounts
		SET balance = balance + amount
		WHERE recid = receiver;
		
		ROLLBACK first_update;
		
		COMMIT;
	
	END;
$$
LANGUAGE PLPGSQL;
```

