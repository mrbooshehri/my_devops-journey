# Installation PostgreSQL

## CentOS/RHEL

1. Add repository
```bash
sudo yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

2. Install PostgreSQL
```bash
yum install postgresql11-server
```
3. Initail database
```bash
/usr/pgsql-11/bin/postgresql-11-setup initdb
```
4. Enable and start db
```bash
systemctl enable --now postgresql-11.service
```
5. Change data directory [optional]
	1. Login to ```postgres``` user
	```bash
	su - postgres
	whoami
	```
	2. Enter into PostgreSQL CLI
	```bash
	psql
	```
	3. Check the data directory and quite
	```postgres
	postgres=# SHOW data_directory;
	\q
	```
	4. Note ownership and permissions of data directory files
	```bash
	ls -l /var/lib/pgsql/11/data
	```
	5. Stop the service
	```bash
	systemctl stop postgresql-11
	systemctl status postgresql-11
	```
	6. Set ownership and permission to your target directory
	```bash
	chown postgres:postgres <directory>
	chmod 700 <directory>
	```
	7. Remove old config files which you copied [in centos only]
	```bash
	rm /pgdata/data/pg_hba.conf
	rm /pgdata/data/pg_ident.conf 
	rm /pgdata/data/postgresql.auto.conf 
	rm /pgdata/data/postgresql.conf
	```
	8. Login into ```postgres``` user
	```bash
	su - postgres
	```
	9. Change ```data_directory``` in ```/var/lib/psql/11/data/postgres.conf```
	```bash
	data_directory='PATH_TO_DATA'
	```
	10. start service
	```bash
	SHOW data_directory;
	```
	11. Login into postgres and check the ```data_directory``` as you did in previous steps
	
## Ubuntu/Debian

1. Check requirements and update system

```bash
sudo apt update && sudo apt upgrade && sudo apt -y install gnupg2 wget vim
```

2. Add the repository
```bash

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```
3. Import the GPG signing key for the repository
```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

4. Update your APT package list
```bash
sudo apt -y update
```

5. Install PostgreSQL
```bash
sudo apt -y install postgresql
```

#  Connect to PostgreSQL

## Option 1

Running the PostgreSQL command directly with sudo.

```bash
$ sudo -u postgres psql
psql (14.0 (Ubuntu 14.0-1.pgdg18.04+1))
Type "help" for help.

postgres=#
```

## Option 2

For this option, you first have to switch to the PostgreSQL user created after PostgreSQL  installation.

```bash
sudo -i -u postgres
```

# Basic usages

1. **Connect to PostgreSQL:**
   - Access the PostgreSQL command-line interface:
     ```bash
     psql -U postgres
     ```

2. **Create a Database:**
   ```sql
   CREATE DATABASE your_database_name;
   ```

3. **Create a User:**
   ```sql
   CREATE USER your_username WITH PASSWORD 'your_password';
   ```

