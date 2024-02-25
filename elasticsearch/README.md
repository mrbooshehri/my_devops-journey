# Installing elasticsearch

## Install java
```bash
yum -y install java-openjdk-devel java-openjdk
```

## Add elasticsearch repository
```bash
vi /etc/yum.repos.d/elastic.repo
```
add the following lines to the file
```bash
[elastic-7.x]
name=Elastic repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

```bash
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
yum clean all
yum makecache
yum -y install elasticsearch
```

# Open ports in firewall
```bash
firewall-cmd --add-port=9200/tcp --permanent
firewall-cmd --relaod
```

# Gather information

On dev-tools console you can issue the following commands to get more
information about your cluster

###  1. Check Cluster Health
```
GET /_cat/health?v
```
This command retrieves the health status of the cluster. The `v` parameter makes the output verbose, showing more details. The health status can be green, yellow, or red, indicating the overall health of the cluster.

###  2. Check Number of Nodes and Cluster
```
GET /_cat/nodes?v
```
This command lists all nodes in the cluster. The `v` parameter makes the output verbose, showing more details about each node, such as the node name, IP address, heap size, and more.

###  3. View Index
```
GET /_cat/indices?v
```
This command lists all indices in the cluster. The `v` parameter makes the output verbose, showing more details about each index, such as the index name, health status, number of documents, and more.

###  4. Disk and Node Status
```
GET /_cat/allocation?v
```
This command shows the disk allocation for each node in the cluster. The `v` parameter makes the output verbose, showing more details about disk allocation, such as the node name, disk size, disk used, and more.

###  5. Shard Status
```
GET /_cat/shards?v
```
This command lists all shards in the cluster. The `v` parameter makes the output verbose, showing more details about each shard, such as the index name, shard number, node name, state, and more.

###  6. Index State Names
```
GET /_cluster/allocation/explain?pretty
```
This command provides explanations for shard allocations in the cluster. The `pretty` parameter formats the output in a more readable format. It's useful for understanding why certain shards are allocated or unassigned.

###  7. RAM
```
GET /_cat/nodes?v=true&h=name,node,hea
```
This command lists nodes with specific headers (`h` parameter) showing the node name, node ID, and heap size. The `v=true` parameter makes the output verbose, but it seems there was a typo in the command. It should probably be `GET /_cat/nodes?v=true&h=name,node,heap.size` to correctly display the heap size for each node.

