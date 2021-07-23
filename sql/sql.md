# SQL

## Lifetime of Query

* **Parser** : handles the textual form of the statement and verifies whether it is correct or not
* **Re-writer** : applying the syntactic rules to rewrite the original SQL statement.
* **Optimiser** : finding the fastest path to the data
* **Executor** : responsible for effectively going to the storage  and retrieving the data from the physical storage.

### Optimiser 

* Finds all the paths and gets the path with cheapest COST
* **LOWEST COST WINS !!**

## Nodes

* Nodes are available for :
  * every operation
  * every access methods
* Nodes are stack-able
  * Parent Node \( cost = 0.00 ... \)
    * Child Node
      * Child Node
* Types of Nodes

  * Sequential Scan
  * Index Scan, Index Only Scan, Bitmap Index Scan
  * Nested Loop, Hash Join and Merge Join 
  * Gather and Merge parallel nodes

  **Get All Node Types** : `SELECT * FROM pg_am;`

### Sequential Scan

Performs a sequential scan on the whole table.

![](../.gitbook/assets/image%20%2811%29.png)

## Index Nodes

* Index is used to access Data
* Types
  * Index scan
  * Index only scan
  * Bitmap Index

#### Index Scan

![](../.gitbook/assets/image%20%289%29.png)

#### Index only scan

![](../.gitbook/assets/image%20%2815%29.png)









