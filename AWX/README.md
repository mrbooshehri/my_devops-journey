# AWX Concepts

## Credential

* Used for authentication when launching jobs(playbook, ad-hoc commands)
	against machines
* Can be granted to users and teams - means share between teams and
	users
* Passwords and keys are encrypted in database(PostgreSQL)

> In ansible you to make the connection password-less or give the
username and password, in AWX it's under credential section.

## Project

In the fist step that we want to declare an ansible playbook, we need to
create a project

* A logical collection of ansible playbooks
* Playbooks can be set in Project base path on AWX server or in source
	code management(SCM) - you can pass dir(s) or URL(s), like git, that 
	contains the playbooks

## Template

* Set of parameters for running an ansible job

> It's the playbook(s)

## Job

Each ad-hoc commands or template which we run, creates an instance
called job.

* An instance of launching ansible playbook against inventory

## Organizations, teams, and users

![TowerHierarchy](./assets/TowerHierarchy.png)

# Start with the UI

> The following steps have chronological order to follow.

## Inventory

1. Create an inventory, chose an organization
1. Select your created inventory and create group
1. Select your created group and add your hosts
	* In time of adding host you can set IP or domain 

> In group/host section under inventory you can select desired groups/hosts and run
ad-hoc command. Remember you need to add remote credential already.

## Credential

To connect to your remote machines or running ad-hoc commands you need
to get the remote machines username and password in this section. There
are lots of option to pick, but for our case select ```Machine``` and
enter remote's credentials.

# Projects

A project is a logical concept which formed from one or more playbooks.
you can easily create new project under ```Resources > Projects```.

> **Note:** you can specify playbook source, git or path, etc., in
```Source Control Credential Type```.

> **Note:** If you want to use a ```path```, in kubernetes or docker,
you should make ```/var/lib/awx/projects``` persists.

# Templates

Template let's you define a set of parameters for your playbook. 

# Tips

1. You can schedule your cron jobs in AWX
