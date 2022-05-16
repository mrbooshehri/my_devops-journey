# Session 1

## Installation

### Network time protocol and timezone

```bash
timedatectl set-timezone Asia/Tehran
yum install ntpdate
ntpdate pool.ntp.org
systemctl start ntpdate
systemctl enable ntpdate
```

### Install Zabbix repository

```bash
rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
yum clean all
```

### Install Zabbix server and Agent

```bash
yum install zabbix-server-mysql zabbix-agent
```

### Install Zabbix fortend

```bash
yum install centos-release-scl
```

then make the following change in ```/etc/yum.repo.d/zabbix.repo```
```
[zabbix-frontend]
...
enabled=1
...
```
```bash
yum install zabbix-web-mysql-scl zabbix-apache-conf-scl
```

### Install MariaDB

```bash
yum install MariaDB-server MariaDB-client
systemctl start mariadb
systemctl enable mariadb
maraiadb-secure-installation
```

### Create initail database

```bash
mysql -uroot -p
```
```mysql
create database zabbix character set utf8 collate utf8_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
flush previleges;
quite;
```

### Importin intial data

```bash
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix
-p zabbix
```

### Configure database for zabbix server

```bash
vim /etc/zabbix/zabbix_server.conf
```

```vim
DBPassword=password
```

### Install apache web server

```bash
yum install httpd
rm -rf /etc/httpd/conf.d/welcome.conf
vim	/etc/httpd/conf/httpd.conf
```
```vim
ServerAdmin root@zabbix.vasl.local
ServerName zabbix.vasl.local:80
AllowOverride All
DirectoryIndex index.html index.cgi index.php
ServerSingnature off
ServerTokens Prod
KeepAlive On
```

### Starting Apache web service

```bash
systemctl start httpd
systemctl enable httpd
firewall-cmd --add-service={http,https} --permanent
firewall-cmd --reload
```

### Configure PHP for zabbix frontend

Eddit the following line under
```/etc/opr/rh/rh-php72/php-fpm.d/zabbix.conf```

```vim
php_value[date.timezone] = Asia/Tehran
```

### Start Zabbix server and agent proccess

```bash
systemctl restart zabbix-sever zabbix-agent httpd rh-php72-php-fm
systemctl enable zabbix-sever zabbix-agent httpd rh-php72-php-fm
```

### Configure firewall 

```bash
firewall-cmd --add-port={10050/tcp,10051/tcp} --permanent
firewall-cmd --reload
```
## Tips
* ```epel-release```
* ```centos-release-scl```
* If you are using socket to connect to mysql, you won't need to set
	value for ```DBPassword```
* for security reasons, remove apach/nginx welcom page ex:
	```bash
	rm -f /etc/httpd/conf.d/welcome.conf
	```
* Add ```KeepAlive on``` at the end of httpd.conf 

# Session 2

## SELinux configuration

If you don't want to disable SELinux, follow the instruction

```bash
setsebool -P httpd_can_connect_zabbix on
setsebool -P httpd_can_network_connect_db on
yum install policycoreutils-python
grep zabbix_t /var/log/audit/audit.log | grep audit2allow -M zabbix_server_custom
semodule -i zabbix_server_custom.pp
systemctl restart httpd
grep denied /var/log/audit/audit.log | grep audit2allow -M zabbix_server_custom
```

**Note:** It's better to use zabbix proxy to reduce connections to
zabbix server.

## Tips

You can save login info credentials to avoid enter them each time you
want to login

```bash
ssh-copy-id HOST@xxx.xxx.xxx.xxx
```

# Session 3

### Check server and agent connection

1. Ping server from client and vise versa - Please consider that maybe
	 ICMP protocol is disable
1. Telnet to check the port (10050, 10051)

### Add host to Zabbix

Host is the device that you want to monitor it.

