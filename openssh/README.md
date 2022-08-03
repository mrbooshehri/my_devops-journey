# How to install openssh 9 on Centos 7

1. Upgrade your ```openssh``` to the latest version
```bash
yum update openssh -y
systemctl restart sshd
```

2. Get the latest version of ```openssl``` and ```openssh```
```bash
wget https://ftp.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-9.0p1.tar.gz
wget https://ftp.openssl.org/source/openssl-1.1.1g.tar.gz
```

3. Install telnet [Optional]

In case you don't access to your machine install telnet ass backup connection method

Installation
```bash
yum install telnet telnet-server -y
```
Start up
```bash
systemctl enable telnet.socket
systemctl start telnet.socket
```
Connection
```bash
useradd telnetuser
passwd telnetuser
# to check the telnet connection
telnet 127.0.0.1
```

4. Upgrade ```openssl```

Backup
```bash
mv /usr/bin/openssl /usr/bin/openssl_old
```
Install
```bash
tar xzvf openssl-1.1.1g.tar.gz
cd openssl-1.1.1g/
./config shared && make && make install
```

Configure soft connection
```bash
ln -s /usr/local/bin/openssl /usr/bin/openssl
```
If implementedopenssl versionReport the following error
```bash
openssl: error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory
```
Execute the following command to solve the problem:
```bash
ln -s /usr/local/lib64/libssl.so.1.1 /usr/lib64/
ln -s /usr/local/lib64/libcrypto.so.1.1 /usr/lib64/
```
Check version
```bash
openssl_old version
openssl version
```
5. Upgrade openssh

Install required dependencies
```bash
yum install zlib-devel  openssl-devel  pam-devel -y
```
Backups
```bash
mkdir /etc/ssh_old
mv /etc/ssh/* /etc/ssh_old/
```
Decompress, compile and install
```bash
tar xzvf openssh-9.0p1.tar.gz
cd openssh-8.2p1/
./configure --prefix=/usr/ --sysconfdir=/etc/ssh --with-ssl-dir=/usr/local/lib64/ --with-zlib --with-pam --with-md5-password --with-ssl-engine --with-selinux
make && make install
ssh -V
```
Configure
```bash
vim /etc/ssh/sshd_config
#Example: configure root login according to your previous configuration
PermitRootLogin yes
```

Start up
```bash
mv /usr/lib/systemd/system/sshd.service /etc/ssh_old/sshd.service
mv /usr/lib/systemd/system/sshd.socket /etc/ssh_old/sshd.socket
cp -a contrib/redhat/sshd.init /etc/init.d/sshd
systemctl daemon-reload
/etc/init.d/sshd restart
chkconfig --add sshd
chkconfig sshd on
```

Tags:
```
#Linux #openssh #centos7
```

Related:
```
* https://developpaper.com/upgrade-centos7-openssh-to-the-latest-version/
```

