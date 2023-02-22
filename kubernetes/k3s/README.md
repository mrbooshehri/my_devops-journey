# How to move k3s data to another location

The standard data location used for k3s is /run/k3s,
/var/lib/kubelet/pods, /var/lib/rancher. Because this directory contains
all containers/images/volumes, it can be large. So you no need to store
this in OS Volume when you can use separate data volume.

1. Stop daemon

```bash
systemctl stop k3s
systemctl stop k3s-agent
/usr/local/bin/k3s-killall.sh
```

2. Copy files to new location

```bash
mv /run/k3s/ /datadrive/k3s/
mv /var/lib/kubelet/pods/ /datadrive/k3s-pods/
mv /var/lib/rancher/ /datadrive/k3s-rancher/
```
3. Create symbolic link

```bash
ln -s /datadrive/k3s/ /run/k3s
ln -s /datadrive/k3s-pods/ /var/lib/kubelet/pods
ln -s /datadrive/k3s-rancher/ /var/lib/rancher
```

4. Start daemon

```bash
systemctl start k3s
systemctl start k3s-agent
```
