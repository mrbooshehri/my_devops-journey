# Install mysql 5.7
1. Change your dns to [shecan](https://shecan.ir)
```bash
vi /etc/resolve.conf
```
add one of the addresses at the begining the file
```bash
namseserver 185.51.200.2
namseserver 178.22.122.100
```
1. Add repository
```bash
yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
```
1. Insatll mysql
```bash
yum install mysql-community-server
```


