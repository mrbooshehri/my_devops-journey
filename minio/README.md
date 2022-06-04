Table of Contents
=================

* [MinIO](#minio)
   * [What is MinIO](#what-is-minio)
   * [How to install MinIO](#how-to-install-minio)
   * [Example scenarios](#example-scenarios)
      * [Single server with 1 disk](#single-server-with-1-disk)
      * [Single server with 4 disks](#single-server-with-4-disks)


# MinIO

## What is MinIO

MinIO is a High Performance Object Storage released under GNU Affero General Public License v3.0. It is API compatible with Amazon S3 cloud storage service. Use MinIO to build high performance infrastructure for machine learning, analytics and application data workloads.

## How to install MinIO

1. Get the latest version
```bash
wget https://dl.minio.io/server/minio/release/linux-amd64/minio
```
2. Set the permission
```bash
chmod u+x minio
```
3. Set the ownership
```bash
chown root:root minio
```
4. Move it to the following directory
```bash
mv minio /usr/local/bin
```
5. Create a user for minio
```bash
useradd minio-user
```
6. Create a directory
```bash
mkdir /tmp/minio
```
7. Set the ownership for the minio directory
```bash
chown -R minio-user:minio-user /tmp/minio
```
8. Configure the minio
```bash
cat > /etc/default/minio << EOF
MINIO_VOLUMES="/tmp/minio/"
MINIO_OPTS="--address 192.168.1.102:9000 --console-address :9001"
EOF
```
**Note:** Check your IP address. You can use the following line, for
cable connections, too:
```bash
ip -br a | grep enp | awk '{print $3}' | cut -d/ -f1
```
**Note:** Minio use random port number for console, you can bind a fix
port number as we did above.

9. Configure the minio service enter into following directory
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
11. Reload systemd
```bash
systemctl daemon-reload
```
12. Start and enable the minio service
```bash
systemctl enable minio.service
systemctl start minio.service
```
13. Check the status 
```bash
systemctl status minio.service
```
14. Add minio port to the Firewall
```bash
firewall-cmd --zone=public --add-port={9000/tcp,9001/tcp} --permanent
firewall-cmd --reload
```
15. Open it in any browser
16. Default login credential 
	* Username: minioadmin
	* Password: minioadmin

## Example scenarios

### Single server with 1 disk

The default configuration will start a single server with a single disk. 

### Single server with 4 disks
Edit the configuration file like
```bash
MINIO_VOLUMES="/PATH/TO/minio/data{0...3}"
```
or
```bash
MNIO_VOLUMES="/PATH/TO/minio/foobar /PATH/TO/minio/barfoo /PATH/TO/minio/faabor /PATH/TO/minio/borfaa"
```
