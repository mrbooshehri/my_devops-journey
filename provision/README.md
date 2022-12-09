# Provision meanings

> Provisioning is the process of setting up IT infrastructure. It can
also refer to the steps required to manage access to data and resources,
and make them available to users and systems.
> 
> Provisioning is not the same thing as configuration, but they are both
steps in the deployment process. Once something has been provisioned,
the next step is configuration.

> Provisioning involves creating resources like VMs/instances, setting
up IAM roles and policies, Firewalls, DBs, Load balancers, any clusters
like EKS, GKE, etc.

> And, configuration part involves, preparing your instances to run
actually your apps by installing necessary softwares and packages,
setting up tables in DBs and populating data, RBAC controls on
resources, etc.

> The term is used in different ways by different people and in
different contexts. I like to think of each layer of 'the whole thing'
as being provisioned individually, so I provision machine or container
images by installing software on them using docker or packer, then I
provision the infrastructure using terraform, and finally, I provision
my application using kubectl or helm. Though, this is only a working
model as these layers and tools tend to blend together at the edges.


Tags:
```
#devops #provision #provisioning
```
Related:
```
* https://devops.stackexchange.com/questions/12933/provisioning-meaning
```
