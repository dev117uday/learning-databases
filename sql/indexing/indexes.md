# Indexes

## B tree

* Default Index
* Self Balancing Index
* SELECT, INSERT, DELETE and sequential access  in logarithmic time
* Can be used for most of operations and column type
* supports unique condition
* Used in Primary Key
* Used with operators 
* Used when pattern matching

## Hash Index

* for equality operators
* not for range 
* Larger than btree in size

![](../../.gitbook/assets/image%20%2813%29.png)

## Brin Index

* block range index
* block data -&gt; min to max value
* smaller index
* less costly to maintain than btree index
* Can be used on very large table





