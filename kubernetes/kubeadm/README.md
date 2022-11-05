# Kubeadm

## Install Kubernetes cluster on VMware workstation 

### Prerequisite
1. Two new workstation machine with a Linux distro installed (here I
	 used Ubuntu 20.04)
1. Assign static IP address to each machine
1. Minimum 2GB of RAM per instance
1. Root access of each machine

### Pre-configurations

1. Update your Linux distro

```bash
sudo apt update -y
```

2. Set static IP address

```yaml
# vim /etc/netplan/00-installer-config.yaml

# For master
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
     dhcp4: false
     addresses: [192.168.1.30/24]
     gateway4: 192.168.1.1
     nameservers:
       addresses: [8.8.8.8,8.8.4.4]

# For worker
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
     dhcp4: false
     addresses: [192.168.1.31/24]
     gateway4: 192.168.1.1
     nameservers:
       addresses: [8.8.8.8,8.8.4.4]
```
3. set hostname

```bash
# For master
hostnamectl set-hostname master-node
# For worker
hostnamectl set-hostname worker-node
```

4. Add hostname of each server in ```/etc/hosts```

In both nodes
```bash
192.168.1.30  master-node
192.168.1.31  worker-node
```
5. Disable swap in both nodes
```bash
swappff -a
sudo sed -i ‘/ swap / s/^\(.*\)$/#\1/g’ /etc/fstab
# or
sed -i '/swap/s/^/#/' /etc/fstab
```
**Note:** Please check ```/etc/fstab``` to eliminate any probable
problem.

### Install Docker
You can follow the [official docker
documentation](https://docs.docker.com/engine/install/ubuntu/) to
install docker or to install it on ubuntu follow these commands:
```bash
apt-get remove docker docker-engine docker.io containerd runc
apt-get update
apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
systemctl enable --now docker
```

### Install Kubernetes
```bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
echo 'deb https://apt.kubernetes.io/ kubernetes-xenial main' | sudo tee /etc/apt/sources.list.d/kubernetes.list
apt-get update -y
sudo apt-get install -y docker.io kubelet kubeadm kubectl containerd
```

**Note:** To prevent apt from updating kubernetes packages run the
following command:
```bash
sudo apt-mark hold docker.io kubelet kubeadm kubectl 
```

**Note:** IF there was a docker permission error while starting docker.
It can be solved by below command :
```bash
sudo chmod 666 /var/run/docker.sock
```

### Configure master node
```bsah
kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=192.168.48.128
```

* ```--pod-network-cidr:``` Specify range of IP addresses for the pod
  network. If set, the control plane will automatically allocate CIDRs
  for every node.
* ```--apiserver-advertise-address:```: The IP address the API Server
  will advertise it's listening on. If not set the default network
  interface will be used.

**Note:** If you faced with ```docker.io : Depends: containerd (>=
1.2.6-0ubuntu1~) ``` error, you need to install ```containerd``` like:
```bash
sudo apt install containerd
```
**Note:** If you faced with ```kubeadm init running into issue - [ERROR
CRI]: container runtime is not running``` error, make sure you have
installed ```docker```, then run the following commands:
```bash
sudo rm /etc/containerd/config.toml
sudo systemctl restart containerd 
```
then run your ```kubeadm init``` command with all your desier flags
again. After it's done copy the output ```token``` and run it in worker
nodes

### Related
```
* https://thinkvirtualblog.wordpress.com/2021/04/25/install-kubernetes-on-vmware-workstation/
* https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/
* https://askubuntu.com/questions/1273024/docker-io-depends-containerd-1-2-6-0ubuntu1
* https://old.reddit.com/r/kubernetes/comments/utiymt/kubeadm_init_running_into_issue_error_cri/
```
