
Table of Contents
=================

* [Chrony](#chrony)
   * [Install chrony](#install-chrony)
   * [Configuration](#configuration)
      * [Configuration structure](#configuration-structure)
   * [Configuration](#configuration-1)
      * [Configuration Sections](#configuration-sections)
   * [Minimal Clinet Configuration](#minimal-clinet-configuration)
   * [Useful chronyc](#useful-chronyc)
   * [Configure chrony as NTP Server](#configure-chrony-as-ntp-server)
   * [Check the workflow](#check-the-workflow)
   * [Chrony force sync with NTP Server](#chrony-force-sync-with-ntp-server)

# Chrony

## Install chrony
```bash
yum install chrony
```

## Configuration
The configuration file is under ```/etc/chrony.conf```

### Configuration structure


## Configuration
The configuration file is under ```/etc/chrony.conf```

### Configuration Sections

- server hostname [option]: The NTP server which can be used as a time source. The server directive is immediately followed by either the name of the server, or its IP address.
```
server 0.centos.pool.ntp.org iburst
server 192.168.136.17	iburst
```

- pool name [option]: The syntax of this directive is similar to that for the server directive, except that it is used to specify a pool of NTP servers rather than a single NTP server. The pool name is expected to resolve to multiple addresses which might change over time.
```
pool pool.ntp.org iburst maxsources 3
```
- Interval options
	- iburst: the interval between the first four requests sent to the server will be 2 seconds or less 
	- minpoll: allows chronyd to make the first update of the clock shortly after start.

- makestep: If the system clock can be far from the true time after boot for any reason, chronyd should be allowed to correct it quickly by stepping instead of slewing, which would take a very long time. The makestep directive does that.
```
makestep 1000 10 
```
This would step the system clock if the adjustment is larger than 1000 seconds, but only in the first ten clock updates.

- driftfile: To stabilise the initial synchronisation on the next start, the estimated drift of the system clock is saved to a file specified by the driftfile directive.

- rtcsync: The system time is periodically copied to the RTC and chronyd does not try to track its drift. This will inform the kernel the system clock is kept synchronized and the kernel will update the real-time clock every 11 minutes.

## Minimal Clinet Configuration

```bash
pool pool.ntp.org iburst
# server 192.168.136.17 iburst
driftfile /var/lib/chrony/drift
makestep 10 3
rtcsync
```
then start the service
```bash
systemctl status chronyd
systemctl enable chronyd
systemctl start chronyd
```

## Useful ```chronyc```

- ```chronyc sources```: This command displays information about the current time sources that chronyd is accessing.

- ```chronyc sources -v```: With -v you get a more verbose output. In this case, extra caption lines are shown as a reminder of the meanings of the columns.

- ```chronyc tracking```: The below tracking command displays parameters about the system’s clock performance

- ```chronyc activity```: It reports the number of servers and peers
	that are online and offline. If the auto_offline option is used in
	specifying some of the servers or peers, the activity command can be
	useful for detecting when all of them have entered the offline state
	after the network link has been disconnected.

- ```chronyc serverstats```: It tells how busy your server is

## Configure chrony as NTP Server

To configure chrony as NTP Server you just need to add an ```"allow"```
directive to the ```/etc/chrony.conf``` file in order to open the NTP port and allow chronyd to reply to client requests.

## Check the workflow

```bash
tcpdump -i INTERFACE_NAME port 123
```

**Note:** After configuring both client and server, restart ```chronyd``` 
in them
```bash
systemctl restart chronyd
```
## Chrony force sync with NTP Server

chronyd does not step the clock by default, but the default chrony.conf file provided in the chrony package allows steps in the first three updates of the clock. After that, all corrections are made slowly by speeding up or slowing down the clock. The chronyc makestep command can be issued to force chronyd to step the clock at any time.

If you wish to provide a custom NTP Server for chrony force sync with NTP Server, follow below syntax:

```bash
chronyd -q "server 192.168.0.113 iburst"
```
