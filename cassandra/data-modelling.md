# Data Modelling

In Scylla, as opposed to relational databases, the data model is based around the queries and not just around the domain entities. When creating the data model, we take into account both the conceptual data model and the application workflow: which queries will be performed by which users and how often.

### Things to keep in mind when design Tables

* **Even data distribution**: data should be evenly spread across the cluster so that every **node** holds roughly the same amount of data. Scylla determines which node should store the data based on hashing the partition key. Therefore, choosing a suitable partition key is crucial. More on this later on.
* **To minimize the number of partitions accessed in a read query**: To make reads faster, we’d ideally have all the data required in a read query stored in a single Table. Although it’s fine to duplicate data across tables, in terms of performance, it’s better if the data needed for a read query is in one table.

### Things we should **NOT** focus on:

* **Avoiding data duplication**: To get efficient reads, we sometimes have to duplicate data.&#x20;
* **Minimizing the number of writes**: writes in Scylla DB aren’t free, but they are very efficient and “cheap.” Scylla DB is optimized for high write throughput. Reads, while still very fast, are usually more expensive than writes and are harder to fine-tune.&#x20;
  * We’d usually be ready to increase the number of writes to increase read efficiency. Keep in mind that the number of tables also affects consistency.&#x20;

