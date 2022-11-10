# Certified Kubernetes Administration (CKA)

**Topics**
* Install and configuration
* Core concepts(Architecture, pods, API, etc.)
* Scheduling
* Logging & Monitoring
* Cluster Administration
* Security
* Storage
* Networking
* Troubleshooting

## Kubernetes Architecture

![Kubernetes-Architecture](./assets/Kubernetes-Architecture-1.png)

### ```etcd```

It's a key-value database, like a document storage, 
which store all the necessary information of worker nodes. For example
we can find out about nodes and the containers they are running.

### ```kube-scheduler```

It manage which node should accept and run which container. For doing
this important job, kube-scheduler considers several criteria like the
size of the container and the capacity of the worker nodes.

### ```Kube-controller-manager```

This component formed from several manager components. It monitors
worker nodes and makes sure the worker nodes are in the state the should
be in.

It monitors different components and try to keep them in the desired
state.

### ```kube-apiserver```

It's the most important component of Kubernetes because it handles all
the other components of Kubernetes, it handles communication between
worker and master nodes.

It orchestrate all the cluster's operations.

### ```kubelet```

It manages all the activities of the worker node and communicate withe
the master node, with kube-apiserver. It also give information about the worker node which
it's running on it to the master node. This component also exist on the master
node.

### ```Kube-proxy```

It handles communication between all worker nodes, and it guarantee that
proper rules exist to communicate containers on different nodes. It
manage communicate between our applications.

> **Note:** All of components on master nodes can deploy as containers
or systemd services.

![Containerd_version](./assets/Containerd_version.png)
![Container runtimes](./assets/container_runtime.jpeg)
![components-of-kubernetes.png](./assets/components-of-kubernetes.png)

## Deploy k8s cluster

* Production
	1. Kubeadm
	1. KOps
	1. kube-spray
* Training
	1. MiniKube
	1. Docker Desktop
	1. KinD
* Other
  1. Manually

## Ports and protocols
In order to proper functioning you need to open these ports on your
firewalls

On master:
1. 6443/TCP - Kubernetes API server
1. 2379-2380/TCP - etcd server client API
1. 10250/TCP - Kubelet API
1. 10259/TCP - Kube-scheduler
1. 10257/TCP - Kube-controller-manager

On worker:
1. 10250/TCP - Kubelet API
1. 30000-32767/TCP - NodePort Service

## Install and configure K8s

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
> **Note:** Please check ```/etc/fstab``` to eliminate any probable
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

> **Note:** To prevent apt from updating kubernetes packages run the
> following command:

```bash
sudo apt-mark hold docker.io kubelet kubeadm kubectl 
```

> **Note:** IF there was a docker permission error while starting docker.
> It can be solved by below command :
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

> **Note:** If you faced with ```docker.io : Depends: containerd (>=
> 1.2.6-0ubuntu1~) ``` error, you need to install ```containerd``` like:
```bash
sudo apt install containerd
```
> **Note:** If you faced with ```kubeadm init running into issue - [ERROR
> CRI]: container runtime is not running``` error, make sure you have
> installed ```docker```, then run the following commands:
```bash
sudo rm /etc/containerd/config.toml
sudo systemctl restart containerd 
```
then run your ```kubeadm init``` command with all your desier flags
again. After it's done copy the output ```token``` and run it in worker
nodes

Now copy the config files into ```$HOME/.kube``` dir

```bash
mkdir -p $HOME/.kube 
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config 
chown $(id -u):$(id -g) $HOME/.kube/config
```

Check all nodes

```bash
kubectl get nodes
```

In the above output, you should see that Master Node is listed as not
ready. Because the cluster does not have a Container Networking
Interface (CNI).

Deploying Calico CNI for the Master Node with the following command:

```bash
curl https://docs.projectcalico.org/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```

### Configure worker node

Just run the join command you've got in the output of ```kubeadm
init```, something like following:
```bash
kubeadm join 192.168.74.137:6443 --token ocbdbr.ghcbi8pw2xgbxqai \
         --discovery-token-ca-cert-hash sha256:11666d5f78f76f3c5b697839460a92eac1f7fd012bcd667d2e879aa5ac6eaa44
```

## Get ```kubectl join```  info

In case of you forgot ```kubectl joing``` information, you can get the
token by issuing the following command on a master node:
```bash
kubeadm token list
```

To get ```--discovery-ca-cert-hash``` run the following command on a
master node:
```bash
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin \
-outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.*//'

```

## Node labels

You can assign label to each kubernetes cluster nodes. To see the list
of labels run the following command:
```bash
kubectl get nodes
```
To assign a label run the following command:
```bash
kubectl label node <node-name> kubernetes.io/<key>=<value>
```
> **Note:** ```<node-name>``` available in ```kubectl get nodes```
output.

As an example:
```bash
kubectl label node kube-worker1 kubernetes.io/role=node
```

> **Note:** It's a good practice to install ```kubectl``` on your local
> machine, a machine outside of the cluster in the same network, and
> control the luster with that. Remember you have to copy
>	```/etc/kubernetes/admin.cnf``` in your local environment, in Linux
> environment it's ```~/.kube/config```

> **Note:** you can get cluster information with ```kubectl cluster-info```

# ETCD

ETCD is a distributed reliable key-value store that is simple, secure
and fast. It's listen on port ```2379``` and you can access to the
database with ```etcdctl```.

```bash
etcdctl set key1 value1
etcdctl get key1
```

## ETCD in kubernetes

In the context of a Kubernetes cluster, etcd instances can be deployed
as Pods on the masters.

![etcd1](./assets/etcd1.png)

To add an additional level of security and resiliency it can also be
deployed as an external cluster.

![etcd2](./assets/etcd2.png)

The following sequence diagram, shows the components involved during a
simple Pod creation process. It’s a great illustration of the API Server
and etcd interaction.

![etcd3](./assets/etcd3.png)

### What kind data ETCD stores?

* Nodes
* PODs
* Configs
* Secrets
* Accounts
* Roles
* Bindings
* Others

For more information head over [here](https://betterprogramming.pub/a-closer-look-at-etcd-the-brain-of-a-kubernetes-cluster-788c8ea759a5)

## Kube API Server

![api-server](./assets/api-server.png)

For more information head over [here](https://michalswi.medium.com/introduction-to-kubernetes-api-the-way-to-understand-the-concept-of-kubernetes-operators-ed667385caf4)



# Related

* [Install kubeadm]( https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
* [Firewall](https://kubernetes.io/docs/reference/ports-and-protocols/)
* [Container runtime prerequisites](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
* [Containerd](https://github.com/containerd/containerd/blob/main/docs/getting-started.md)
* [Containerd](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd)

