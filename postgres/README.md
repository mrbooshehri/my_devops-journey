# Install Postgres
1. Add repository
```bash
sudo yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

1. Install postgres
```bash
yum install postgresql11-server
```
1. Initail database
```bash
/usr/pgsql-11/bin/postgresql-11-setup initdb
```
1. Enable and start db
```bash
systemctl enable --now postgresql-11.service
```
1. Change data directory [optional]
	1. Login to ```postgres``` user
	```bash
	su - postgres
	whoami
	```
	1. Enter into postgres CLI
	```bash
	psql
	```
	1. Check the data directory and quite
	```postgres
	postgres=# SHOW data_directory;
	\q
	```
	1. Note ownership and permissions of data directory files
	```bash
	ls -l /var/lib/pgsql/11/data
	```
	1. Stop the service
	```bash
	systemctl stop postgresql-11
	systemctl status postgresql-11
	```
	1. Set ownership and permission to your target directory
	```bash
	chown postgres:postgres <directory>
	chmod 700 <directory>
	```
	1. Remove old config files which you copied [in centos only]
	```bash
	rm /pgdata/data/pg_hba.conf
	rm /pgdata/data/pg_ident.conf 
	rm /pgdata/data/postgresql.auto.conf 
	rm /pgdata/data/postgresql.conf
	```
	1. Login into ```postgres``` user
	```bash
	su - postgres
	```
	1. Change ```data_directory``` in ```/var/lib/psql/11/data/postgres.conf```
	```bash
	data_directory='PATH_TO_DATA'
	```
	1. start service
	```bash
	SHOW data_directory;
	```
	1. Login into postgres and check the ```data_directory``` as you did in previous steps
	

Related
```
* https://tecadmin.net/install-postgresql-11-on-centos/
* https://thecodinginterface.com/blog/postgresql-changing-data-directory/
```
