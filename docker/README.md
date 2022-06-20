# Session 1

**Note:** Docker offers good solutions to manage microservice based
applications. 

## DevOps
DevOps is a culture, movement or practice that emphasizes the
**collaboration** and **communication** of both software developers and
other IT professionals while **automating the process of software
delivery and infrastructure changes.**  
DevOps is a **cultural** and operational model that fosters
**collaboration** to enable high-performance **IT** to achieve
**business goals**

**Artifact:** It's the production of development side. It can be a jar
file or even a docker image or etc. The operation side deploy the
artifact.

DevOps fills the gap between developers and operation unit. It tends to
find the best and easiest practice that connect development unit and
operation unit(to share the artifact, at least)

## Continues Integration(CI)
CI is a software development practice which frequently refers to
integrating, building and testing code within the development
environment.

Benefits of CI
* If you are going to fail, then fail early
* Increase confidence in the software
* Team communication
* Risk mitigation

CI pipeline
* Build
* Unit tests
* Integration tests

### Continues Delivery(CD)
* Continues Delivery is about putting the release schedule in the hands
of the business, not in the hands of IT.
* Making sure you software is always production ready throughout its
	entire lifecycle that any build could potentially be released to
	users.

Benefits of CD
* It helps in time reduction

CD pipeline
* Review
* Staging
* Production

### Continues Deployment
All the process form	code to production handel by automation
mechanisms, if you do anything manually from staging to production it's
not continues deployment anymore, it's continues delivery.

# Session 2

## Monolithic Architecture
A monolithic architecture is where all required logic and components are
located within one unit (a war, a jar, a single application, one
repository)

## Pros and cons of monolithic architecture
Pros:
* Simple delivery
* Simple test
* Simple deploy

Cons:
* The size of the application can slow down the start-up time
* Difficulty to adopt new and advanced technology. Since changing in
	language or framework affect an entire application
* Maintenance, it's difficult to make change fast and correctly
* Reliability, bug in any module can potentially bring down the entire
	process

## Microservice Architecture
An application is built in smaller, separate pieces. Each service is
developed, tested and deployed independently. This architecture works
with API gateway to communicate with customers and response to their
requests.

### Pros and cons of microservice architecture
Pros:
* Faster and simpler deployment and rollback
- Independent scalable services
* Microservice enables the continues delivery and deployment of large
	and complex applications
* Improve fault isolation
* Technology diversity

Cons:
* Increase network communication
* Additional complexity in testing and distributed system
* Deployment complexity

## What is docker
Docker is a containerization platform that package your application and
all its dependencies together in the form of a docker container to
ensure that your application works seamlessly in any environment

## What is a docker image
A docker image is a collection of all the files that make up an
executable software application. This collection includes the
application plus all the libraries, binaries, and other dependencies
just needed to run the application anywhere.

### Docker image architecture(layered architecture)
* Docker image followed a layered architecture
* Layers are reusable and share
* Rebuilding the image will be faster
* Files in the layers are read-only
* If you chose to alter the content of your image, the only valid docker
	option is adding another layer with the new changes
* Docker images have a parent-child relationship
* The bottom-most image is called the base-image that doesn't have any
	parent

**Note:** We cannot run widows-based containers on Linux, because they
use different kernel. So to run that kind of containers you need to run
that on docker in a windows machine.

### What is a docker container
A container is a runnable instance of an image. It is a standard unit of
software packages up code and all its dependencies, so the application
runs quickly and reliably from one computer environment to another.

### What is a docker registry
It's a place that docker images can be stored in order to publicly or
privately found and accessed. Docker registry can be hosted by a
third-party as a publicly or privately registry like:
1. Docker hub
1. Quay
1. Google container registry
1. AWS container registry

Image registries can contain multiple image repositories. In turn, image
repositories can contain multiple images(also different versions of an
image).

**Note:** Container is a process on your host machine.

## Installing Docker
```bash
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum-config-manager --disable docker-ce-edge
yum makecache fast
yum install docker-ce
systemctl enable --now docker
```

## Get information about docker installed version

The following command will only display the version of docker
```bash
docker --version
```

For more information about your docker you should use this command
instead
```bash
docker version
```
According to the previous command you can see docker has a client and a
server part.

In case of you need full information about docker you can use the
following command
```bash
docker info
```

