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
