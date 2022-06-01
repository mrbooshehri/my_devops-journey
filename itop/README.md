# Itop
iTop, stands for IT Operational Portal, is an Open Source web based application for the day to day operations of an IT environment. iTop was designed with the ITIL best practices in mind but does not dictate any specific process, the application is flexible enough to adapt to your processes whether you want rather informal and pragmatic processes or a strict ITIL aligned behaviour.

This tool is ideal for Help Desk agents, Support engineers, Service managers, IT managers and End-users. Hence it is based on Apache, MySQL and PHP, so you can run it on any operating system that supports those applications like Windows, Linux, MacOS and Solaris as well.

Using iTop you can:

* Document your entire IT infrastructure assets such as servers, applications, network devices, virtual machines, contacts.. etc.
* Manage incidents, user requests, planned outages.
* Document IT services and contracts with external providers including service level agreements.
* Export all the information in a manual or scripted manner.
* Import or synchronize/federate any data from external systems.

## Prerequisites
As it relies on apache, mysql and php, we need a server that has a working LAMP stack. 

1. http
1. php < v7.4
1. mariadb < 5.7

### Install httpd

Install epel-release repo
```bash
yum install epel-release -y
```

Install and start LAMP stack

```bash
yum install httpd php mariadb-server graphviz unzip php-zip php-gd php-mysqlnd php-imap php-soap php-ldap php-mbstring php-mcrypt php-pecl-zendopcache -y
```

Ensure session directory permissions

```bash
mkdir -p /var/lib/php/session
chown -R apache:apache /var/lib/php/session
chmod -R apache:apache /var/www/itop/conf/ /var/www/itop/data/
/var/www/itop/env-production/ /var/www/itop/log/
```

## Install get iTop package and install it