4. **Grant Permissions:**
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_username;
   ```

5. **List Databases:**
   ```sql
   \l
   ```

6. **Connect to a Database:**
   ```sql
   \c your_database_name
   ```

7. **List Tables:**
   ```sql
   \dt
   ```

8. **Basic Queries:**
   - Select all records from a table:
     ```sql
     SELECT * FROM your_table_name;
     ```

   - Insert a record into a table:
     ```sql
     INSERT INTO your_table_name (column1, column2) VALUES (value1, value2);
     ```

   - Update records in a table:
     ```sql
     UPDATE your_table_name SET column1 = value1 WHERE condition;
     ```

   - Delete records from a table:
     ```sql
     DELETE FROM your_table_name WHERE condition;
     ```

9. **Exit PostgreSQL:**
    ```sql
    \q
    ```

# Backup and Restore

## Dumping a Database:

### Using `pg_dump`:

1. Open a terminal and run the following command to dump a database:

   ```bash
   pg_dump -U your_username -d your_database_name -F c -f /path/to/your/backup_file.dump
   ```

   - `-U`: Specifies the PostgreSQL user.
   - `-d`: Specifies the name of the database.
   - `-F c`: Specifies the custom format for the dump.
   - `-f`: Specifies the output file for the dump.

   Replace `your_username`, `your_database_name`, and `/path/to/your/backup_file.dump` with your actual values.

2. You will be prompted for the password of the specified user.

### Using `pg_dumpall` (for all databases):

1. Open a terminal and run the following command to dump all databases:

   ```bash
   pg_dumpall -U your_username -F c -f /path/to/your/backup_file.dump
   ```

   Similar to `pg_dump`, replace `your_username` and `/path/to/your/backup_file.dump` with your actual values.

## Restoring a Database:

1. Open a terminal and run the following command to restore a database:

   ```bash
   pg_restore -U your_username -d your_database_name -F c -c /path/to/your/backup_file.dump
   ```

   - `-c`: Clears (drops) the database before restoring.

   Replace `your_username`, `your_database_name`, and `/path/to/your/backup_file.dump` with your actual values.

2. You will be prompted for the password of the specified user.

> **Note:**
>
> - Ensure that the PostgreSQL service is running before attempting to
>   restore a database.
> - Adjust file paths, usernames, and database names according to your
>   environment.
> - The `-F c` flag specifies the custom format, which is often used for
>   better compression and additional options. Adjust it based on your
>   requirements.
>

## Restoring a Database with `psql`:

1. Open a terminal and navigate to the directory where your backup file is located.

2. Run the following command to restore a database:

   ```bash
   psql -U your_username -d your_database_name -f /path/to/your/backup_file.sql
   ```

   - `-U`: Specifies the PostgreSQL user.
   - `-d`: Specifies the name of the database.
   - `-f`: Specifies the input file for the SQL script.

   Replace `your_username`, `your_database_name`, and `/path/to/your/backup_file.sql` with your actual values.

3. You will be prompted for the password of the specified user.

4. The SQL script will be executed, and the database will be restored.

### Example:

```bash
psql -U postgres -d mydatabase -f /path/to/your/backup_file.sql
```

> **Note:**
>
> - Make sure that the PostgreSQL service is running before attempting
>   to restore a database.
> - Adjust file paths, usernames, and database names according to your
>   environment.
> - The `-f` flag specifies the input file containing SQL commands to be
>   executed.

> **Note:**
>
> This method is suitable for SQL script backups. If you have a
> custom-format dump file (created with `pg_dump -F c`), you may need to
> use the `pg_restore` command instead. However, for simple SQL script
> backups, the `psql` command is often sufficient.


# Administration tips

1. **Regular Backups:**
   - Implement a regular backup strategy using tools like `pg_dump` or `pg_basebackup` to ensure data integrity and facilitate disaster recovery.

2. **Monitoring and Logging:**
   - Set up monitoring tools and regularly review PostgreSQL logs to identify potential issues early on. Tools like PgBadger can help analyze PostgreSQL logs.

3. **Vacuum and Analyze:**
   - Run regular `VACUUM` and `ANALYZE` operations to reclaim storage space and update statistics for query optimization.

     ```sql
     VACUUM ANALYZE;
     ```

4. **Indexing:**
   - Properly index your tables to improve query performance. Regularly review and optimize indexes based on query patterns.

5. **Configuration Tuning:**
   - Adjust PostgreSQL configuration parameters in `postgresql.conf` based on your system's hardware and workload. Key parameters include `shared_buffers`, `effective_cache_size`, and `work_mem`.

6. **Connection Pooling:**
   - Consider using connection pooling solutions like PgBouncer or Pgpool-II to efficiently manage database connections and improve performance.

7. **Security:**
   - Regularly update PostgreSQL and apply security patches to protect against vulnerabilities.
   - Use strong authentication mechanisms, including SSL/TLS for secure connections.
   - Limit database access to the principle of least privilege.

8. **Replication:**
   - Implement replication for high availability and scalability. PostgreSQL supports various replication methods, including streaming replication and logical replication.

9. **Database Maintenance:**
   - Regularly review and optimize database schema design.
   - Consider partitioning large tables to improve query performance.

10. **Error Handling:**
    - Implement error handling and alerting mechanisms for critical issues. Tools like pgBadger and Sentry can help analyze and alert on PostgreSQL errors.

11. **Upgrade Planning:**
    - Plan and schedule upgrades carefully. Test the upgrade process in a staging environment before applying it to production.

12. **Resource Utilization:**
    - Monitor resource utilization, including CPU, memory, and disk space, to identify potential bottlenecks.

13. **Documentation:**
    - Maintain comprehensive documentation for your database environment, including configurations, backup procedures, and recovery plans.

14. **Regular Health Checks:**
    - Conduct regular health checks on your PostgreSQL instance to identify and address any potential issues.

15. **Community Resources:**
    - Stay connected with the PostgreSQL community. Forums, mailing lists, and online resources can provide valuable insights and solutions to common challenges.

Related
```
* https://tecadmin.net/install-postgresql-11-on-centos/
* https://thecodinginterface.com/blog/postgresql-changing-data-directory/
```
