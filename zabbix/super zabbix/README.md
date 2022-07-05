* [Session 1](#session-1)
   * [Installation](#installation)
      * [Network time protocol and timezone](#network-time-protocol-and-timezone)
      * [Install Zabbix repository](#install-zabbix-repository)
      * [Install Zabbix server and Agent](#install-zabbix-server-and-agent)
      * [Install Zabbix fortend](#install-zabbix-fortend)
      * [Install MariaDB](#install-mariadb)
      * [Create initail database](#create-initail-database)
      * [Importin intial data](#importin-intial-data)
      * [Configure database for zabbix server](#configure-database-for-zabbix-server)
      * [Install apache web server](#install-apache-web-server)
      * [Starting Apache web service](#starting-apache-web-service)
      * [Configure PHP for zabbix frontend](#configure-php-for-zabbix-frontend)
      * [Start Zabbix server and agent proccess](#start-zabbix-server-and-agent-proccess)
      * [Configure firewall](#configure-firewall)
   * [Tips](#tips)
* [Session 2](#session-2)
   * [SELinux configuration](#selinux-configuration)
   * [Tips](#tips-1)
* [Session 3](#session-3)

      * [Check server and agent connection](#check-server-and-agent-connection)
      * [Add host to Zabbix](#add-host-to-zabbix)
      * [Add item to host](#add-item-to-host)
      * [Setting trigger](#setting-trigger)
      * [Last data section](#last-data-section)
      * [ACK section](#ack-section)
      * [Overview section](#overview-section)
      * [Tags section](#tags-section)
   * [Inventory section](#inventory-section)
      * [Tips](#tips-2)
* [Session 4](#session-4)

      * [SSH check another agentless check](#ssh-check-another-agentless-check)
         * [Create public and private key for Zabbix user](#create-public-and-private-key-for-zabbix-user)
      * [zabbix-agent.conf](#zabbix-agentconf)
      * [Active check vs passive](#active-check-vs-passive)
      * [User Parameter](#user-parameter)
      * [zabbix_get and zabbix_sender](#zabbix_get-and-zabbix_sender)
      * [Zabbix agetn template](#zabbix-agetn-template)
* [Session 5](#session-5)
   * [Use UserParameter=](#use-userparameter)
   * [Run script using Telnet](#run-script-using-telnet)
   * [Inventory auto collection](#inventory-auto-collection)
   * [Time Intervals](#time-intervals)
      * [Custom interval](#custom-interval)
   * [Storage period](#storage-period)
   * [Value mapping](#value-mapping)
   * [Application Name](#application-name)
   * [Monitroing via SNMP agent](#monitroing-via-snmp-agent)
      * [Setup SNMP on zabbix UI](#setup-snmp-on-zabbix-ui)
   * [Macros <strong>(Read more on zabbix official website)</strong>](#macros-read-more-on-zabbix-official-website)
* [Session 6](#session-6)
   * [Adding SNMP item](#adding-snmp-item)
* [Session 7](#session-7)
   * [Custom Graph](#custom-graph)
   * [Screens](#screens)
      * [Sliedshows](#sliedshows)
   * [Maps](#maps)
      * [Creating new map](#creating-new-map)
   * [Trigers](#trigers)
      * [Expression functions](#expression-functions)
* [Session 8](#session-8)
   * [Triggers](#triggers)
   * [Prediction and Forecasting](#prediction-and-forecasting)
      * [Ok Event Generation](#ok-event-generation)
         * [Recovery Expression](#recovery-expression)
   * [Tags](#tags)
   * [Dependences](#dependences)
   * [Event correlation](#event-correlation)
   * [Templates](#templates)
      * [Nested Templates](#nested-templates)
   * [Macro](#macro)
   * [http agent](#http-agent)
   * [Tips](#tips-3)
      * [Get Aapache status](#get-aapache-status)
      * [User Hostname in item title](#user-hostname-in-item-title)
      * [Operational data](#operational-data)
* [Session 9](#session-9)
   * [Web monitoring](#web-monitoring)
      * [Web Scenario:](#web-scenario)
      * [Wep steps](#wep-steps)
   * [Scripts](#scripts)
      * [External check](#external-check)
      * [Zabbix trapper](#zabbix-trapper)
   * [Tips](#tips-4)
* [Session 10](#session-10)
   * [ODBC](#odbc)
      * [Intsall ODBC on zabbix server](#intsall-odbc-on-zabbix-server)
      * [Configture ODBC item](#configture-odbc-item)
      * [Configture ODBC item](#configture-odbc-item-1)
   * [Calculated item type](#calculated-item-type)
   * [Prediction graphs](#prediction-graphs)
   * [Descovery](#descovery)
      * [Low Level discovery (LLD)](#low-level-discovery-lld)
      * [Createing discovery rule](#createing-discovery-rule)
   * [Tmplate vs Discovey](#tmplate-vs-discovey)
   * [Network discovery](#network-discovery)
   * [Actions](#actions)
      * [Action fields](#action-fields)
   * [Auto registration](#auto-registration)
* [Session 11](#session-11)
   * [Services](#services)
   * [Log file monitoring](#log-file-monitoring)
      * [Monitoring a singe file](#monitoring-a-singe-file)
         * [Useful trigger functions for log monitoring](#useful-trigger-functions-for-log-monitoring)
* [Session 12](#session-12)
   * [Define Regex](#define-regex)
   * [Monitor log files with time stamp](#monitor-log-files-with-time-stamp)
   * [Grafana and Zabbix](#grafana-and-zabbix)
      * [Install Grafana](#install-grafana)
      * [Install Zabbix plugin on Grafana](#install-zabbix-plugin-on-grafana)
      * [Connect Zabbix to Grafana](#connect-zabbix-to-grafana)
      * [Creating custom dashboard](#creating-custom-dashboard)
      * [Alerting](#alerting)
* [Session 13](#session-13)
   * [Zabbix proxy](#zabbix-proxy)
   * [Install Zabbix Proxy](#install-zabbix-proxy)
      * [Active vs Passive zabbix proxy](#active-vs-passive-zabbix-proxy)
      * [Add zabbix proxy in zabbix server](#add-zabbix-proxy-in-zabbix-server)
   * [Queue](#queue)
   * [Docker and zabbix](#docker-and-zabbix)
* [Session 14](#session-14)
   * [Tuining](#tuining)
      * [Understand zabbix_server.conf deeper](#understand-zabbix_serverconf-deeper)
      * [Prtitioning database](#prtitioning-database)
      * [Elasticsearch and Zabbix](#elasticsearch-and-zabbix)
         * [Important tables in zabbix database](#important-tables-in-zabbix-database)

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

## Inventory auto collection
In each agent invetory section you can select ```Automatic``` option. it
will get some of data according to the template it has and put them on the list.
for example ```hostname``` and ```operating system``` in ```Zabbix agent
for linux OS```

You can set your custom items return value as inventory automatice
input feild. You just need to specify the ```Populate host inventory
filed``` when you're adding an item, in ```create item``` page.

## Time Intervals
In ```Create item``` page you can set an interval to get data from
zabbix agent on regular sequence. In this method you cannot set the
start time. If you want to get information in specific times, for
example 20:00, 20:30, and etc. , you should use ```Custom
Intervals```.
```Update Interval``` is a mandatory option in itrem creation time, if
you want to use ```Custom Intervals``` instead, you need to set the
```Update Interval``` to ```0``` to prevent any interval collision.

### Custom interval
* Flexible:
	* Interval: It's exactly as the same as ```Update Interval```
	* Period: It defines the time period which ```Interval``` iterates in
		it. It's like ```1-7,8:00-23:59``` which means from Monday to
		Saturday, from 8:00 AM to 23:59 PM
	* You can set multiple itervals. If some interval overlap, the
		Smallest one will iterate.

* Scheduling: It's something close to cron jobs. It works based on
	filters which should write in specific order ```mdwdhms```. filters list:
	* ```md```: Month day - valid values 1-31
	* ```wd```: Week day - valid values 1-7
	* ```h```: hours
	* ```m```: minuts
	* ```s```: seconds

**Note:** You can define time steps in your time scheduling, for example
```md1-31/1``` works on from 1 to 31 of a month every other day. It
works for other filters as well.
**Note:** If you don't choose Specific time it works on ```00:00:00```.
**Note:** ```wd6``` means iterate only on Saturdays.

## Storage period
* History storage period: It's the exact data.
* Trend storage period: It's stores a compact version of data. It
	collects data overy hour and stores min, max, and average values.

## Value mapping
With this option you can make alias for item value. For example if the
item returns 0 or 1, you can specify miningful alias. You can define
your value maps in ```value mapping``` under
```Administration/General``` section.

## Application Name
You can specify your item as an application by name it in ```New
application``` or choose it form ```Applications``` list.

## Monitroing via SNMP agent

SNMP has different versions, V1 is not perfered because it doesn't have
any security mechanism, V2 use communitiy key as the security suloution,
default community key is ```public```. V3 has propare security solution,
uses ssl encryption.

SNMP almost used in type of devices which they don't support zabbix
agent or don't have OS like, switches, routers, servers, rack door's
sensor, and etc.

install SNMP client on zabbix server. the following package is used for
SNMP server and client.

```bash
yum install net-snmp net-snmp-utils 
```

you need to make some changes in config file under ``` vim
/etc/snmp/snmpd.conf```.

```bash
...
com2sec	notConfigUser	default	YOUR_CUMMUNITY_PASSWORD
...
			name				inc/exc		subtree
view	systemview	included	.1
...
```
then you need to enable and start SNMP service
```bash
systemctl enable snmpd
systemctl start snmpd
```
to list all avialable SNMP values you can use this command
```bash
snmpwalk -v 2c -c YOUR_CUMMUNITY_PASSWORD SERVER_IP
```
to get more information about ```MIB.OID.UID```  of the device that you
want to monitor via SNMP, head over to the official documentation of
that device. Also you can list all the SNMP Values without resolving
theier name by the following command:
```bash
snmpwalk -v 2c -c YOUR_CUMMUNITY_PASSWORD -O n SERVER_IP 
```
**Note:** You can download any MIB file that is not exist on your server
and put the file on server and also on your agent.

**Note:** Don't forget to open ```SNMP``` port in firewall.
```bash
firewall-cmd --add-port=161/udp --permanent
firewall-cmd --reload
```
### Setup SNMP on zabbix UI
In host configuration section add new interface and set it on ```SNMP```
and choose version 2, then add ```SNMPv2``` in templates

## Macros **(Read more on zabbix official website)**
Macros are kind of variable which helps to map values securely to name.
Macro format is like ```{$MACRO_NAME}```

# Session 6

## Adding ```SNMP``` item

In item creation set the agent type on ```SNMP Agent```, then you need
to choose a propare key, for example
```snmp.system.mac.address.enp0s3```. Then enter your desired SNMP OID.
The rest of the configuration is the same as previous

# Session 7

In ```Latest data``` section you can select multiple items and display
them as a grph form the bottom of the list.

## Custom Graph
Each hosts can has its own custom grapgh. Under ```Configuration >
Hosts```, in fron of each host there is a ```graph``` section which you
can define your custom graphs there.

## Screens
In zabbix you can display multiple graphs as a slidshow. It cloud be
find under ```Monitoring``` section.

In ```create screen``` section you can define size of the screen (number
of row and columns), also you can set access permissions to the screen.
After you defined your screen you need to construct it and add some
graphs to it to display.

### Sliedshows

Under ```screen``` section you can click on ```screen``` hyperlink in
top right of the page then select slideshow and make one there.

## Maps
It helps in several manner take for example, making a diagram from our
network.

### Creating new map

In ```Map``` section click on ```create map``` button then enter the
required information.

## Trigers

### Expression functions

1. ```last()```
1. ```abschange()```
1. ```avg(a,b)```  - ```a``` can be time or count, ```b``` is timeshift,
	 it means ```b``` times earlier
1. ```change()```
1. ```count()``` - ```T``` is past time, ```V```is given value and
	 ```O``` is operator. Valid operators, ```O```s are
	  {eq, ne, gt, ge, lt, le, like, band, iregexp, regexp}
1. ```date()``` - if useful in combinational expressions. Valid date
	 format ```YYYYMMDD```
1. ```dayofmounth()```
1. ```diff()``` 
1. ```fuzzytime()```
1. ```max()```
1. ```nodata()```
1. ```now()```
1. ```sum()```
1. ```time()``` - Valid time format ```HHMMSS```
1. ```band()``` - Find out if a received value is even or odd is an
	 example this function.
1. ```percentile``` - It gets Persentage of values ot ```T``` times ago
	 and check thme with the condition. For example it 75% of the data of
	 30 minutes ago are less than 10 fires a trigger

**Note:** Be aware of **False-Posetive** and **False-Negative**
expression errors.

# Session 8

## Triggers

In expression section you can compare two variable with eachoter instead
of comparing them with just a number. 
```bash
host1.tcp.listen[80].last()=host1.tcp.listen[80].avg(5,10)
```
## Prediction and Forecasting

There is an official article on Zabbix website about this section but
here we just want to check perdefined functions. [here](https://www.zabbix.com/documentation/5.0/assets/en/manual/config/triggers/prediction_docs.pdf)

Zabbix has two main functions for forecasting:
* ```timeleft()```
	* ```Last of T```: Means, last ```T``` times ago. For example
		```30m``` means 30 minutes ago.
	* ```Time Shift```: It's a step for previous value for example if you
		consider ```1d``` for time shift, zabbix will get the data of last
		30 minutes in, according to current time, for a day ago
	* ```Threshold```: The value that we are sensitive to it
	* ```fit```: It's the mathematical model - linear, ploynomialN,
		exponensial, logaritmic, power - which you chose to predice
		your dataset. You can left this field empty.

**Example:**

The following expression will fire a trigger when there is only one day
left to host fs size reach to 10GB. The expression forcast the data of
last 30 minutes of today with the exact time for a day ago considering
10GB as its threshold.
```bash
{host:vfs.fs.size[/,used].timeleft(30m,1d,1000000000)}<1d
```
* ```forecast()```
	* ```Last of T```: Means, last ```T``` times ago. For example
		```30m``` means 30 minutes ago.
	* ```Time Shift```: It's a step for previous value for example if you
		consider ```1d``` for time shift, zabbix will get the data of last
		30 minutes in, according to current time, for a day ago
	* ```Time```: The next t times

**Example:**

The following expression will fire a trigger if the disk space of one
hour later be more than 10GB according to the disk space values of 30
minutes ago.

```bash
{host:vfs.fs.size[/,used].forecast(30m,1h,polynomial)}>1000000000
```
### Ok Event Generation

There are three types os Expression in this section:
1. Expression
1. Recovery Expression
1. None

#### Recovery Expression
As well as we can set triggers for problem and disaster situations, we
can also defined recovery expression, which means if the condition, the
one we define in recovery expression, is true the situation is ok and
the disaster trigger will remove automatically.

Attention to the following example:

we want to fire a trigger if disk space is more than 10GB and remove
this trigger automatically if the disk space is less than 8GB.

**Disaster expression**

```bash
{host:vfs.fs.size[/,used].forecast(30m,1h,polynomial)}>1000000000
```

**Recovery expression**

```bash
{host:vfs.fs.size[/,used].forecast(30m,1h,polynomial)}<800000000
```

## Tags
It helps in event correlation, do action on hosted which has the same
tag or better to say there is correlation between them.

## Dependences
In this section we cam make dependencies between triggers. For example
there are two triggers, ```A```  and ```B``` and the trigger ```B```
depends on ```A```. If we define this dependency chain in the trigger
section, it will helps us to have a cleaner overview tab. Because zabbix
does not fire trigger for the triggers that are lower in the hierarchy.

**Real-life Example:**

We are monitoring a router and a server at the same time and defined a
dependecy chain between them. So each time the router goes down zabbix
just fire a trigger for router not for both of them.

## Event correlation
It's under ```configuration``` section. It's simillar to dependecy with
a big diffrence, dependecy will not show the dependet triggers but
correlation will close the triggers.

For making correlation you need to defind a relationship between your
tags, for example ```old event tag name``` AndOr ```new event tag
name```, thne set the operation in ```operations``` tab.

**Note:** It's a good approach to consider hyrarcical structures of
bottlenecks as dependency, and related events as correlation.

## Templates
Templates help us to define items, application name, trigger, screen,
graph, and etc. once and then use them in our hosts. There are a lot of
predefined templates in zabbix and much more on ```share``` section. To
add your downloaded template to zabbix just go to ```Templates```
section then import the downloaded file.

### Nested Templates
Sometimes we can add other templates in our template. Our template could
contain only ```linked templates```, like zabbix ```Template OS Linux
serve by Zabbix agent``` official template.

## Macro
Macros are equivalent to variablesin Zabbix. Macros define in
```Hosts``` or ```Templates``` There are two types of Macros.

1. System Macro: The Macros that defined by Zabbix
1. User Macro: The Macros that define by user

Valid macro format is like ```{$VARIABLE_NAME}```. It's better practice
to name your macros in capital letters.

* You can store Macro's value as a plane text or secret text
* You cam use macros almost anywhere you want, Name field, Expression,
	and etc.

## ```http agent```
It's a useful agent that helps you post, get, and etc. to a dynamic URL
you can also have request body, headers, response code, and etc.

## Tips
### Get Aapache status
Just add the following lines in your Aapache config then reload it's
daemon. Apache confing file location ```/etc/httpd/conf/httpd.conf```
```bash
<Location "/server-status">
	SetHandler server-status
	Deny from All
	Allow from IP_ADRESS_YOU_WANT_TO_ACCESS
<Location>
```
### User Hostname in item title
With the help of macros you can wtire a general item for all hosts in
the name field like, ```Agent in not available on {$HOST.NAME}```. This
helps us to have an understanable title, sending error via messege or
Telegram.

### Operational data
If you want to opertational data in dashboard terminal, you should
define it in item configuration in ```operational data``` field.
```{#ITEM.VALUE}``` could be candidate for this field.

# Session 9

## Web monitoring
You can monitor a website with this feature with the help of scenarios
and steps.

### Web Scenario:
In this section you need to enter the basic information such as name,
application, intervals, variables, and headers.

### Wep steps
Here you should break down your web browsing scenario into steps.
Consider a situation you need to login to a website. The folloing steps
should be defined to login to the website:
1. Load the website
1. Find login button/link and click on it
1. Check we are in the right page
1. Enter username and password in the propare place
1. press login button
1. check we are in the right page

**Note:** In add step modal for ```Post field``` values, you need to
inspect elemet of the login page and check the source code of the page
and find the relevant field and take its name.

## Scripts
Scripts are another agentless monitoring method. There are two methods
to monitor with scripts:

### External check
These type of scripts are defined on a path on server/proxy. In the
```zabbix_server.conf``` the defualt value is
```ExternalScripts=/usr/lib/zabbix/externalscripts```, you can put your
scripts there and make it executable, also make the ownership of it to
```zabbix```. To use your scrip, in add item page select item type on
```External chck``` and for key value just enter the script name. If
your script has input argument you have to pass them between ```[]```
like ```MY_SCRIPTS[P1,P2,...]```. For making this flow more dynamic you
can define script based items in templates and pass macros as thier
input parameter like, in item template ```MY_SCRIPT[{$SERVICE_NAME},
{$PORT}]```, then in each host under macro section define sutable macro.
**(Super zabbix session 9 2:50)**

**Note:** If you want to use input arguments in zabbix agent in ```UserParameter``` you
should add a ```*``` between brackets like:
```bash
UserParameter=KEY[*], COMMAND $1 | COMMAND $2 ,...
```

### Zabbix trapper
In this type you can witre your script in any languages and palce it
where ever you want. Implanting this type has two sides:
1. Creating an item on zabbix server with type ```zabbix trapper``` and
	 a unique key
1. Write your script on the zabbxi agnet side and make it executable.
	 and send the scritp result to the server with the following command:
	 ```bash
	 zabbix_sender -z ZABBIX_SERVER/PROXY_IP -p 10051 -s
	 HOSTNAME_ON_ZABBIX_SERVER -k KEY -o VALUE
	 ```
	 **Note:** ```KEY``` is the one you set in zabbix item, ```VALUE``` is the one
	 you want to send to zabbix

It's a good practice to put the ```zabbix_sender``` command into your
script like:

```bash
#!/bin/bash
status_code=$(systemctl is-active mariadb | grep -c ^active)
zabbix_sender -z 192.168.100.2 -p 10050 -s mariadb.host -k
mysql.status.check -o "$status_code"
```

**Note:** To prevent run dangerous commands like ```rm```, it's a good
practice to add a user without root privilege and with ```nologin```
shell, like:
```bash
useradd scriptrunner
usermode -s /sbin/nologin scriptrunner
```

To run the script periodically, put it in cron tab, like:
```bash
sudo -u scriptrunner cronta	-e
```
in crontab add the folloing line to run the script every 5 minuts:
```bash
*/5 * * * *	SCRIPT_PATH
```

You can use [this](cronmaker.com) website to calculate cron rule easily.

**Note:** To prevent getting garbage value on you ```zabbix traper```,
you can strict the item, in item configuration/creation, by ```Allowed
hosts```, by this rule zabbix server only get values from specific
hosts(by ip).

## Tips 
1. Install ```fail2man``` to prevent DOS and DDOS attacks
1. Once you produced ```FILE_NAME.pp``` with ```allo2audit```, for your
	 specific demand, you can store it somewhere and use it as
	 ```semodule``` input parameter input parameter 

# Session 10

**Note:** Have check on ```http agent```.

## ODBC

ODBC is a third-party package which it allows the OS to connect directly
to the database and search queries on database. To monitor data on
zabbix we need to install ODBC package on zabbix server.

### Intsall ODBC on zabbix server
```bash
yum -y install unixODBC unixODBC-devel
```

ODBC package will make two files for us that we need to config them
before ues the package, but first we need to make sure the MYSQL
connector packe for ODBC, ```mysql-connector-odbc``` is installed. You
can search for other databases with the following command:
```bash
yum search odbc | grep DB_NAME
```

Che the folloing file and make sure related libraries for your case is
available on your system.
```bash
vim /etc/odbsinst.ini
```
Create/Edit ```odbc.ini``` file under ```/etc``` and enter the
information of the database you want to connect.

```bash
[Target_server]
Description = MySQL database 1
Driver = MySQL
Port = 3306
Server = 192.168.100.43
User = zabbix
Password = zabbix
Database = application
```

**Note:** ```Driver``` value should be the same as defined in
```/etc/odbcinst.ini```

**Note:** Please consider the ```User```, ```Password```, and
```Database``` you have input in ```/etc/odbc.ini```, **are NOT related
to zabbix database**. They're just credential information for the
database you want to monitor

On the database server open the related port on the firewall:
```bash
firewall-cmd --add-port=3306/tcp --permanent
firewall-cmd --reload
```

Now we can check ODBC with the folloing command:
```bash
isql Target_server
```

**Note:** ```mariadb``` my not work with older version of odbs
connectors in case of error, try to install it from source 
if it is not available on the official repository. If you install it
from source make a soft link to related library if it's not exits:
```bash
ln -s /usr/lib64/libmyodbc5w.so /uer/lib64/libmyodbc5.so
```

### Configture ODBC item

### Configture ODBC item

1. Choose ```Database monitor``` as its type
1. Select the propar key
	1. ```db.odbc.select``` - it can return a single item of a row in
		 the database table
	1. ```db.odbc.get``` - it can get all the data, including rows, and
		 trun it to json. You can use it as master item and feed dependent
		 items with it. This function is useful for those type of databases
		 that has limit on the number of sessions you can establish with the
		 database.
	1. for both type of keys:
		* enter a uniqe description - it's mandatory
		* for ```dsn```, enter ther database name you had enter in
			```odbc.ini```
		* You can skip the last argument, ```conncetion string```, and
			remove it
1. If you have username and password in your ```odbc.ini```, you don't
	 need to write them in the ```username``` and ```password``` field
	 respectively. If You want to stoer usename and password in the item
	 you can use the macros to store the password as a
	 secret text value for more security.

**Note:** Each database connection happens three times for a single
request:
	1. For login and authentication
	1. For executing your query
	1. For logout, disconnecting form the database

**Note:** You can add ```simple change``` type in preproccessing in each
item to check the number of values which came to server in last 5
minutes. **CHEACK IT ONLINE**

## ```Calculated``` item type
You can monitor events by calculating specific value in specific host
in any host itm.
1. Select ```calculated``` as item type
1. Enter a key
1. Write the following formula format
	```bash
	func(<key|hostname:key,parameter1,parameter12,,,,)
	```
	* func - all types of function we had in triggers like, last, min,
		max, avg, count, and etc.
	* The above formula is meaningless, becase there is no arithmatic
		operation performed on it.

## Prediction graphs

1. Create an item an choose ```calculeted``` as its type
1. Assgin a proper key to it
1. Write a formula like this:
	```bash
	forecast(sec|count, <time_shift>, time, <fit>, <model>)

**Note:** If you want to compare perdicted value with the current real
value, you need to create another calculated item and use the  previous predicet
calculated item in the new one as ```last()``` function paramter, then
with the help of time step of ```last()``` function overlap the
prediceted value and real value.

**Note:** Please consider you can use all predict models in as different
items and check which one is close to reality, but it's a very better
practice to get some knowledge about each one and their usecases.
	```

## Descovery
With this feature you can define dynamic items according to the
application you are communicate with, take odbc as an example. In this
example you need to create items, graphs, predictions, and etc. one bye
one for each set of records you want to monitor. Discovey feature will
add item automatically acording to the situation.

### Low Level discovery (LLD)
It will create item by the rule we defiend for it.

there are several ways to implement LLD like, ```ODBC```, ```SNMP```,
```JMX```, or even custom script.  

### Createing discovery rule
Under ```Hosts``` in ```Configuration``` tab in front of your desier
hsot you can find a discovery section. The discovery types are the same as
item types. Keys are also similar to items with small difference, here
there is ```discovery``` term in the key name. The only new thing here
is ```keep lost resources period```, which is responsible to remove all
the ```items```, ```triggers```, and every thing related to removed data
that we uesed to monitor.

Consider the following example:

```sql
SELECT * FORM DATABASE WHERE 'status' = 'falied'
```

As far as the query we made returns different values ,according to the 
situation, some items, triggers and etc. should remove, LLD do this job
for us.

discovery retun values follow special format. It's a JSON which has
```$KEY``` as its key to use them as macro in zabbix. If you want to
write your own custom script you should make the return value like this.

in ```portotype``` section for each field in discovery section you can
define dynamic items or other things with the help of return macros.

**Note:** It's a better practice to optimize your query with propar
condition instead of preporccess data in zabbix, becaues it can reduce
the proccess pressure on the database.

## Tmplate vs Discovey
In temple we create static items then link them to our hosts, but in
discovery all we create everything dynamically

**Note:** In template you can also define discovery rules to create dynamic items too.

## Network discovery
Under ```Configuration```, there is ```Descovery```  section where you
can define the range of your hsots to add to zabbix server
automatically. Its like:
```bash
192.168.1.2-50,192.168.100.4-90
```
There are some other configurations to find your desiered host more
accurately that they are mentioned bellow:
* ```Update interval```
* ```Checks``` - Here you can check if a specific port/service on the host is in
	open added it to server.

You need to define an ```action``` for found hosts inorder to use/show
them in ```Configuration/hosts```. 

## Actions
Action do something when they get specific data from host, discovery, 
items, and etc.

**Note:** On the top left hand of ```Action``` page you can select which
types of action do you want to use.

Types of action:

1. Trigger action
1. Discovey action 
1. Autogeneration action
1. Internal action

### Action fields

1. ```Name```
1. ```Condition``` - Type of event that action performs based on that
	 like, for ```Trigger actions``` there are, item, trigger, trigger
	 sensitivty, and etc.  ther are different conditions according to the
	 type of Action you pick.
1. operation - it has serveral options
	1. Send message
	1. Remote command
	1. Add/Remove host
	1. Add/Remove hostgroup
	1. Link/Unlink to template
	1. Enable/disable host
	1. set host inventory mode

## Auto registration
In used in active monitoring. Each agent introduce itself to zabbix
server and it will be add automatially to the zabbix server.

# Session 11

## Services

We can monitor availability of a service in zabbix based on items we are
monitoring. Under ```configuration``` there is ```Services``` section
that you can calculate ```SLA``` there for your serivce.

1. Assign a proper name
1. Select caculatoin algorthim according to your contract
1. Select ```SLA``` availability.
1. Select Triggers which you want to calculate availability of your
	 service based on that.

You can also add dependecy to other services and bind the proper time
that the service should be run over that period, you can add the valid
downtime there too. 

**Note:** If you set tigger on service(parent) you cannot add trigger to
its childs at the same time. So we set the triggers on final childs, the
leat of the tree but we can set ```SLA``` on paretns

**Note:** Only triggers higher then ```warning``` sensitivty will be considered in ```SLA```
calculation, the ```information``` jsut display in services and ```Not
classified``` will be ignored 

Need to know terms:
* KPI
* SLA
* Downtime
	* Contorlled: downtime that happens in deploying service or situations
		like that, thats totally expectable and it doesn't count in outage.

## Log file monitoring
It's a kind of agentless monitoring method. Zabbix is not a log
analyzing tool but we can monitor logs with it, in fact we tell the
zabbix to pick a specific factor from a log file and monitor that,
nothing else more. There are two types of log monitoring:

### Monitoring a singe file
In some cases some services/applications store thier data in a single
file 

To monitor logs we need to create an item with ```Zabbix
agent(active)``` and ```log``` key, then pick ```log``` for type of
information. Update interval will check for new log in the given time
period. 

log key instruction
```bash
log[PATH,"PATTERN",,,skip/all]
```

**Note:** You need to reload config cache on server:
```bash
zabbix_server -R config_cache_reload
```

#### Useful trigger functions for log monitoring
1. ```logeventid```
1. ```logsimularity```
1. ```logsource```
1. ```str```
1. ```strlength```
1. ```regex```
1. ```iregex```

# Session 12

**Note:** If you want to use ```docker``` command in ```crontab``` **you
should not use** ```-it```.

## Define Regex
Under ```Administration/General``` section, n the left had side drop
down menu under ```Regular expressions```, you can define your custom
regexs. You can use your custom regexs as pattern in log items.

## Monitor log files with time stamp
In some cases there is a log file per day or any other time stamps for a
server/application, take logrotate version mode as an example, in these
cases we should use ```logrt``` as key in item creation. You need to
pass file name regex as the fires argument.

**Note:** In log key creation there is a ```output``` section that you
can use it te extract special group from regex, for example:
```bash
This is a ([0-9][a-z]+) and ([A-Z]{3})
```
if we use ```\1``` in ```output```, zabbix consider the first group,
```([0-9][a-z]+)``` matched value as output. It will be helpful in
drawing graphs.

**Note:** If you want to count the number of occurrence of a string in a
log file you should use ```log.count``` or ```logrt.count``` as key in
item creation.

**Note:** You can write your desired log time format, match with the one
exists in log file, in item creation for log monitoring items.

## Grafana and Zabbix

You need to have grafana installed on your zabbix server, then install
grafana plugin on your server.


### Install Grafana

Add Grafana repo to the system
```bash
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-8.5.3-1.x86_64.rpm
sudo yum install grafana-enterprise-8.5.3-1.x86_64.rpm
```
Reload systemd
```bash
systemctl daemon-reload
```
Enable and start Grafana service
```bash
systemctl enable grafana-server
systemctl start grafana-server
```
Check the status
```bash
systemctl status grafana-server
```
Open port on firewall
```bash
firewall-cmd --add-port=3000/tcp --permanent
firewall-cmd --reload
```
Login to the web interface by ```admin:admin``` username:password.

### Install Zabbix plugin on Grafana

From the left hand side menu in the web interface select the gear
icon(configuration), then plugins. By defualt Zabbix plugin is not
pre-installed in Grafana, so click on ```Find more plugins on
Grafana.com``` and search for Zabbix plugin.

**Note:** Use [shecan](https://shecan.ir) to proceed the installation
process.

```bash
grafana-cli plugins install alexanderzobnin-zabbix-app
systemctl restart grafana-server
```
Go to Grafana web user interface and enable Zabbix plugin from the
plugins section.

### Connect Zabbix to Grafana
From configuration section select ```Data Sources```, click on ```Add
data source```, then select zabbix. Config fields:
1. Name
1. URL - Zabbix ```api``` URL. Follow the filed hint, just replce the
	 sever address with yours.
1. Access - The way you want to connect to zabbix
1. Auth - The security configuration
1. Custom HTTP Headers - Web server configuration
1. Zabbix API details - You need to create a username in zabbix for
	 graphana
	 1. In ```Administration/User Group``` in zabbix, create new group
	 1. Pick a good name like ```API Visualization```
	 1. From the ```Permission``` tab set permission on ```read```
	 1. Click on ```select``` and check all the options
	 1. Check ```Include all subgroups```, then click on ```add```
			hyperlink
	 1. Click on ```Add``` button
	 1. Disable ```Frontend access```
	 1. Under ```Administration/Users```, click on ```create user```
	 1. Selecet Group on ```API Visualization```
	 1. Pick a password
	 1. Click on ```Add``` button
1. Direct DB Connection - You need to add the proper plugin according to
	 your database and add it as a new data source then configure it. With
	 this option Grafana just get the query command from zabbix then get
	 the data from database directly. It's a good prctice for a larg
	 amount of data.
1. Alerting
1. Click on ```Save & Test```, it should pop a green toast
1. From ```Dashboard``` section, select your desier dashboard to view	

### Creating custom dashboard 

From the left side pannel click on ```+``` icon, then select ```create
dashboard```. Create your desired pannel and add it, then pick a data
source and choose your monitoring factors.

**Note:** You can find the ```item id``` by going to the graph of each
item, in latest data section, and from the URL. The numbers at the end of the URL is the item
id.

**Note:** You can find the ID of any thing by gonig to the configuration
section of it, from the URL.


### Alerting
You can set alert rules and notification channel. With this feature you
can send rend informations like renderd picture of the graph to
configured channel like Email, Telegram, Slack, and etc.

# Session 13

## Zabbix proxy

We use Zabbix proxy in two types of scenarios:
* There are some nodes in internet in on a same network
	* Pros:
		* We don't need to stablish separate connection for each host to
			connect to the server(port forwarding, opening port)
		* The gathered data from host will cache for a while in zabbix proxy
			so if we have connection problem there will be no data lost
		* Increase network security and performance
* There are a large number of hosts in the our zabbix server zone
	* Pros:
		* Zabbix server don't waste its proccess power on gathering data and
			handeling them, it jsut proccess the data

**Note:** We use Zabbix server in our data center and zabbix proxy
any elsewhere that we need to monitor.

## Install Zabbix Proxy

First of all you need to add ```epel-release``` and zabbix
repositories on your system.

You can install zabbix proxy with a variety of database options, here we
chose sqlite

```bash
yum install zabbix-proxy-sqlite3
```

Open zabbix port on firewall

```bash
firewall-cmd --add-port={10050/tcp,10051/tcp} --permanent
firewall-cmd --reload
```

It's a good practice to choose a good hsotname like
```bash
hostnamectl set-hostname zabbix-proxy
```
Initiate sqlite database:
```bash
mkdir /opt/zabbix
zcat /usr/share/doc/zabbix-proxy-sqlite3*/create.sql.gz | sqlite3
/opt/zabbix/zabbix.db
chown -R zabbix:zabbix /opt/zabbix
```

Now configure ```zabbix_proxy.conf``` under ```/etc/zabbix/```:
1. Set Server IP
1. Set Hostname
1. Set DBNaem - DBNaem=/opt/zabbix.db

Start and enable zabbix proxy
``` bash
systemctl start zabbix-proxy
```
### Active vs Passive zabbix proxy
The difence is in configuration data. In active mode zabbix proxy get
config data, like hosts config, from the server and load them, But in
passive zabbix server will send the config data to the proxy

### Add zabbix proxy in zabbix server
Under ```Administration/proxies```, click on ```create proxy```. The
proxy name should be the exact name which you puth in your
```zabbix_proxy.conf``` as its hostname. In active mode ony hostname is
mandatory but it's a better practice to write the IP address(for
security considerations). In passive mode you need to get all the
necessary informations. To monitor a host by proxy you need to select
proper proxy for that host in its configuration page. 

There are two stratagies in usings zabbix proxy
1. The hosts we want to monitor are only reachable through the
	 proxy, zabbix proxy is at the edge and all nodes are in the zone.
1. Choose the closest zabbix proxy, the one has the minimun cost and
	 maximun performance.

**Note: ** You need to put **Zabbix proxy IP adrress** as the server ip
in all zabbix agents.

**Note: ** If you used ```zabbix_sender``` in your agent you need to
change the IP of reciver to zabbix proxy.

**Note: ** If you made changes while the zabbix proxy is running you can
reload its cache configuration by the following command.
```bash
zabbix_proxy -R config_cache_reload
```

**Note: ** To store data locally in zabiix proxy you need to chage this
option.
```bash
ProxyLocalBuffer=HOURE
```

## Queue
It's the list of item that zabbix server expect to get from its items. 
The more ```0```s you see in the queue the better performance you have.	

## Docker and zabbix
Prerequisites:
```bash
yum install -y epel-release yum-urils ntpd 
```

Enabling ntpd

```bash
systemctl start ntpd
systemctl enable ntpd
```

Adding docker repository
```bash
yum-config-manager --add-repo
https://download.docker.com/linux/centos/docker-ce.repo
```

Insatlling docker
```bash
yum install docker-ce docker-ce-cli containerd.io -y
```

Enabling docker
```bash
systemctl start docker
systemctl enable docker
```
Pull related packages

```bash
docker pull mariadb/server
docker pull zabbix/zabbix-agent
docker pull zabbix/zabbix-server-mysql
docker pull zabbix/zabbix-web-nginx-mysql
```
Create external storage for containers
```bash
mkdir -p /docker/mysql-data
mkdir -p /docker/mysql-grafana 
mkdir -p /usr/lib/zabbix/externalscripts
```
**Tip:** Read about Network Defined Network (SDN) in docker

Create a sparate network for our containers
```bash
docker network create --subnet 172.20.0.0/16 --ip-rage 172.20.240.0/20
zabbix-net
```
run a mariadb container
```bash
docker run --name mariadb-server -t -e MYSQL_DATABASE="zabbix" -e \
MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbixpass" -e \
MYSQL_ROOT_PASSWORD="rootpass" -e TZ="Asia/Tehran" -v \
/docker/mysql-data:/var/lib/mysql --network=zabbix-net -d mariadb-server \
--character-set-server=utf8 --collation-server=utf8_bin \
--default-authentication-plugin=mysql_native_password
```
run zabbix mysql server 
```bash
docker run --name zabbix-server-mysql -t -e \
DB_SERVER_HOST="mariadb-server" -e MYSQL_DATABASE="zabbix" -e \
MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbixpass" -e \
MYSQL_ROOT_PASSWORD="rootpass" -e TZ="Asia/Tehran" -v \
/usr/lib/zabbix/externalscripts:/usr/lib/zabbix/externalscripts:rw \
-p 10051:10051 --restart unless-stop --network=zabbix-net -d \
zabbix/zabbix-server-mysql
```
run zabbix web server
```bash
docker run --name zabbix-web-nginx-mysql -t -e \
DB_SERVER_HOST="mariadb-server" -e MYSQL_DATABASE="zabbix" -e \
MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbixpass" -e \
MYSQL_ROOT_PASSWORD="rootpass" -e TZ="Asia/Tehran" -e \
PHP_TZ="Asia/Tehran" -e ZBX_SERVER_HOST="zabbox-server-mysql" -p 80:8080 --restart unless-stop --network=zabbix-net -d \
zabbix/zabbix-web-nginx-mysql
```

run zabbix agent
```bash
docker run --name zabbix-agent --network=zabbix-net -e \
ZBX_SERVER_HOST="zabbix-server-mysql" -e ZBX_HOSTNAME="zabbix server - \
with docker" -e TZ="Asia/Tehran" -d zabbix/zabbix-agent
```

get zabbix agent ip and replace int with ```172.0.0.1```
```bash
docker ps -a
docker inspect ID_OF_DOCKER_IMAGE | grep \"IPAddress
```

run grafana

```bash
docker run -d -p 3000:3000 --name=grafana --network=zabbix-net -v \
/docker/grafana:/var/lib/grafana:rw -e \
"GF_INSTALL_PLUGINS=alexanderzobnin-zabbix-app" grafana/grafana
```
**Note:** If you faced with any erros in ```docker ps```, read log of
the container that cuases problem
```bash
docker logs CONTAINER_ID
```
To add zabbix as data source in grafana you need to get
```zabbix-web-nginx-mysql```, ```zabbix-net``` ip and put in graphana
config.
```bash
docker inspect CONTAINER_ID | grep \"IPAddress
```

**Note:** You can resolve any host in a same docker network by name, ex:
```bash
ping zabbix-web-nginx-mysql
```

**Note: Please consider you have to use **container port**, which is
8080 here, in grafana config not the exposed one, 80

**Tips:** You can find useful docker files [here](https://github.com/zabbix/zabbix-docker)

# Session 14

## Tuining

From all zabbix server monitored data, ```Utilizations``` are more
important and give us an overview about zabbix performance. It's a good
solution to use Grafana to monitor zabbix server data and make
decissions for optimizing your server. 

### Understand ```zabbix_server.conf``` deeper

At least you should read whole the configuration file once to grasp an
overview about how things work in zabbix. Here there are some
considerable options:

* Listen port for trapper - default port is 10051
* Source IP - determine which NIC to use in case of having multiple NICs
* DBSocket - use linux socket instead of put DB username and password in
	config file for more security
* HistoryStorageURL, HistoryStorageDateIndex, HistoryStorageType - used
	to store history in ```elasticsearch```, it doesn't store any trends
* ExportDir, ExportFileSize - You can export 	events, history and trends
	in JSON format
* StartPollers - the number of pre-forked instances of pollers. You
	should increse that number if you have a lot of items or triggers, of
	cource the correct practice is monitoring the ```Utilization of poller
	data collectore proccesses``` parameter and according to your hardware
	resourcees.
* JavaGateway - Zabbix has an external component called ```Zabbix java
	gateway``` which allows you to monitor java applications with
	```JMX``` port
* VMwareCollectors - It's used for monitoring ```ESXi```
* VMwareCacheSize - 
* HousekeepingFrequency - How often zabbix remove outdated data from
	database

**Note:** If you want to monitor other zabbix servers form main one you
should in all zabbix servers, except the main one, in
```StatsAllowedIP=``` put the main zabbix server's ip address, and also
you need to select ```App Remote zabbix server``` as template for the
zabbix server you want to moitor in the main one.

### Prtitioning database
To imporve zabbix performance we can partition data on database, based
on day, which means database does not need to make query on all data, it
just apply the query on the partition we give. To achive this goal we
need to define some procedures in the database.

1. Connect to ```zabbix``` database with ```MySql workbench```, if you are
	 using different database, follow the related instruction.
	 * Connect over ```ssh``` if you disabled root remoate connection for
		 your databse
1. Select ```zabbix``` schema
1. Run the following query(Strongly recommand to do [this](https://bestmonitoringtools.com/zabbix-partitioning-tables-on-mysql-database/)) instead
```mysql
DELIMITER $$
CREATE PROCEDURE `partition_create`(SCHEMANAME varchar(64), TABLENAME varchar(64), PARTITIONNAME varchar(64), CLOCK int)
BEGIN
        /*
           SCHEMANAME = The DB schema in which to make changes
           TABLENAME = The table with partitions to potentially delete
           PARTITIONNAME = The name of the partition to create
        */
        /*
           Verify that the partition does not already exist
        */

        DECLARE RETROWS INT;
        SELECT COUNT(1) INTO RETROWS
        FROM information_schema.partitions
        WHERE table_schema = SCHEMANAME AND table_name = TABLENAME AND partition_description >= CLOCK;

        IF RETROWS = 0 THEN
                /*
                   1. Print a message indicating that a partition was created.
                   2. Create the SQL to create the partition.
                   3. Execute the SQL from #2.
                */
                SELECT CONCAT( "partition_create(", SCHEMANAME, ",", TABLENAME, ",", PARTITIONNAME, ",", CLOCK, ")" ) AS msg;
                SET @sql = CONCAT( 'ALTER TABLE ', SCHEMANAME, '.', TABLENAME, ' ADD PARTITION (PARTITION ', PARTITIONNAME, ' VALUES LESS THAN (', CLOCK, '));' );
                PREPARE STMT FROM @sql;
                EXECUTE STMT;
                DEALLOCATE PREPARE STMT;
        END IF;
END$$
DELIMITER ;

DELIMITER $$
CREATE PROCEDURE `partition_drop`(SCHEMANAME VARCHAR(64), TABLENAME VARCHAR(64), DELETE_BELOW_PARTITION_DATE BIGINT)
BEGIN
        /*
           SCHEMANAME = The DB schema in which to make changes
           TABLENAME = The table with partitions to potentially delete
           DELETE_BELOW_PARTITION_DATE = Delete any partitions with names that are dates older than this one (yyyy-mm-dd)
        */
        DECLARE done INT DEFAULT FALSE;
        DECLARE drop_part_name VARCHAR(16);

        /*
           Get a list of all the partitions that are older than the date
           in DELETE_BELOW_PARTITION_DATE.  All partitions are prefixed with
           a "p", so use SUBSTRING TO get rid of that character.
        */
        DECLARE myCursor CURSOR FOR
                SELECT partition_name
                FROM information_schema.partitions
                WHERE table_schema = SCHEMANAME AND table_name = TABLENAME AND CAST(SUBSTRING(partition_name FROM 2) AS UNSIGNED) < DELETE_BELOW_PARTITION_DATE;
        DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;

        /*
           Create the basics for when we need to drop the partition.  Also, create
           @drop_partitions to hold a comma-delimited list of all partitions that
           should be deleted.
        */
        SET @alter_header = CONCAT("ALTER TABLE ", SCHEMANAME, ".", TABLENAME, " DROP PARTITION ");
        SET @drop_partitions = "";

        /*
           Start looping through all the partitions that are too old.
        */
        OPEN myCursor;
        read_loop: LOOP
                FETCH myCursor INTO drop_part_name;
                IF done THEN
                        LEAVE read_loop;
                END IF;
                SET @drop_partitions = IF(@drop_partitions = "", drop_part_name, CONCAT(@drop_partitions, ",", drop_part_name));
        END LOOP;
        IF @drop_partitions != "" THEN
                /*
                   1. Build the SQL to drop all the necessary partitions.
                   2. Run the SQL to drop the partitions.
                   3. Print out the table partitions that were deleted.
                */
                SET @full_sql = CONCAT(@alter_header, @drop_partitions, ";");
                PREPARE STMT FROM @full_sql;
                EXECUTE STMT;
                DEALLOCATE PREPARE STMT;

                SELECT CONCAT(SCHEMANAME, ".", TABLENAME) AS `table`, @drop_partitions AS `partitions_deleted`;
        ELSE
                /*
                   No partitions are being deleted, so print out "N/A" (Not applicable) to indicate
                   that no changes were made.
                */
                SELECT CONCAT(SCHEMANAME, ".", TABLENAME) AS `table`, "N/A" AS `partitions_deleted`;
        END IF;
END$$
DELIMITER ;

DELIMITER $$
CREATE PROCEDURE `partition_maintenance`(SCHEMA_NAME VARCHAR(32), TABLE_NAME VARCHAR(32), KEEP_DATA_DAYS INT, HOURLY_INTERVAL INT, CREATE_NEXT_INTERVALS INT)
BEGIN
        DECLARE OLDER_THAN_PARTITION_DATE VARCHAR(16);
        DECLARE PARTITION_NAME VARCHAR(16);
        DECLARE OLD_PARTITION_NAME VARCHAR(16);
        DECLARE LESS_THAN_TIMESTAMP INT;
        DECLARE CUR_TIME INT;

        CALL partition_verify(SCHEMA_NAME, TABLE_NAME, HOURLY_INTERVAL);
        SET CUR_TIME = UNIX_TIMESTAMP(DATE_FORMAT(NOW(), '%Y-%m-%d 00:00:00'));

        SET @__interval = 1;
        create_loop: LOOP
                IF @__interval > CREATE_NEXT_INTERVALS THEN
                        LEAVE create_loop;
                END IF;

                SET LESS_THAN_TIMESTAMP = CUR_TIME + (HOURLY_INTERVAL * @__interval * 3600);
                SET PARTITION_NAME = FROM_UNIXTIME(CUR_TIME + HOURLY_INTERVAL * (@__interval - 1) * 3600, 'p%Y%m%d%H00');
                IF(PARTITION_NAME != OLD_PARTITION_NAME) THEN
                        CALL partition_create(SCHEMA_NAME, TABLE_NAME, PARTITION_NAME, LESS_THAN_TIMESTAMP);
                END IF;
                SET @__interval=@__interval+1;
                SET OLD_PARTITION_NAME = PARTITION_NAME;
        END LOOP;

        SET OLDER_THAN_PARTITION_DATE=DATE_FORMAT(DATE_SUB(NOW(), INTERVAL KEEP_DATA_DAYS DAY), '%Y%m%d0000');
        CALL partition_drop(SCHEMA_NAME, TABLE_NAME, OLDER_THAN_PARTITION_DATE);

END$$
DELIMITER ;


DELIMITER $$
CREATE PROCEDURE `partition_verify`(SCHEMANAME VARCHAR(64), TABLENAME VARCHAR(64), HOURLYINTERVAL INT(11))
BEGIN
        DECLARE PARTITION_NAME VARCHAR(16);
        DECLARE RETROWS INT(11);
        DECLARE FUTURE_TIMESTAMP TIMESTAMP;

        /*
         * Check if any partitions exist for the given SCHEMANAME.TABLENAME.
         */
        SELECT COUNT(1) INTO RETROWS
        FROM information_schema.partitions
        WHERE table_schema = SCHEMANAME AND table_name = TABLENAME AND partition_name IS NULL;

        /*
         * If partitions do not exist, go ahead and partition the table
         */
        IF RETROWS = 1 THEN
                /*
                 * Take the current date at 00:00:00 and add HOURLYINTERVAL to it.  This is the timestamp below which we will store values.
                 * We begin partitioning based on the beginning of a day.  This is because we don't want to generate a random partition
                 * that won't necessarily fall in line with the desired partition naming (ie: if the hour interval is 24 hours, we could
                 * end up creating a partition now named "p201403270600" when all other partitions will be like "p201403280000").
                 */
                SET FUTURE_TIMESTAMP = TIMESTAMPADD(HOUR, HOURLYINTERVAL, CONCAT(CURDATE(), " ", '00:00:00'));
                SET PARTITION_NAME = DATE_FORMAT(CURDATE(), 'p%Y%m%d%H00');

                -- Create the partitioning query
                SET @__PARTITION_SQL = CONCAT("ALTER TABLE ", SCHEMANAME, ".", TABLENAME, " PARTITION BY RANGE(`clock`)");
                SET @__PARTITION_SQL = CONCAT(@__PARTITION_SQL, "(PARTITION ", PARTITION_NAME, " VALUES LESS THAN (", UNIX_TIMESTAMP(FUTURE_TIMESTAMP), "));");

                -- Run the partitioning query
                PREPARE STMT FROM @__PARTITION_SQL;
                EXECUTE STMT;
                DEALLOCATE PREPARE STMT;
        END IF;
END$$
DELIMITER ;

DELIMITER $$
CREATE PROCEDURE `partition_maintenance_all`(SCHEMA_NAME VARCHAR(32))
BEGIN
                CALL partition_maintenance(SCHEMA_NAME, 'history', 7, 24, 3);
                CALL partition_maintenance(SCHEMA_NAME, 'history_log', 7, 24, 3);
                CALL partition_maintenance(SCHEMA_NAME, 'history_str', 7, 24, 3);
                CALL partition_maintenance(SCHEMA_NAME, 'history_text', 7, 24, 3);
                CALL partition_maintenance(SCHEMA_NAME, 'history_uint', 7, 24, 3);
                CALL partition_maintenance(SCHEMA_NAME, 'trends', 365, 24, 3);
                CALL partition_maintenance(SCHEMA_NAME, 'trends_uint', 365, 24, 3);
END$$
DELIMITER ;
```

**Note:** You need to keep the last to numbers of each procedure the
same.

1. Once you create all your procedures run ```partition_maintenance```
	 under ```stored procedures``` section, pass ```zabbix``` as schema
	 name and click ```no``` on printed error.
	
**Note:** It's a good practice to partition zabbix immediately after
```zcat``` zabbix sql.

**Note:** Hosuekeeping can push extra load on database, with the help of
partitioning we can get ride of this problem.

### Elasticsearch and Zabbix

To improve the performance we can store hostiry data in elasticsearch,
in ```zabbix_server.config``` write the IP address of elasticsearch in
```HistoryStorageURL``` field, then select the Histoy type you want to
store. Also you need to config zabbix frontend to get logs from
elasticsearch, for this purpose open
```/etc/zabbix/web/zabbix.conf.php```  file and add the following lines
```php
// add to the first of the file
global $DB, $HISTORY;
// add in elastic section
$HISTORY['url'] = 'http://ELASTIC_IP:9200'
$HISTORY['type'] = ['uint', 'dbl', 'str', 'text', 'log']
```
Installin elasticsearch

```bash
yum install -y elasticsearch libcurl
```

Start and enable elasticsearch service
```bash
systemctl daemon-reload
systemctl start elasticsearch
systemctl enable elasticsearch
```
get status
```bash
curl -X GET "localhost:9200/?pretty"
```
Create mapping(sort of table) peer to the history tables like below (Complete guide [here](https://www.zabbix.com/documentation/current/ua/manual/appendix/install/elastic_search_setup))
```bash
curl -X PUT \
http://your-elasticsearch.here:9200/text \
-H 'content-type:application/json' \ 
-d '{    
	"settings": {       
		"index": {          
			"number_of_replicas": 1,          
			"number_of_shards": 5       
			}    
		},    
		"mappings": {       
			"properties": {          
				"itemid": {             
					"type": "long"          
					},          
					"clock": {             
						"format": "epoch_second",             
						"type": "date"          
					},          
					"value": {             
						"fields": {                
							"analyzed": {                   
								"index": true,                   
								"type": "text",                   
								"analyzer": "standard"                
							}             
						},             
						"index": false,             
						"type": "text"          
					}       
				}    
			} 
		}'
```

#### Important tables in zabbix database

* ```history``` - It stores float data
* ```history_log``` - It stores log data
* ```history_str``` - It stores web scenario errors or outpots * ```history_text``` - It stores text data
* ```history_uint``` - It stores unsigned intigers
* ```trends``` - It stores float trends
* ```trends_uint``` - It stores unsigned intiger trends

