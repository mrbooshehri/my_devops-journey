# Clickhouse

## Installation on Debian-based

1. Install dependencies
```bash
sudo apt install apt-transport-https dirmngr ca-certificates
```

2. Add key and repository
```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv E0C56BD4
echo "deb https://repo.clickhouse.com/deb/stable/ main/" | sudo tee /etc/apt/sources.list.d/clickhouse.list
sudo apt update
```

3. Install clickhouse
```bash
sudo apt install clickhouse-server clickhouse-client -y
sudo systemctl status clickhouse-server
```

4. Change listen IP address in ```/etc/clickhouse-server/config.xml```

```bash
<listen_host>0.0.0.0</listen_host>
```

5. Enable web GUI
```bash
<http_server_default_response><![CDATA[<html ng-app="SMI2"><head><base href="http://ui.tabix.io/"></head><body><div ui-view="" class="content-ui"></div><script src="http://loader.tabix.io/master.js"></script></body></html>]]></http_server_default_response>
```

6. Restart service
```bash
systemctl restart clickhouse-server
```

7. Default username and password
	* Username: default
	* Password:[blank]

Tags:
```
#Clickhouse
```

Related:
```
* https://www.linuxcloudvps.com/blog/how-to-install-clickhouse-on-ubuntu-20-04/
```
