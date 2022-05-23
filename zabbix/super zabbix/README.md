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
will get some of data accorting to the template it has and put them on the list.
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
3:32:29