1. Go to ```configuration ```section
1. Click on ```crete host```
1. Enter suitable host name
	1. Host name must be uniqe, and it's a good
	 practice to name your host hyphenized or in camel standard. 
	 1. Hostname should be the same with the name you set in ```zabbix_agent.conf```
1. Enter suitable visible name if you need to. It makes the host more
	 understandable
1. Chose/create sutiable host group
	1. It's helpful in managing hosts 
	1. In zabbix you cannot give direct host permission to a user but
	 with the help of host group you can do this. 
	 1. You can add multiple host group to a host
	 1. Never add ```Templates/...``` in host group
1. Chose the protocol you want to connect to Zabbix agent -
	 IP/SNMP/JMX/IPMI
1. Enter agent IP
1. Click on add

### Add item to host

Item is the thing that we want to monitor. The number of items that we
want to monitor is important in allocating hardware resources.

**Note:** Unless you don't add item for your host zabbix is not going to
collect any data

1. 	under ```configuration``` section under ```Hosts```, choose
		```item``` in front of your desire host from the table
1. click on create item 
1. Choose a suitable name
1. Choose proper type
1. Choose a proper key - key should be uniqe in this host only. 
	1. Some keys have thier own stracture, for example ```icmppinglost```
		 item under ```simple check``` type gives several input arguments
1. Choose suitable ```type of information```
	1. Numeric(unsigned) - Positive integer numbers
	1. Float - Negative or floating point numbers
	1. Character
	1. Log
	1. Text
1. Enter proper unit in ```Units``` section
	It just used for clarification purpose when you look at reports
1. Select interval
	1. ```Update interval```: It gets report every [1m, 1d, 1w, ...]

### Setting trigger

You can create custom trigger to notify certain situation.

1. Under ```configuration``` selecet ```Hosts```, in front of your
	 desire host, click on ```triggers```
1. Click on ```create trigger```
1. For name section it's a good practice to write a meaningful sentence
	 for more clarification
1. Choose proper severity
1. Select ```add``` button under ```Expression``` section and choose
	 proper expression
	 1. If you don't enter a value for ```Last of (T)``` for ```Last()```
			function, it would get the last single data by default. and also
			you can set time limit, ```T```, to check the last value of
			```T``` times ago
1. Click on ```Expression constructor```
1. Click on ```Add``` button

**Note:** It's a better practice to set the trigger after you analyze
the data you get from item and then write the best trigger for your case. So before you 
create a trigger you need to observe the return values form your desire item.

### ```Last data``` section
In this section you can get information about the latest data about each
host. You can set various filters and selecet single or muliple host to
its/thier graph

### ```ACK``` section
In this section you can leave message about the specific problem, change
sensitivity, or even close the problem.

### ```Overview``` section
you can monitor all triggers and thier status for each agent in a table.

### ```Tags``` section
In ```Hosts``` section on each host, there is a ```host``` section where
you can add a tag to server. 

## ```Inventory``` section
In this section you can enter all the useful information about the
agent's machine like OS, type, or even contact number of people who are
involved. 

**Note:** Zabbix can also get some of the information
automatically.

### Tips
1. set/query hostname with ```hostnamectl```
	1. to set ```hostnamectl set-hostname HOSTNAME```

# Session 4

In each item you can get just one value, but in zabbix 5 and later you
can define a master item then feed other items.

### ```SSH check``` another agentless check

#### Create public and private key for Zabbix user

1. First of all stop agent and server then generate your key.
	```bash
	systemctl stop zabbix-server
	systemctl stop zabbix-agent

	```
1. Change ```zabbix``` home directory to store keys.
	```bash
	usermod -m -d /home/zabbix
	mkdir /home/zabbix
	chmod 700 /home/zabbix
	```
1. start both agent and server again
```bash
	systemctl start zabbix-server
	systemctl start zabbix-agent
```
1. Generate ssh key for ```zabbix``` user
	```bash
	sudo -u zabbix ssh-keygen -t rsa
	# leave the passphrase EPTY
	```
