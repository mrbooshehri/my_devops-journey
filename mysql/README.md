# Install mysql 5.7
1. Change your dns to [shecan](https://shecan.ir)
```bash
vi /etc/resolve.conf
```
add one of the addresses at the begining the file
```bash
namseserver 185.51.200.2
namseserver 178.22.122.100
```
1. Add repository
```bash
yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
```
1. Insatll mysql
```bash
yum install mysql-community-server
```

# Upgrade mysql 5.7 to 8.0

There are two different ways to update mysql:
1. INPLACE 
1. LOGICAL

## INPLACE
The INPLACE upgrade involves shutting down the MySQL 5.7 server, replacing the
old binaries with MySQL 8.0 binaries and then starting the MySQL 8.0 server on
the old data directory.  

## LOGICAL
The LOGICAL upgrade involves exporting SQL from the
MySQL 5.7 version using a backup or export utility such as mysqldump or
mysqlpump, installing the MySQL 8.0 binaries, and then applying the SQL to the
new MySQL version.

## Pros and cons
The INPLACE upgrade is faster than the LOGICAL upgrade since it does not
require loading of the databases after installing MySQL 8.0 version. Also while
loading the databases during LOGICAL upgrade, errors might be encountered due
to the incompatibilities which would require modifying the exported SQL file.

## How to upgrade

1. Download `mysqlsh` form [here](https://dev.mysql.com/downloads/shell/) and
   check if you are eligible to upgrade or not
	1. Install `mysqlsh`
	```bash
	dpkg -i mysql-shell_8.0.31-1ubuntu18.04_amd64.deb
	```
	1. Check status
	```bash
	mysqlsh root:@localhost:3306 -e "util.checkForServerUpgrade();"
	```
	> **Note:** If you faced with permission denied error, check root user login method
	```bsah
	mysql -u root

	mysql> USE mysql;
	mysql> SELECT User, Host, plugin FROM mysql.user;
	mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';
	mysql> FLUSH PRIVILEGES;
	mysql> exit;

	service mysql restart
	```
	For more information check [here](https://stackoverflow.com/questions/39281594/error-1698-28000-access-denied-for-user-rootlocalhost)

2. Install `mysqldump` and make a backup
	1. Add mysql repository
	```bash
	wget https://dev.mysql.com/downloads/repo/apt/
	dpkg -i mysql-apt-config_0.8.24-1_all.deb
	apt update
	apt install mysql-client
	```
3. 
