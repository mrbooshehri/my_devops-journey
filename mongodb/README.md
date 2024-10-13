# Install MongoDB Standalone

## Add repo for version 5
```bash
sudo tee /etc/yum.repos.d/mongodb-org-5.0.repo<<EOF
[mongodb-org-5.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/\$releasever/mongodb-org/5.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-5.0.asc
EOF
```
**Note:** Please consider mongodb version 5 and higher relies on ```avx``` 
CPU feature so make sure your CPU has that feature before
installing version 5 or higher, to check:

```bash
grep avx /proc/CPUinfo
```

## Add repo for version 4.4
```bash
sudo tee /etc/yum.repos.d/mongodb-org-4.4.repo<<EOF
[mongodb-org-4.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/7Server/mongodb-org/4.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc
EOF
```

## Install MongoDB
```bash
yum install mongodb-org
```

## Remove MongoDB
```bash
yum erase $(rpm -qa | grep mongodb-org)
```

## Enable and start service

```bsah
systemctl enable --now monogd
```

# What is a MongoDB Cluster?
In MongoDB, clusters can refer to two different architectures. They can either mean a replica set or a sharded cluster. 

## Replica Sets
A MongoDB replica set is a group of one or more servers containing the
exact copy of the data. While it's technically possible to have one or
two nodes, the recommended minimum is three. A primary node is
responsible for providing your application's read and write operations,
while two secondary nodes contain a replica of the data. 

![replicaset](./assets/replicaset-1.png)

If the primary node becomes unavailable a new primary node would be picked by an
election process. The new primary node is now responsible for the read
and write operations. 

![replicaset](./assets/replicaset-2.png)

Once the faulty server comes back online, it will sync up with the
primary node and become a new secondary node in the cluster.

![replicaset](./assets/replicaset-3.png)

The goal is to provide your application with high availability over your
data. Even in a server failure, your client application can still
connect to the cluster and access the data, reducing the overall
potential downtime.

## Sharded Clusters
A sharded cluster is a way to scale horizontally by distributing your
data across multiple replica sets. When a read or write operation is
performed on a collection, the client sends the request to a router
(mongos). The router will then validate which shard the data is stored
in via the configuration server and send the requests to the specific
cluster.

![shard](assets/shard-1.png)

Each of the shards would contain its own replica set. You should also
have more than one router or configuration server to ensure high
availability. With this type of architecture, you can scale your
database as much as you want without compromising availability or
worrying about storage capacity.

# Deploy a Replica Set

Replica sets should always have an odd number of members. This ensures
that elections will proceed smoothly.

**Tip**
> When possible, use a logical DNS hostname instead of an IP address,
particularly when configuring replica set members or sharded cluster
members. The use of logical DNS hostnames avoids configuration changes
due to IP address changes.

## Network Considerations

* Establish a virtual private network. Ensure that your network topology
	routes all traffic between members within a single site over the local
	area network.
* Configure access control to prevent connections from unknown clients
	to the replica set. 
* Configure networking and firewall rules so that incoming and outgoing
	packets are permitted only on the default MongoDB port and only from
	within your deployment. See the IP Binding considerations.
* Ensure that each member of a replica set is accessible by way of
	resolvable DNS or hostnames. You should either configure your DNS
	names appropriately or set up your systems' ```/etc/hosts``` 
	file to reflect this configuration.

## Configuration

Create the directory where MongoDB stores data files before deploying MongoDB.
Specify the mongod configuration in a configuration file stored in ```/etc/mongod.conf```
 or a related location.

## Procedure
The following procedure outlines the steps to deploy a replica set when access control is disabled.
1. Start each member of the replica set with the appropriate options.

For each member, start a mongod instance with the following settings:

* Set replication.replSetName option to the replica set name. If your application connects to more than one replica set, each set must have a distinct name.
* Set net.bindIp option to the hostname/IP or a comma-delimited list of hostnames/IPs.
* Set any other settings as appropriate for your deployment.


