# Deployments vs StatefulSets vs DaemonSets

Different resources that Kubernetes provides for deploying
pods.different resources that Kubernetes provides for deploying pods.

* Deployments
* StatefulSets
* DaemonSets

There is one other type [ReplicationController](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/)but Kubernetes now favors
Deployments as Deployments configure ReplicaSets to support
replication.

## Deployments

Deployment is the easiest and most used resource for deploying your
application. It is a Kubernetes controller that matches the current
state of your cluster to the desired state mentioned in the Deployment
manifest.

Deployments are usually used for stateless applications. However, you
can save the state of deployment by attaching a Persistent Volume to it
and make it stateful, but all the pods of a deployment will be sharing
the same Volume and data across all of them will be same.

Deployments creates a ReplicaSet which then creates a Pod
so whenever you update the deployment using RollingUpdate
strategy, a new ReplicaSet is created and the Deployment moves the Pods
from the old ReplicaSet to the new one at a controlled rate. Rolling
Update means that the previous ReplicaSet doesn’t scale to 0 unless the
new ReplicaSet is up & running ensuring 100% uptime. If an error occurs
while updating, the new ReplicaSet will never be in Ready state, so old
ReplicaSet will not terminate again ensuring 100% uptime in case of a
failed update. 

## StatefulSets

StatefulSet is a Kubernetes resource used to
manage stateful applications. It manages the deployment and scaling of a
set of Pods, and provides guarantee about the ordering and uniqueness of
these Pods.

StatefulSet is also a Controller but unlike Deployments, it doesn’t
create ReplicaSet rather itself creates the Pod with a unique naming
convention. e.g. If you create a StatefulSet with name counter, it will
create a pod with name counter-0, and for multiple replicas of a
statefulset, their names will increment like counter-0, counter-1,
counter-2.

Every replica of a stateful set will have its own state, and each of the
pods will be creating its own PVC(Persistent Volume Claim). So a
statefulset with 3 replicas will create 3 pods, each having its own
Volume, so total 3 PVCs.

StatefulSets don’t create ReplicaSet or anything of that sort, so you
cant rollback a StatefulSet to a previous version. You can only delete
or scale up/down the Statefulset.

StatefulSets are useful in case of Databases especially when we need
Highly Available Databases in production as we create a cluster of
Database replicas with one being the primary replica and others being
the secondary replicas.

## DaemonSet

A DaemonSet is a controller that ensures that the pod runs on all the
nodes of the cluster. If a node is added/removed from a cluster,
DaemonSet automatically adds/deletes the pod.

Some typical use cases of a DaemonSet is to run cluster level
applications like:

1. Monitoring Exporters
1. Logs Collection Daemon

However, Daemonset automatically doesn’t run on nodes which have a taint
e.g. Master. You will have to specify the tolerations for it on the
pod.

In terms of behavior, it will behave the same as Deployments i.e. all
pods will share the same Persistent Volume.

Similar to ReplicaSet, but DaemonSets run one replica per node in the
cluster.

Tags:
```
#kubernetes #ReplicationController
```

Related:
```
* https://medium.com/stakater/k8s-deployments-vs-statefulsets-vs-daemonsets-60582f0c62d4
```
