# Mongo Administration

### dbpath

The dbpath is the directory where all the data files for your database are stored. The dbpath also contains journaling logs to provide durability in case of a crash. As we saw before, the default dbpath is /data/db; however, you can specify any directory that exists on your machine. The directory must have read/write permissions since database and journaling files will be written to the directory.

### Port

The port option3XhfaAvFFNUkRy2c allows us to specify the port on which mongod will listen for client connections. If we don't specify a port, it will default to 27017. Database clients should specify the same port to connect to mongod.

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
processManagement:
  fork: true
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
processManagement:
  fork: true
replication:
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
processManagement:
  fork: true
replication:
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
processManagement:
  fork: true
replication:
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
processManagement:
  fork: true
replication:
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

rs.add("localhost:27012")
rs.add("localhost:27013")
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

