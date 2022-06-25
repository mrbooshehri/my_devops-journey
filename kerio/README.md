# Install Kerio on Centos

1. Download the latest version of ```kerio connect``` from [here](http://download.kerio.com/archive/index.php)
1. Install the downloaded package
``` bash
yum install kerio-connect-VERSION-linux-x86_64.rpm
```
1. Start the service and check the status
```bash
systemctl enable --now kerio-connect
systemctl status kerio-connect
```
1. Open port on firewall
```bash
firewall-cmd --add-port=4040/tcp --permanent
firewall-cmd --reload
```
1. Login to the admin pannel via ```https://your_CentOS_IP:4040/admin```

# Install KerioOS