## Get an image from docker registry
The default docker registry is ```docker hub```. To get image use the
following command
```bash
docker pull IMAGE_NAME
```
**Note:** It's a good pracitce not to use the ```latest``` version(tag)
of images in production phase. 

To see the downloaded images
```bash
docker image ls
# or
docker images
```
with ```-a``` option you can see all the images and ```-q``` will just
list image IDs.

## Image naming structure
Each image formed from to part, repository and tag which follow this
stype ```IMAGE_REPO_NAME:IMAGE_TAG```. Each image has a uniqe ID.

## Runing an instace from an image
Until here we just pulled an image from docker official registry, to
creating an instace:
```bash
docker run IMAGE_NAME
```
## Get information about containers

```docker ps``` by default lists the runing containers and get
information about them but, ```-a``` option will show all the
containers. ```-f status=exited``` lists containers with a specific
status,```-f``` stands for filter and ```-q``` list only the container
IDs. Also ```-n=N``` list the ```N``` last containers.

# Session 3
## Manage docker as a non-root user
The docker daemon binds to a unix socket instead of a TCP port. By
default a unix socket owned by root user and other users can only access
it using sudo. The docker daemon always runs as the root user. To solve
this problem you should add your user to the ```docker``` gourp.

```bash
usermod -aG docker $USER
```
**Note:** Only root user can use unix sockets.

## Virtual machine vs Docker

### Differences

| VM | Docker |
| :--: | :--:|
| Each VM runts its OS | All containers share the same kernel of the host |
| Boot up time is in minutes | Containers instantiate in seconds |
| VMs snapshots are sparingly | Images are build incrementally on top of another like layers |
| Not effective diffs, no version control | Image can be diffed and can be version controlled |
| Cannot run more than couple of VMs on an average laptop | Can run many docker containers on a laptop|
| Only one VM can be started from set of VMX and VMDK files | Multiple docker containers can be started from an docker image|

### Similarities

| VM | Docker |
| :--: | :--:|
| Process in one VM cannot see processes in other VMs | Process in one container cannot see processes in other containers |
| Each VM has its own root file system | Each container has its own root file system (not kernel) |
| Each VM gets its own virtual network adapter | Docker can get virtual network adapter | 
| VM is a running instance of physical files(VMX and VMDK) | Containers are running instances of docker image |
| Host OS can be different from guest OS | Host OS can be different from container OS|

## Where should you run docker containers?

### Why run docker on bare-metal
1. Access through bare-metal hardware in apps without relying on
	 pass-through techniques.
1. Have optimal use of system resources. A host can, distirbute use of
	 shared system resources
1. Get bare-metal performance to apps, because there is no hardware
	 emulation layer saparating them from a host server.
1. deply apps inside portable environments that can move easily between
	 host and server
1. Get app isolation

### Why don't run docker on bare-metal
1. Physical server upgrades are difficult
1. Most clouds require VMs
1. Container platforms don't support all hardware and software
	 configurations
1. Containers are OS-dependet
1. Bare-metal servers don't offer rollback features

### Advntages of docker on bare-metal vs VM
1. Fewer layers to manage and simpler trubleshooting
1. High efficiency
1. More containers per server
1. Better, more predictable performance 
1. Lower total costs

## Benefits Docker containers
1. Modularity
1. Free up a huge amount of system resources
1. Speed
1. Portability
1. Reduce potential licencing cost, patching and maintenance of OS
1. Layers and image version control
1. Rollback
1. Rapid deployment
1. Run you application in an isolation and independent environment

## What problems docker solve?
1. Dependencies - dependency hell
1. Improving portability - your desktop your develop environment,
	 company's server, and your company's all can run the same programs.
1. Protecting your computer

## Traditional Linux container (LXC) vs Docker
LXC can run multiple aplications in each container while it 
is recomanded to run only single applications in a docker container.

## Docker Engine
The docker engine the core software that run and manage containers which
follows client-server architecture.

### Docker engine components
* Server: The docker daemon (dockerd)
* Rest API: To instruct docker container what to do
* Command line interface (CLI)

## Older version of dockr
* Docker had two major components:
	* The docker daemon
	* LXC
* The docker daemon was a monolithic binary
* LXC provided the daemon with access to the fundemental
	building-blocks of containers existed in the linux kernel.

