# Install mongodb

## Add repo for version 5
```bash
sudo tee /etc/yum.repos.d/mongodb-org-5.0.repo<<EOF
[mongodb-org-5.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/\$releasever/mongodb-org/5.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-5.0.asc
EOF
```

## Add repo for version 4.4
```bash
sudo tee /etc/yum.repos.d/mongodb-org-4.4.repo<<EOF
[mongodb-org-4.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/7Server/mongodb-org/4.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc
EOF
```

## Install mongo
```bash
yum install mongodb-org
```

## Enable and start service

```bsah
systemctl enable --now monogd
```

Related:
```
* https://www.itzgeek.com/how-tos/linux/centos-how-tos/how-to-install-latest-mongodb-2-6-4-on-centos-7-rhel-7.html
```
