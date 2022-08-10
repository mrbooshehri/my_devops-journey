# Install Jira with Mysql

## Mysql configuration

```mysql
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER,INDEX on jiradb.* TO 'jirauser'@'%' IDENTIFIED BY '<PASSWORD>';
flush privileges;
```

Add the following lines in ```/etc/mt.conf```
```mysql
[mysqld]
default-storage-engine=INNODB
character_set_server=utf8mb4
innodb_default_row_format=DYNAMIC
innodb_large_prefix=ON
innodb_file_format=Barracuda
innodb_log_file_size=2G
# remove this if it exists
#sql_mode = NO_AUTO_VALUE_ON_ZERO
```
then copy ``mysql-connecor-java``` to ```/PATH/TO/JIRA/lib```

## Install Jira servicedesk
Use costom installation and choose your favorite configurations, but at the end **do not start** the service. Then Edit /PATH/TO/JIRA/bin/setenv.sh and add the the following line.

```bash
export JAVA_OPTS="-javaagent:/PATH/TO/JIRA/atlassian-agent.jar ${JAVA_OPTS}"
```

now start the service
```bash
/PATH/TO/JIRA/bin/start-jira.sh
```
follow the installation to get ```server id```, then run the following command.
```bash
java -jar /PATH/TO/atlassian-agent.jar  -m <EMAIL> -o <ORGANIZATION_NAME> -p jsd -s <SERVER_ID>
```
then use the generated key.