## What is cgroups
Cgroups are a kernel mechanism for limiting and measuring the total
resorces usedd by a group of process runing on a system. For example,
you can apply CPU, momory, network, or IO quotas

## What is Namespace
Namespaces are a kernel measuring for limiting the visibility that a
group	of processes have of the rest of the system. For example you can
limit visibility to certain process trees, network interfaces, user IDs
or filesystem mounts.

## Docker engine architecture
![docker engine architecture](./assets/docker-engine-architecture.png)

* ```runc``` has a single purpose in life -> **Create containers**
* The ```contierd``` purpose in life was to **manage container
	life-cycle** operations -> container life-cycle operations -> start |
	stop | pause | rm ..
* ```shim```: once a container's parent ```runc``` exits, the associated
	containerd, ```shim``` process becomes the **container's parent**.
	some of the responsibilites:
	* Keeping any STDIN and STDOUT stream open
	* Reports the container's exit status back to the daemon.

## Let's run a container
docker run <option> <image>:<tag> <app>
```docker run -it centos:latest /bin/bash```

* ```docker run``` tells the docker daemon to start a new container.
* The ```-it``` flags tell docker to make the container interactive
	and attach our current shell to the container's terminal.
* ```/bin/bash``` is the app we want to run inside the container

**Note:** It's not a good practice not to use ```latest``` tag for the
production.

**Note:** The ```PID 1``` in coniter belongs to the app we pass to the
container

**Note:** You can use container in detach mode by ```-d``` option.

**Note:** You can exit the container witout stop the container with
```ctrl+PQ```

# Session 4

## Daemonless containers
* By removing the code of containers management from the daemon, the
	entire container runtime is decoupled from the Docker daemon.
* It makes it possible to perform maintenance and upgrades on the docker
	daemon without impacting runing containers

### Enable live restore
* By default, when the docker daemon terminates, it shuts down running
	containers
* Live restore: containers remain running if the docker becomes
	unavailable
* Live restore is not supported on windows containers, but it does work
	for linux containers running on docker desktop for windows.

### How to enable live restore
Create/open the following file
```bash
vim /etc/docker/daemon.json
```
add this line
```bash
{
	"live-restore":true
}
```
reload docker daemon
```bsah
systemctl reload docker
```

### Docker components on Linux system
* dockerd - the docker daemon
* docker-containerd - containerd
* docker-containerd-shim - shim
* docker-runc - runc

### Docker daemon responsibilities
* Image management
* Image builds
* REST API
* Authentication
* Security
* Core Networking
* Orchestration

## Storage drivers
Every docker container gets its own area of local storage where all
containers read/write operations occur. This local storage area has been
managed by the storage driver or graphdriver. Some of the storage driver
abvailable for docker on Linux include:
* aufs - the original and the oldest
* overlay2 - probably the best choice for the future
* devicemapper
* btrfs
* zfs

On linux you set the storage driver in ```/etc/docker/daemon.json``` and
the you need to restart the docker service for any changes to take
effect.
```json
{
	"storage-driver":"overlay"
}
```
To change your driver storage 	while you need your images and
containers to be available after change
* Save them with docker save
* Pusht the saved images to a repository
* Chage the storage driver
* Restart docker
* Pull the images locally
* Restart your containers

**Note:** It's a good practice to choose your storage driver 
in design phaze(installation) and before you do anyting with docker.

### Chose which storage diver to ues
| OS | Storage dirver|
| :--- | :-----|
| RHEL with a 4.x kernel or higher + dokcer 17.06 and higher | overlay2|
| RHEL with an older kernel and older docker 	| devicemapper |
| Ubuntu with a 4.x kernel or higher | overlay2 |
| Ubuntu with an earlier kernel | aufs |
| SUSE Linux enterprise server | btrfs |
|

## Docker socket
When you install docker, you get two major components:
* the docker client
* teh docker daemon
On Linux the client talks to the daemon via a local IPC/Unix socket at
/var/run/docker.sock

## ```docker ps``` options
* ```-a``` - list all containers
* ```-q``` - only lists the container IDs
* ```-n=N``` - list the last ```N``` containers
* ```-f``` - fileter the output like ```-f status=exited```
* ```-l``` - the latest container
* ```-s``` - show the size of containers, the value inside the ```()```
	is the image size and the one before it is the container size minuse
	the image size

## Container resource management
-1:10:00

