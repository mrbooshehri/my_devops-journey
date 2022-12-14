# Install Jira with Mysql

## Mysql configuration

### Mysql 5.7

```mysql
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER,INDEX on jiradb.* TO 'jirauser'@'%' IDENTIFIED BY '<PASSWORD>';
flush privileges;
```

### Mysql 8

Enter ```mysql``` command then run the following query.

```mysql
CREATE DATABASE jiradb CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'jirauser'@'%' IDENTIFIED BY '<password>'; 
ALTER USER 'jrauser'@'%' IDENTIFIED WITH mysql_native_password BY '<password>';
GRANT ALL PRIVILEGES ON jiradb.* TO 'jirauser'@'%'; 
FLUSH PRIVILEGES;
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
then copy ```mysql-connector-java``` to ```/PATH/TO/JIRA/lib```, Download [here](https://dev.mysql.com/downloads/connector/j/8.0.html)

## Install openjdk
```bash
apt insatll openjdek11-jre
```

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

> **Note:** For jira core use ```jc```


## systemd unit

Create a unite file like bellow

```bash
vim /etc/systemd/system/jira.service
```
then put the following lines there
```bash
[Unit]
Description=Atlassian JIRA Software Server
After=network.target postgresql.service

[Service]
Type=forking
User=jira
ExecStart=/opt/atlassian/jira/bin/start-jira.sh
ExecStop=/opt/atlassian/jira/bin/stop-jira.sh
ExecReload=/opt/atlassian/jira/bin/start-jira.sh | sleep 60 | /opt/atlassian/jira/bin/stop-jira.sh

[Install]
WantedBy=multi-user.target 
```

> **Note:** Don't forget to remove legacy SysV
```bash
sudo rm /etc/init.d/jira
```

> **Note:** Get ```obr``` files from [here](https://marketplace.atlassian.com/apps)

