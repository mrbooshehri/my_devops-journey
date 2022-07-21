# Kubernetes Installation methods

## Single-node installation 
for those ones to want to preview Kubernetes or it’s suitable for
practices, tests and development purposes.
1. minikubea - With minikube, Kubernetes can be deployed on VM,
	 Container, or bare-metal systems. Multiple container runtimes
	 (Docker, Containerd, CRI-O) are supported too.
1. kind - a distribution for running Kubernetes cluster inside Docker
	 containers on your local system.
1. k3s - It’s initially built for IoT and edge computing but can be used
	 for any other purpose. You can run a single-node Kubernetes or a
	 cluster with a couple of commands.
1. k3d - is a helper for running k3s on Docker. It launches a Kubernetes
	 cluster on your local machine just like “kind”
1. microk8s - Not only a single-node Kubernetes but a cluster can also
	 be deployed using microk8s. 

## Manual cluster installation:
This type of installation is used for deploying a minimum viable
cluster. It's a preferred way to deploy the Kubernetes cluster for the first time.
* kubeadm - tool that is used to deploy a cluster by human hands. 

## Automatic cluster installation
This type of installation is done by using automation tools, scripts, or
provider distributed installers. It’s a preferred way for those ones
want to deploy production-grade Kubernetes clusters on an on-premises
environment or want to manage cluster lifecycle manually.

1. [kubespray](https://kubespray.io) - a collection of Ansible playbooks that are used to deploy
	 production-grades clusters on both bare-metal and the cloud.
	 Alongside the installation, day two operations can be performed with
	 kubespray.{SEARCH FOR TERM 'day two operations'}
1. kops - ill not only manage the cluster lifecycle, but it also will
	 provision necessary cloud infrastructure. Deploying on AWS is
	 supported officially, deploying on other cloud providers is available
	 but in alpha and beta state.
1. RKE - is a Rancher distributed Kubernetes that can deploy
	 production-grade Kubernetes clusters on top of Docker containers.
	 Managing Kubernetes clusters is done easily with RKE. If you want to
	 use the Rancher platform, you should select this distribution.
1. Charmed Kubernetes - It’s suitable for running Kubernetes both on
	 Multi-cloud environments and bare-metal. If you look for an eligible
	 Kubernetes distribution that can be deployed on OpenStack, this
	 installer is for you.
1. KubeSphere - It is not only a Kubernetes distribution, it also is a
	 platform for creating a cloud solution based on Kubernetes.
1. Kubermatic - is a Kubernetes platform just like the Rancher. You can
	 deploy and manage Kubernetes clusters on clouds as well as
	 on-premises. The connection between the master/seed cluster and the
	 downstream clusters is handled by OpenVPN.
1. KubeOne - is a tool to provision necessary infrastructure and
	 deploying Kubernetes on a couple of providers. It can easily be
	 integrated with Terraform and Kubermatic.

## Managed clusters:
The lifecycle of clusters is managed by the providers.
1. Magnum
1. EKS
1. GKE
1. AKS

## Kubernetes the hard way
The hard way method is used to install Kubernetes from scratch.
# What is a Kubernetes distribution
It’s not “vanilla” Kubernetes, meaning a Kubernetes installation that
you create by downloading the Kubernetes source code from GitHub,
compiling it and installing it yourself. Almost no one would install
Kubernetes that way because it would take way too much work. It build
Kubernetes. It provide other tools and components to enhance or
provide more features and to focus on additional aspects like a security
focus, a DevOps focus, or another focus. 

## Types of Kubernetes distributions

* **‘Pure’ distributions:** These are platforms that offer pre-built
	Kubernetes and just pre-built Kubernetes. For the most part, they
	leave it up to the user to choose which other technologies to use to
	build out a complete containerized application stack. Canonical Kubernetes
	and Kontena Pharos are examples in this category.
* **‘Plus’ distributions:** Platforms that integrate Kubernetes
	with other specific technologies, such as certain container runtimes,
	host operating systems or control-plane add-ons. These are Kubernetes
	distributions in the sense that they include Kubernetes, but they do
	not give you the pure upstream version of the technology or the
	flexibility to set it up any way you want. OpenShift and Rancher (both
	of which are broad platforms that didn't include Kubernetes at all in
	their early days) are examples.
* **Kubernetes-as-a-service:** If you run Kubernetes in the cloud using a
	managed service, you are essentially consuming a distribution that is
	provided by your cloud vendor. These distributions give users the
	least amount of control. But you can get up and running in just a few
	clicks, so who’s complaining? Azure AKS, AWS EKS and Google GKE are
	the obvious examples of this type of Kubernetes-as-a-service
	distribution.
* **Limited-purpose** distributions: The final category includes those
	distributions (or platforms built using Kubernetes) that are intended
	for a specific and limited purpose. Single-node, “lightweight”
	Kubernetes distributions such as MicroK8s and K3s are examples. So is
	(arguably) KubeVirt, a platform that uses Kubernetes to orchestrate
	virtual machines instead of containers.

# What shipped in Kubernetes distributions
1. Container runtime and registries
	1. CRI-O
	1. Containerd
	1. Kata
1. Networking
	1. Container Network Interface (CNI)
	1.  Openshift SDN
1. Storage

**Note:** There are tons of Kubernetes distributions out there,
CNCF-certified can be a good criteria too chose suit distributions

## Important distributions
1. Red Hat OpenShift
1. Mirantis
1. VMware Tanzu
1. Canonical
1. Rancher


# Rancher distributions

## RKE 

RKE is a CNCF-certified(RKE's CNCF certification means that every
release supports the same APIs as upstream Kubernetes.) Kubernetes distribution that runs entirely
within Docker containers. It solves the common frustration of
installation complexity with Kubernetes by removing most host
dependencies and presenting a stable path for deployment, upgrades, and
rollbacks.

> It is possible to deploy an HA cluster and perform both version and configuration updates from a single manifest.

## RKE2
RKE2, also known as RKE Government, is Rancher's next-generation
Kubernetes distribution. It is a fully conformant Kubernetes
distribution that focuses on security and compliance within the U.S.
Federal Government sector. 
To meet these goals, RKE2 does the following:

* Provides defaults and configuration options that allow clusters to
	pass the CIS Kubernetes Benchmark v1.6 with minimal operator
	intervention
* Enables FIPS 140-2 compliance
* Regularly scans components for CVEs using trivy in our build pipeline

## K3s
The certified Kubernetes distribution built for IoT & Edge computing -
Lightweight Kubernetes


# cluster lifecycle manager


# Related

```
* https://itnext.io/kubernetes-installation-methods-the-complete-guide-1036c860a2b3
* https://containerjournal.com/topics/container-ecosystems/kubernetes-distribution-what-it-is-and-what-it-isnt/
* https://betterprogramming.pub/kubernetes-distributions-what-are-they-be2c438c8706
* https://www.suse.com/c/rancher_blog/rke-vs-rke2-comparing-kubernetes-distros/
* https://docs.rke2.io/
* https://k3s.io/
```


