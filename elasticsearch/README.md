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

# Backup snapshots in MinIO

It is possible to send Elasticsearch snapshots to single/multiple MinIO
server(s). Elasticsearch supports sending snapshots to remote
repositories, and MinIO is compatible with the S3 API, which
Elasticsearch uses for remote repositories. This means you can configure
Elasticsearch to send snapshots to multiple MinIO servers by setting up
multiple S3 repositories in Elasticsearch, each pointing to a different
MinIO server.

Here's a step-by-step guide on how to achieve this:

##  1. Set Up MinIO Servers

First, ensure you have your MinIO servers up and running. You can follow the official MinIO documentation for installation and setup instructions.

##  2. Configure Elasticsearch to Use MinIO as a Repository

Elasticsearch uses S3-compatible storage for snapshots. You need to configure Elasticsearch to use MinIO as a repository. This involves creating a repository in Elasticsearch that points to your MinIO server.

### Step  2.1: Install the Repository Plugin

Elasticsearch requires the S3 repository plugin to interact with S3-compatible storage like MinIO. If you haven't already installed it, you can do so by running:

```bash
bin/elasticsearch-plugin install repository-s3
```

### Step  2.2: Create Repositories in Elasticsearch

You need to create a repository for each MinIO server you want to use. Use the Elasticsearch API to create these repositories. Replace `minio_server_1` and `minio_server_2` with your MinIO server addresses, and adjust the `bucket` and `region` as necessary.

```bash
curl -X PUT "localhost:9200/_snapshot/minio_repo_1" -H 'Content-Type: application/json' -d'
{
  "type": "s3",
  "settings": {
    "endpoint": "http://minio_server_1:9000",
    "access_key": "YOUR_ACCESS_KEY",
    "secret_key": "YOUR_SECRET_KEY",
    "bucket": "your_bucket_name",
    "region": "us-east-1"
  }
}
'

curl -X PUT "localhost:9200/_snapshot/minio_repo_2" -H 'Content-Type: application/json' -d'
{
  "type": "s3",
  "settings": {
    "endpoint": "http://minio_server_2:9000",
    "access_key": "YOUR_ACCESS_KEY",
    "secret_key": "YOUR_SECRET_KEY",
    "bucket": "your_bucket_name",
    "region": "us-east-1"
  }
}
'
```

##  3. Create Snapshots

Now that you have your repositories set up, you can create snapshots. You can specify which repository to use when creating a snapshot.

```bash
curl -X PUT "localhost:9200/_snapshot/minio_repo_1/snapshot_1?wait_for_completion=true"
```

##  4. Restore Snapshots

To restore a snapshot, you can specify the repository from which to restore.

```bash
curl -X POST "localhost:9200/_snapshot/minio_repo_1/snapshot_1/_restore"
```

> **Note**
> 
> - Ensure that the MinIO servers are accessible from the Elasticsearch
>   cluster.
> - The `access_key` and `secret_key` should have the necessary
>   permissions to read and write to the specified bucket.
> - Adjust the `region` and `bucket` according to your MinIO server
>   configuration.
