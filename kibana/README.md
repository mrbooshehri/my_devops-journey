# Install kibana

## Install java

```bash
yum -y install java-openjdk-devel java-openjdk
```

## Add elastic repository

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

## Import GPG-key

```bash
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

## Clear yum cache
```bash
yum clean all
yum makecache
```

## Installing kibana
```bash
yum -y install kibana
```