Related:
```
* https://www.itzgeek.com/how-tos/linux/centos-how-tos/how-to-install-latest-mongodb-2-6-4-on-centos-7-rhel-7.html
* https://www.mongodb.com/basics/clusters/mongodb-cluster-setup
* https://www.mongodb.com/docs/v4.4/tutorial/convert-standalone-to-replica-set/
* https://www.mongodb.com/docs/manual/tutorial/convert-standalone-to-replica-set/
* https://www.mongodb.com/docs/manual/tutorial/deploy-shard-cluster/
* https://www.mongodb.com/docs/v4.4/tutorial/deploy-shard-cluster/
* https://www.mongodb.com/docs/v4.4/tutorial/convert-shard-standalone-to-shard-replica-set/
* https://www.mongodb.com/docs/v4.4/tutorial/deploy-replica-set/
```
# MongoDB Cluster-to-Cluster Sync

MongoDB Cluster-to-Cluster Sync is a method used for data
synchronization between two MongoDB clusters. Two different clusters can
be continuously kept in sync, and this synchronization can be stopped
and triggered again whenever needed. In this article, I will try to
explain how to synchronize MongoDB clusters installed on two different
distributions.

Let’s designate our source and destination clusters as follows:

* Source:
```
mongo-test-centos-01
mongo-test-centos-02
mongo-test-centos-03
```

* Destination:
```
mongo-test-ubuntu-01
mongo-test-ubuntu-02
mongo-test-ubuntu-03
```

1. MongoDB Cluster-to-Cluster Sync Download

You can download mongosync from the link below; it is sufficient to
download it to the source server.

```bash
wget https://fastdl.mongodb.org/tools/mongosync/mongosync-rhel70-x86_64-1.7.1.tgz
After the download process, you can extract the compressed file.
tar -zxvf mongosync-*.tgz
To add the mongosync binary file to your Path, you can use one of the following commands.
sudo cp /root/mongosync-rhel70-x86_64-1.7.1/bin/mongosync /usr/local/bin/
sudo ln -s /root/mongosync-rhel70-x86_64-1.7.1/bin/mongosync /usr/local/bin/mongosync
```

2. Creating a New User

Root role users must be created for both clusters. These users will
facilitate communication and data transfer between the source and
destination clusters.


Source:
```
db.createUser({user: "clusteradmin", pwd: "password1", roles: [{role: "root", db: "admin"}]});
```

Destination:

```
db.createUser({user: "clusteradmin", pwd: "password2", roles: [{role: "root", db: "admin"}]});
```

3. Creating the Mongosync Configuration File

We can create the configuration file under etc as mongosync.conf. While
cluster0 points to the source cluster, cluster1 points to the
destination cluster.

```bash
vi /etc/mongosync.conf
```

```bash
cluster0: "mongodb://clusteradmin:password1@mongo-test-centos-01:27127,mongo-test-centos-02:27127,mongo-test-centos-03:27127"
cluster1: "mongodb://clusteradmin:password2@mongo-ubuntu-test-01:27127,mongo-ubuntu-test-02:27127,mongo-ubuntu-test-03:27127"
logPath: "/var/log/mongosync"
verbosity: "WARN"
```

To check if the configuration is successful, you can run the following command.

```bash
cd /usr/local/bin/ 
nohup mongosync - config /etc/mongosync.conf >> mongosync1.log &
```

4. Initiating the Synchronization Process

The following command starts the synchronization, and data flow from the
source cluster to the destination cluster begins. Unless manually
intervened, synchronization will continue continuously. If needed, data
flow can be paused and canceled.

```bash
curl localhost:27182/api/v1/start -XPOST \
--data '
   {
      "source": "cluster0",
      "destination": "cluster1",
      "reversible": true,
      "enableUserWriteBlocking": true
   } '
```

Note: The “enableUserWriteBlocking” parameter being set to true
indicates that a user other than the clusteradmin cannot input data into
the destination cluster.

Different situations, such as progress, pause, resume, and commit, are available for use when needed.

```bash
curl localhost:27182/api/v1/progress -XGET
curl localhost:27182/api/v1/pause -XPOST --data '{ }'
curl localhost:27182/api/v1/resume -XPOST --data '{ }'
curl localhost:27182/api/v1/commit -XPOST --data '{ }'
```
