# Mongo Administration

### dbpath

The dbpath is the directory where all the data files for your database are stored. The dbpath also contains journaling logs to provide durability in case of a crash. As we saw before, the default dbpath is /data/db; however, you can specify any directory that exists on your machine. The directory must have read/write permissions since database and journaling files will be written to the directory.

### Port

The port option allows us to specify the port on which mongod will listen for client connections. If we don't specify a port, it will default to 27017. Database clients should specify the same port to connect to mongod.

### Auth

auth enables authentication to control which users can access the database. When auth is specified, all database clients who want to connect to mongod first need to authenticate.

Before any database users have been configured, a Mongo shell running on localhost will have access to the database. We can then configure users and their permission levels using the shell. Once one or more users have been configured, the shell will no longer have default access

### bind\_ip

The bind\_ip option allows us to specify which IP addresses mongod should bind to. When mongod binds to an IP address, clients from that address are able to connect to mongod

Sample Config File

```yaml
storage:
  dbPath: "/data/db"
systemLog:
  path: "/data/log/mongod.log"
  destination: "file"
replication:
  replSetName: M103
net:
  bindIp : "127.0.0.1,192.168.103.100"
tls:
  mode: "requireTLS"
  certificateKeyFile: "/etc/tls/tls.pem"
  CAFile: "/etc/tls/TLSCA.pem"
security:
  keyFile: "/data/keyfile"
tim
```

## User management commands

```bash
db.createUser()
db.dropUser()

# Collection management commands:

db.<collection>.renameCollection()
db.<collection>.createIndex()
db.<collection>.drop()

# Database management commands:

db.dropDatabase()
db.createCollection()

# Database status command:

db.serverStatus()

# Creating index with Database Command:

db.runCommand(
  { "createIndexes": <collection> },
  { "indexes": [
    {
      "key": { "product": 1 }
    },
    { "name": "name_index" }
    ]
  }
)

# Creating index with Shell Helper:

db.<collection>.createIndex(
  { "product": 1 },
  { "name": "name_index" }
)

# Introspect a Shell Helper:

db.<collection>.createIndex
```

## File Structure

```bash
# List --dbpath directory:
ls -l /data/db

# List diagnostics data directory:
ls -l /data/db/diagnostic.data

# List journal directory:
ls -l /data/db/journal

# List socket file:
ls /tmp/mongodb-27017.sock
```

## Create new user

```bash
use admin

db.createUser({
  user: "root",
  pwd: "root123",
  roles : [ "root" ]
})
```

### Create security officer

```bash
db.createUser(

  { user: "security_officer",
    pwd: "h3ll0th3r3",
    roles: [ { db: "admin", role: "userAdmin" } ]
  }
)

# Create database administrator:

db.createUser(

  { user: "dba",
    pwd: "c1lynd3rs",
    roles: [ { db: "admin", role: "dbAdmin" } ]
  }
)

# Grant role to user:

db.grantRolesToUser( "dba",  [ { db: "playground", role: "dbOwner"  } ] )

# Show role privileges

db.runCommand( { rolesInfo: { role: "dbOwner", db: "playground" }, showP
```

### Setting Up a Replica Set

The configuration file for the first node \(node1.conf\):

```text
storage:
  dbPath: /var/mongodb/db/node[1,2,3]
net:
  bindIp: 192.168.103.100,localhost
  port: 2701[1,2,3]
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node[1,2,3]/mongod.log
  logAppend: true
timreplication:
  replSetName: m103-example
```

```bash
mkdir -p /var/mongodb/db/{1,2,3}
sudo mkdir -p /var/mongodb/pki/
openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
chmod 400 /var/mongodb/pki/m103-keyfile
```

```bash
storage:
  dbPath: /var/mongodb/db/1
net:
  bindIp: localhost
  port: 27001
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/logs/mongod1.log
  logAppend: true
timreplication:
  replSetName: m103-repl

storage:
  dbPath: /var/mongodb/db/2
net:
  bindIp: localhost
  port: 27002
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/logs/mongod2.log
  logAppend: true
timreplication:
  replSetName: m103-repl

storage:
  dbPath: /var/mongodb/db/3
net:
  bindIp: localhost
  port: 27003
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/logs/mongod3.log
  logAppend: true
timreplication:
  replSetName: m103-repl
```

