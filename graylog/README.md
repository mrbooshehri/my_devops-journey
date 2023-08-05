# Graylog

## How to configure Graylog to accept incoming syslog entries

1. Select `inputs` from `system/inputs` on the top menu

![01.jpg](./assets/01.jpg)

2. Click on `Launch new input` and fill out the information

![02.jpg](./assets/02.jpg)

* **Node:** Select the node for the hosting server.
* **Title:** syslog
* **Bind address:** 0.0.0.0
* **Port:** 5140

## How to configure your Linux clients to sent syslog information

1. Create configuration file under `/etc/rsyslog.d/`

```bash
sudo touch /etc/rsyslog.d/90-graylog.conf
```

2. Add the following configuration to the file

For UDP connection (syslogUDP):

```bash
# UDP (single @):
sudo echo "*.* @SERVER:5140;RSYSLOG_SyslogProtocol23Format" > /etc/rsyslog.d/90-graylog.conf
```

For TCP connection (syslogTCP)

```bash
# TCP (double @@):
sudo echo "*.* @@SERVER:5140;RSYSLOG_SyslogProtocol23Format" > /etc/rsyslog.d/90-graylog.conf
```

> **Note:** `SERVER` is the address of your graylog server

3. Restart rsyslog daemon

```bash
sudo systemctl restart rsyslog
```

## How to send and analyze HAProxy logs in graylog

1. Add the following config for rsyslog under `/etc/rsyslog.d/49-haproxy.conf`

```
$template GRAYLOGRFC5424,"<%PRI%>%PROTOCOL-VERSION% %TIMESTAMP:::date-rfc3339% %HOSTNAME% %APP-NAME% %PROCID% %MSGID% %STRUCTURED-DATA% %msg%\n"
$AddUnixListenSocket /var/lib/haproxy/dev/log

# Send HAProxy messages to a dedicated logfile
if $programname startswith 'haproxy' then /var/log/haproxy.log
if $syslogtag contains 'haproxy' then @192.168.68.52:5140;GRAYLOGRFC5424
&~
```

2. Add the following config for HAProxy under `/etc/haproxy/haproxy.cfg`
```
...
defaults
		...
    log-format {"haproxy_clientIP":"%ci","haproxy_clientPort":"%cp","haproxy_dateTime":"%t","haproxy_frontendNameTransport":"%ft","haproxy_backend":"%b","haproxy_serverName":"%s","haproxy_Tw":"%Tw","haproxy_Tc":"%Tc","haproxy_Tt":"%Tt","haproxy_bytesRead":"%B","haproxy_terminationState":"%ts","haproxy_actconn":%ac,"haproxy_FrontendCurrentConn":%fc,"haproxy_backendCurrentConn":%bc,"haproxy_serverConcurrentConn":%sc,"haproxy_retries":%rc,"haproxy_srvQueue":%sq,"haproxy_backendQueue":%bq,"haproxy_backendSourceIP":"%bi","haproxy_backendSourcePort":"%bp","haproxy_statusCode":"%ST","haproxy_serverIP":"%si","haproxy_serverPort":"%sp","haproxy_frontendIP":"%fi","haproxy_frontendPort":"%fp","haproxy_capturedRequestHeaders":"%hr","haproxy_httpRequest":"%r"}
		...
...
```

3. Restart rsyslog and haproxy services
```
systemctl restart rsyslog.service 
systemctl haproxy rsyslog.service 
```

4. In the graylog under `inputs` section find your input, then select
	 `Manage extractors`

5. In `Manage extractors` create new extractor. [ You can get `Massage
	 ID` and `Index` of your desierd message from `Input` section under
	 `Show received messages` in `All Messages` section ] 
	
6. Config your extractor and save

Related:
```
* https://www.techrepublic.com/article/how-to-add-clients-to-the-graylog-system-log-manager/
* https://go2docs.graylog.org/5-0/getting_in_log_data/ingest_syslog.html
* https://uysalmustafa.com/2019/05/22/sending-haproxy-logs-to-graylog/
```


