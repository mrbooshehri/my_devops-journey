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
### Installing containerd
1. Check uniqueness of ```mac``` and ```product_uuid```
```bash
ip link
cat /sys/class/dmi/id/product_uuid
```
2. Set [shecan](https://shecan.ir) DNS
```bash
# vim /etc/resolve.cof
nameserver 185.51.200.2
```
3. Disable swap
```bash
swappff -a
sudo sed -i ‘/ swap / s/^\(.*\)$/#\1/g’ /etc/fstab
# or
sed -i '/swap/s/^/#/' /etc/fstab
```




# Related

[Install kubeadm]( https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
[Firewall](https://kubernetes.io/docs/reference/ports-and-protocols/)
[Container runtime prerequisites](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
[Containerd](https://github.com/containerd/containerd/blob/main/docs/getting-started.md)
[Containerd](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd)