```bash
mongo --port 27001

rs.initiate()

use admin

db.createUser({
  user: "m103-admin",
  pwd: "m103-pass",
  roles: [
    {role: "root", db: "admin"}
  ]
})

# then exit

mongo --host "localhost:27001" -u "m103-admin"
-p "m103-pass" --authenticationDatabase "admin"

rs.status()

rs.add("localhost:27002")
rs.add("localhost:27003")
```

### Basic Replication functions

```bash
rs.add
rs.initiate
rs.remove
rs.config
rs.reconfig
rs.isMaster
rs.printReplicationInfo
```

`rs.status` :

* reports health on replica set nodes
* uses data from heartbeats

`rs.isMaster` :

* descibe's a node's role in a replica set

`rs.printReplicationInfo` :

* only returns oplogs data relative to current node
* contains timestamps for first and oplog events

`oplog.rs` :

* central point of replication
* keeps track of all statements getting replicated
* its is a capped collection : size of collection is limited 
* by defualt, it takes 5% of the available disk
* it appends statements \( used in replication \), till the file cap is reached
* once full, it starts to over write operations from top
* replication windows is proportional to the  system load
* size of oplog.rs will determine how much time a secondary node has to join in before it thorws itself in recovery mode

```bash
# Display collections from the local database (this displays more collections from a replica set than from a standalone node):

use local
show collections

# Query the oplog after connected to a replica set:

use local
db.oplog.rs.find()

# Get information about the oplog (remember the oplog is a capped collection).
# Store oplog stats as a variable called stats:

var stats = db.oplog.rs.stats()

# Verify that this collection is capped (it will grow to a pre-configured size before it starts to overwrite the oldest entries with newer ones):

stats.capped

# Get current size of the oplog:

stats.size

# Get size limit of the oplog:

stats.maxSize

# Get current oplog data (including first and last event times, and configured oplog size):

rs.printReplicationInfo()
```

## Failover :

* Primary node is the first point of contact from client
* we first upgrade the secondary nodes
* then we step down the primary node to become secondary \( using rs.stepDown\(\) \), once the election is complete, we can then safely upgrade the primary \(which is now secondary\) and connect it back to the replica set

Elections :

* Happens when the primary node becomes unavailable or primary node wants to step down
* Next primary node will be elected keeping the following things in mind :
  * Which ever node has the latest copy of data, it will run for election and automatically vote for itself
  * If two node \( in a cluster of 3\) has same recent data, the third node will cast a vote for any one of them to become primary. This becomes a problem in even node replica set
* priority : likelihood that a node will become primary in case of election
  * default priority is 1
  * priority 1 or higher will give the node a higher chance of winning the election
  * set priority of node to 0 if we dont want node to become primary
  * node that cannot become primary are known as passive nodes

## write concerns

* Commands under write concern
  * insert
  * update
  * delete
  * find or modify
* ACK mechanism added to write ops to provide stronger durability garuntee
* MAX : majority of nodes : roundup\(\[num of nodes\]/2\)
* more durability requires more time to achieve
* write concerns level
  * 0 : dont wait for ack
  * 1 : wait for primary to ack
  * > =2 : wait for primary and one or more secondary to ack
  * "majority" : wait for majority to ack
* write concern options
  * wtimeout : time to wait before marking operation as failed
  * j \[true\|false\] : requires node to commit  the write operation to the journal before returning the ack
* setting write concern higher make ops slower

## read concern / preference

* specifies durability during a read operation
* read concern level
  * local : returns from the primary node
  * available 
  * majority 
* read preference allows you to redirect read operation to specific members of replica set
* read preference may return stale data 
* read preference modes
  * primary : default
  * primaryPreferred : can route read ops to secondary in case primary in not available
  * secondary : routes read ops to sec. nodes only
  * secondaryPreferred : if sec. not available, routes read ops to primary
  * nearest : routes to least network latency from the host, ignores primary or secondary

## Sharding

* There is upper limit to vertical scaling
* Sharding means adding more machines and dividing the dataset into multiple pieces
* For each shard, we add more replica to make sure we dont lose data
* a sharded cluster contains a config server, that store the metadata about each shard. This config server are responsible to distributing the queries to the shard containing the data. 
* to make config servers highly available, they are deployed in a replica set configuration
* We use mongos to route the queries to each shard

## When to shard a database

* if not economically viable to scale up the throughput, speed and volume
* scaling horizontally will add more cost to backup, restore and initial sync time, when not feasible, shard the database
* Max a server should contain 2tb to 5tb data \(factor in CPU and RAM usage\)
* when geographically distributed database is required
* when single threaded operation needs to be parallelised

