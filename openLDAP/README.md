# openLDAP terminology

* LDAP - Light weight directory access protocol (kind of storage to
	store user identities)
* It's an application protocol for querying and modifying items in
	directory services.



## LDIF fields

* dn - distinguished name
This refers to the name that uniquely identifies an entry in the
directory.
* dc - domain component
This refers to each component of the domain. For example
www.mydomain.com would be written as DC=www,DC=mydomain,DC=com
* ou - organizational unit
This refers to the organizational unit (or sometimes the user group)
that the user is part of. If the user is part of more than one group,
you may specify as such, e.g., OU= Lawyer,OU= Judge.
* cn - common name
This refers to the individual object (person's name; meeting room;
recipe name; job title; etc.) for whom/which you are querying.

# openLDAP

## What is LDAP

LDAP is Known as Lightweight Directory Access Protocol. It is used for
consolidating all the services in one directory services which will be
further accessed and managed by the LDAP Client like email client, mail
servers, web browsers. LDAP uses TCP/IP stack to access and manage the
directory services.

## What is LDIF

A LDIF(LDAP Interchange Format) file is Known as a standard text file
which can be used for configuring and storing information in LDAP
directory. This file is usually used for the addition or modification of
data inside the LDAP Directory Server based on Schema rules accepted by
the Directory.

## What is an Attribute

An attribute is like a variable which holds the value. It can be
different types based on the different values it holds just like the
variable in Programming Paradigms where it could be of type int, char,
float, double etc.

# Install openLDAP on centos 7

## Install openldap server and client
```bash
yum install openldap openldap-servers -y
yum install openldap-clients -y
```

## Start service
```bash
systemctl enable --now slapd
```

# Configuration
## Setup OpenLDAP root user password
```bash
slappasswd
```

##  Configure OpenLDAP Server
```bash
vi ldaprootpasswd.ldif
dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}PASSWORD_CREATED
```

