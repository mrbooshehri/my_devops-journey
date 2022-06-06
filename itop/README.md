# What is iTop?
iTop, stands for IT Operational Portal, is an Open Source web based application for the day to day operations of an IT environment. iTop was designed with the ITIL best practices in mind but does not dictate any specific process, the application is flexible enough to adapt to your processes whether you want rather informal and pragmatic processes or a strict ITIL aligned behaviour.

This tool is ideal for Help Desk agents, Support engineers, Service managers, IT managers and End-users. Hence it is based on Apache, MySQL and PHP, so you can run it on any operating system that supports those applications like Windows, Linux, MacOS and Solaris as well.

Using iTop you can:

* Document your entire IT infrastructure assets such as servers, applications, network devices, virtual machines, contacts.. etc.
* Manage incidents, user requests, planned outages.
* Document IT services and contracts with external providers including service level agreements.
* Export all the information in a manual or scripted manner.
* Import or synchronize/federate any data from external systems.

# Hardware & Software requirements

Minimum Hardware requirements
|Ticket |created per month |	Console Users 	|CMDB: CIs 	|Servers 	|CPU |	Memory 	Disk for MySQL|
|:----- |:-----|:-----|:-----|:-----|:-----|:-----|
|< 200 	|< 20 	|< 50k 	|An all in one server 	|2vCPU 	4Gb 	|10Gb|
|< 5000 	|< 50 	|< 200k| 	Two servers: Web + MySQL |	4vCPU 	|8Gb |	20Gb|
|> 5000 	|> 50 |	> 200k |	Two servers: Web + MySQL 	|8vCPU 	|16Gb 	|50Gb|

|itop| PHP| 	MySQL| 	MariaDB|
|:-----|:-----|:-----|:-----|
|3.x.x+ |7.4 → 8.2 ? |	5.7+| 	10.3+|


## Install httpd
```bash
yum install -y httpd
```

## Install MariaDB
### Add MariaDB Yum Repository
First you should add the repository under
```/etc/yum.repos.d/MariaDB.repo```

```bash
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.6/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```
### Install MariaDB in CentOS 7
```bash
yum install MariaDB-server MariaDB-client -y
```
### Start and enable the MariaDB service
```bash
systemctl start mariadb
systemctl enable mariadb
systemctl status mariadb
```
### Secure MariaDB in CentOS 7
```bash
mysql_secure_installation
# or mariadb_secure_installation
```

```bash
yum install httpd
yum install mysql mysql-server
yum install php php-mysql php-xml php-cli php-soap php-ldap php-gd php-zip php-json php-mbstring graphviz
```
## Install PHP 8
### Add repository
```bash
yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum -y install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
yum -y install yum-utils
yum-config-manager --disable 'remi-php*'
yum-config-manager --enable remi-php80
```

### Install Packages
```bash
yum install php php-mysql php-xml php-cli php-soap php-ldap php-gd php-zip php-json php-mbstring graphviz
```
# Software configuration

## Write permission on temp directory
iTop needs write access to the temp dir (this path is retrieved using
the PHP function ```sys_get_temp_dir()```). Check rights and also the
```openbase_dir``` PHP parameter !

## PHP
Recommanded valuse for ```php.ini```
```php
memory_limit = 256M ; could be increased if needed
max_input_vars = 5000
 
; upload_tmp_dir : should point to a directory with write access
 
; also check those options for attachments (se dedicated chapter below)
; adapt values depending of your preferences!
; - upload_max_filesize
; - max_file_uploads
; - post_max_size
; - max_input_time
```
**Note:** If you're using CLI tools like cron.php, check also the PHP instance used for CLI !



# Tuning iTop performance
Head over [here](https://www.itophub.io/wiki/page?id=3_0_0%3Aadmin%3Aperformance)