## Sharding Architecture

* client do not connect to sharded cluster directly
* they connect to a process called mongos that routes queries to shards

### How mongos figures out where to route the queries

Let's say we have 3 shard containing the data about football players

* shard 1 : A-J
* shard 2 : K-Q
* shard 3 : R-Z

When you send a query to find details about player name Messi, it will know which shard contains the data about that player \( as all shards store data about specific player only \). The config server will contain the metadata, for example : shard 1 contains names from A-J, shard 2 contains names from K-Q, shard 3 contains name from R-Z which helps mongos the route the queries

There can be one mongos process routing queries to 3 shards or there can be multiple mongos process routing queries to shards and takign request also from multiple clients

If size of one shard grows more than other, the data needs to be moved from the bigger shard to the smaller ones to keep consist storage capacity. This update will also be reflected in config server.

As not all collections in a database needs to be sharded, there is one shard in the cluster that will act as primary shard keeping all the non sharded collections

If we query the database, lets say where age is in 28-30, then mongos will not be able to route the query to specific shard, rather it will send it to all shards to find out the data, then SHARD\_MERGE stage takes place. This stage can take place on mongos or a randomly chosen shard in the cluster.

Setting up a sharded cluster

```bash
sharding:
  clusterRole: configsvr
replication:
  replSetName: m103-csrs
security:
  keyFile: /var/mongodb/pki/m103-keyfile
net:
  bindIp: localhost,192.168.103.100
  port: 27004
systemLog:
  destination: file
  path: /var/mongodb/db/csrs1.log
  logAppend: true
storage:
  dbPath: /var/mongodb/db/csrs1

sharding:
  clusterRole: configsvr
replication:
  replSetName: m103-csrs
security:
  keyFile: /var/mongodb/pki/m103-keyfile
net:
  bindIp: localhost,192.168.103.100
  port: 27005
systemLog:
  destination: file
  path: /var/mongodb/db/csrs2.log
  logAppend: true
storage:
  dbPath: /var/mongodb/db/csrs2

sharding:
  clusterRole: configsvr
replication:
  replSetName: m103-csrs
security:
  keyFile: /var/mongodb/pki/m103-keyfile
net:
  bindIp: localhost,192.168.103.100
  port: 27006
systemLog:
  destination: file
  path: /var/mongodb/db/csrs3.log
  logAppend: true
storage:
  dbPath: /var/mongodb/db/csrs3

# start this servers using
mongod -f csrs1,2,3.conf
```

* Connect to primary node of config server replica set \(csrs\)

```bash
mongo --port 27004 --username m103-admin --password m103-pass --authenticationDatabase admin

# # initiate replica set
# rs.initiate()

# # create super user
# db.createUser({
#   user: "m103-admin",
#   pwd: "m103-pass",
#   roles: [
#     {role: "root", db: "admin"}
#   ]
# })

# db.auth("m103-admin","m103-pass")

# # add replica set node 
# rs.add("192.168.103.100:27002")
# rs.add("192.168.103.100:26003")

# verify the replica is set up properly
rs.isMaster()
```

* Config File Mongos

```bash
sharding:
  configDB: m103-csrs/192.168.103.100:27004,192.168.103.100:27005,192.168.103.100:27006
security:
  keyFile: /var/mongodb/pki/m103-keyfile
net:
  bindIp: localhost,192.168.103.100
  port: 26000
systemLog:
  destination: file
  path: /var/mongodb/db/mongos.log
  logAppend: true
```

* start mongos using : `mongos -f mongos.conf`
* connect to mongos

  \`\`\`sh

  mongo --port 26000 --username m103-admin --password m103-pass --authenticationDatabase admin

## check sharding status

sh.status\(\)

```text
```sh
sharding:
  clusterRole: shardsvr
storage:
  dbPath: /var/mongodb/db/node1
  wiredTiger:
    engineConfig:
      cacheSizeGB: .1
net:
  bindIp: 192.168.103.100,localhost
  port: 27011
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node1/mongod.log
  logAppend: true
timreplication:
  replSetName: m103-repl

sharding:
  clusterRole: shardsvr
storage:
  dbPath: /var/mongodb/db/node2
  wiredTiger:
    engineConfig:
      cacheSizeGB: .1
net:
  bindIp: 192.168.103.100,localhost
  port: 27012
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node2/mongod.log
  logAppend: true
timreplication:
  replSetName: m103-repl

sharding:
  clusterRole: shardsvr
storage:
  dbPath: /var/mongodb/db/node3
  wiredTiger:
    engineConfig:
      cacheSizeGB: .1
net:
  bindIp: 192.168.103.100,localhost
  port: 27013
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node3/mongod.log
  logAppend: true
timreplication:
  replSetName: m103-repl
```