1. To privent entering ssh passphrase each time use the following
	 command:
	 ```bash
	 sudo -u zabbix ssh-copy-id root@IP_ADDRESS
	 ```
1. then in ```/etc/zabbix/zabbix_server.conf``` add the path of your
	 keys(```SSHKey=```). 

1. Go to ```items``` section
	1. Create new item
	1. Set item ```key``` to ```ssh.run```
	1. Write your key between ```[]```
	1. set authentication method to ```public key```
	1. Enter ```root``` as username
	1. Enter public key file value ```id_rsa.pub```
	1. Enter private key file value ```id_rsa```
	1. write your command in ```Execution script``` box

**Note:** make sure to cheack ```/var/log/audit/aduit.log``` if you are
using ```selinux```.

### ```zabbix-agent.conf```
- ```SourceIP=```: detrmine the ip address of the network adapter that
	you wand to send data to server with it, in case of you have multiple
	NICs.
- ```AllowKey``` and ```DenyKey```: they are used for limiting the keys
	on zabbix agent, for example you can deny some of default keys such as
	```sys.run[*]```.
- ```StartAgent```: It defines the number of zabbix agents as separate
	processes to increase the performance. Best value is the range of 3-5.
- ```HostnameItem```: Get host name from system and set it as the zabbix
	hostname.
- ```RefreshActiveCheck```: How often list of active checks is
	refreshed.
- ```BufferSend```: Size of data that zabbix agent keep in case of
	disconnect from server.
- ```Timeout```: in case of hang in infint situation it halt the process

**Note:** by enabling ```EnableRemoteCommand``` and ```action```s you
can run commands on agent 

### Active check vs passive

Passive -> Server ask about a data from the agent

Active -> Agent ask server which kind of data it wants, then send the
data to the server

**Note:** Passive chech is more common 


### User Parameter

if you want to run a script by getting specific key you should defined
it in the config file under ```UserParameter``` section. example:
```vim
...
UserParameter=mysql.status,if [[ $systemctl is-active mariadb.service == active]]; then echo 1; else echo 0; fi
...
```
**Note:** This practice is not common, instead you can put these hooks
under ```/etc/zabbix/zabbix_agentd.d``` as seperate files.

###	```zabbix_get``` and ```zabbix_sender``` 

Install them with the folloing command
```bash
yum install zabbix-get zabbix-sender
```
you can get value from other agents over terminal like
```bash
zabbix_get -s HOSTNAME_OR_IP -k ITEM_KEY
```
There are some predefined keys like ```agent.ping``` which you can use
in previous command

**Note:** Please consider that you need to open ```10050/tcp``` port on
the machine you want to get data from

```bash
firewall-cmd --add-port=10050/tcp --permanent
firewall-cmd --reload
```
### Zabbix agetn template
In the host configuration you need to add ```Template OS Linux by zabbix
agent```, in this case we use linux, to start monitoring.

# Session 5

## Use ```UserParameter=```
You can write your own scripts in ```zabbix_agentd.cong``` or in
```zabbix_agentd.d``` as separated files like ```SCRIPT_NAME.conf```.
Your file content should follow this structure:
```bash
UserParameter=SCRIPT_KEY, SCRIPT
```
**Note:** Please don't forget to restart your zabbix agent to make the
chages effect.
```bash
systemctl restart zabbix-agentd
```

## Run script using Telnet
First set type on ```telnet``` then enter your telnet user and password.
For script part; remember you need to enter a one liner script, sprate
commands with ```;``` and then put ```#``` at the new line.

## Inventory automatic value input
In each agent invetory section you can select ```Automatic``` option. it
will get some of data accorting to the template it has and put them on the list.
for example ```hostname``` and ```operating system``` in ```Zabbix agent
for linux OS```


## Value mapping

## Monitroing with SNMP

```bash
yum install net-snmp net-snmp-utils
```
config file
```bash
vim /etc/snmp/snmpd.conf
```


