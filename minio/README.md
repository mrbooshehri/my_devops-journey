# Install MinIO

## What is MinIO


## How to install MinIO

1. Get the latest version
```bash
wget https://dl.minio.io/server/minio/release/linux-amd64/minio
```
1. Set the permission
```bash
chmod u+x minio
```
1. Set the ownership
```bash
chown root:root minio
```
1. Move it to the following directory
```bash
mv minio /usr/local/bin
```
1. Create a user for minio
```bash
useradd minio-user
```
1. Create a directory
```bash
mkdir /tmp/minio
```
1. Set the ownership for the minio directory
```bash
chown -R minio-user:minio-user /tmp/minio
```
1. Configure the minio
```bash
cat > /etc/default/minio << EOF
MINIO_VOLUMES="/tmp/minio/"
MINIO_OPTS="--address 192.168.1.102:9000"
EOF
```
**Note:** Check your IP address. You can use the following line too:
```bash
ip -br a | grep enp | awk '{print $3}' | cut -d/ -f1
```
1. Configure the minio service enter into following directory
```bash
cat > /etc/systemd/system/minio.service << EOF
[Unit]
Description=Minio
Documentation=https://docs.minio.io
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/usr/local/bin/minio

[Service]
WorkingDirectory=/usr/local/

User=minio-user
Group=minio-user

PermissionsStartOnly=true

EnvironmentFile=-/etc/default/minio
ExecStartPre=/bin/bash -c "[ -n \"\${MINIO_VOLUMES}\" ] || echo \"Variable MINIO_VOLUMES not set in /etc/default/minio\""

ExecStart=/usr/local/bin/minio server \$MINIO_OPTS \$MINIO_VOLUMES

StandardOutput=journal
StandardError=inherit

#Specifies the maximum file descriptor number that can be opened by this process
LimitNOFILE=65536

#Disable timeout logic and wait until process is stopped
TimeoutStopSec=0

#SIGTERM signal is used to stop Minio
KillSignal=SIGTERM

SendSIGKILL=no

SuccessExitStatus=0

[Install]
WantedBy=multi-user.target
EOF
```
1. Reload systemd
```bash
systemctl daemon-reload
```
1. Start and enable the minio service
```bash
systemctl enable minio.service
systemctl start minio.service
```
1. Check the status 
```bash
systemctl status minio.service
```
1. Open it in any browser
1. To get credential information see the file below
```bash
cat /tmp/minio/.minio.sys/config/config.json
```