* Apply the new config to the secondary nodes and then connect to primary, preform force stepdown, and reload with new configuration
* then connect to mongos and add the primary node of the the shard

```bash
sh.addShard("shard1/localhost:27001")
```

### The Config database

* maintained and used internally by mongodb, dont touch it i not necessary

**Switch to config DB:**

```bash
use config

# Query config.databases:
db.databases.find().pretty()

# Query config.collections:
db.collections.find().pretty()

# Query config.shards:
db.shards.find().pretty()

# Query config.chunks:
db.chunks.find().pretty()

# Query config.mongos:
db.mongos.find().pretty()
```

### Shard Key

* It is the indexed field that mongodb uses to partition data in a sharded collection and distribute it across the shards in your cluster
* You need to create index first before you can select your shard key.
* MongoDB uses these shard keys to distribute data across sharded clusters. This groupings are also known as chunks
* shard key should be present in every document in the collection \(if not already\) or in every new document that is inserted
* shard keys are immutable, cannot change shard key post-sharding
* you cannot change the values of shard key fields post-sharding
* sharded collections are irreversible, you cannot unshard a collection, once sharded.

### How to shard

* use `sh.enableSharding("database")` to enable sharding for the specific database.
* use `db.collections.createIndex()` to create the index for your shard key field
* use `sh.shardCollections("<database>.<collections>",{shard_key})` to shard the collection

### Picking a Good shard key

* Cardinality 
  * High Cardinality = many possible unique shard key values.
  * Low Frequency  = low repetition of a given unique shard key value.
  * Avoid shard keys that changes monotonically \(keeping incrementing\), choosing `_id` or `timestamp` or not a great options.

### Hashed Shard Keys

* shard key where the underlying index is hashed
* mongodb uses a hashing function to calculate the hash shard key and then you out where the data is located
* the data is not changed in the docuement, instead the underlying index backing the shard key itself is hashed
* As monotonically changind value like `_id` or `timestamp` can be hashed, because the output from the hash function can prevent hotspotting
* this can make data highly distribute, so in case where you need
* you cannot support geographically isolated read operations using zoned sharding
* use `sh.enableSharding("database")` to enable sharding for the specified
* use `db.collection.createIndex({"field":"hashed"})` to create the index for your shard key field
* use `sh.shardCollection("<database>.<collection>",{ shard_key : "hashed" })` to shard the collection

### Lab shard a collection

```bash
mongoimport --drop /dataset/products.json --port 26000 -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin" --db m103 --collection products

mongo --port 26000 --username m103-admin --password m103-pass --authenticationDatabase admin

use m103

sh.enableSharding("m103")
db.products.createIndex({"sku":1})
sh.shardCollection("m103.products",{ "sku" : 1 })
```

### Chunks

* group of documents, who information is store in mongos determining which data belongs to which chunk and which shard contains it
* rebalancing of chunks is preformed by primary of config server replica set
* Default Chunk Size : 64Mb

  ```bash
  use config
  db.settings.save({_id: "chunksize", value: 2})
  ```

lab : copy sample data

```bash
mongoimport /dataset/products.part2.json --port 26000 -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin" --db m103 --collection products
```

Targeted Queries vs Scatter Gather

* Each Shard contains chunks of sharded data, where each chunk represents a inclusive lower bound and upper bound. 
* The config server replica set keeps maintains the primary record of where all the chunks are present.
* Mongos keeps a cached copy of the data chunks
* If the query contains the shard key, then mongos knows where to target the query. This is known as targeted query.
* targeted query are much faster and always used in query data request preformed by the customer.
* if the shard key isn't present in the query, them mongos will send the query to all shards and merge back the results from each shard. this is known as scatter gather.
* scatter gather query sometimes could be extremely slow, hence on admins performing analytics queries should be allowed to run them.

#### In case of composite key

example shard key : `{"sku":1,"type":1,"name":1}`

**Valid Targeted Queries**

```bash
db.products.find( { "sku": ... } )
db.products.find( { "sku": ... , "type": ... } )
db.products.find( { "sku": ... , "type": ... , "name": ... } )
```

**Scatter Gather**

```bash
db.products.find( { "type": ... } )
db.products.find( { "name": ... } )
```

