# Install redis

## Add REMI repository
```bash
yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

# Install Redis on CentOS 7 / RHEL 7
```bash
yum --enablerepo=remi install redis
```

# Start Redis Service on CentOS 7 / RHEL 7
```bash
sudo systemctl enable --now redis
```
