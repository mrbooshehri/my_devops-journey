# Session 2

## Introduction to ansible 
Ansible is an automation platform that makes your application and
systems easier to deploy

It support configuration management with examples as below:
* Install and configuration of servers
* Application deployment
* Continuous testing of already installed application
* Orchestration
* Automation

Ansible can handle both Infrastructure as a code(IaaC) and
Configurations as a code(CaaC). 

IaaC ansible can handle the following situations:
1. Install a new package on an existing OS
1. Install an OS on physical server, Virtual server
1. Install Physical/virtual server on bare metal - for example only ILO
	 exists

## Why ansible
1. It's free and open source application
1. Agent-less - No need for agent installation and management, only
	 needs IP and SSH
1. Python/yaml based
1. Highly flexible and configuration management of system
1. Large number of ready to use modules for system management
1. Custom modules can be added if needed
1. Configuration roll-back in case of error
1. Simple and human readable
1. Self documenting

## Introduction to YAML
Yaml includes a markup language with important construct

The design goal and features of Yaml:
1. Matches native data structure languages
1. Yaml data is possible between programming languages
1. Includes data consist data model
1. Easily readable by human
1. Supporting one-direction	processing
1. Ease of implementation and usage

## Ansible architecture
![architecture](assetes/architecture.png)

* **Playbook**: It demonstrate what job should run in which host
* **Role**: It's set of task which lead to perform a job
* **Tasks**: Procedure of your desired job

## Ansible playbook structure
In ansible everything places in a provision(a directory).

Directory layout based on [official documentation](https://docs.ansible.com/ansible/2.8/user_guide/playbooks_best_practices.html)
```
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
   group1.yml             # here we assign variables to particular groups
   group2.yml
host_vars/
   hostname1.yml          # here we assign variables to particular systems
   hostname2.yml

library/                  # if any custom modules, put them here (optional)
module_utils/             # if any custom module_utils to support modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier

roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""

```
Alternative way:
```
inventories/
   production/
      hosts               # inventory file for production servers
      group_vars/
         group1.yml       # here we assign variables to particular groups
         group2.yml
      host_vars/
         hostname1.yml    # here we assign variables to particular systems
         hostname2.yml

   staging/
      hosts               # inventory file for staging environment
      group_vars/
         group1.yml       # here we assign variables to particular groups
         group2.yml
      host_vars/
         stagehost1.yml   # here we assign variables to particular systems
         stagehost2.yml

library/
module_utils/
filter_plugins/

site.yml
webservers.yml
dbservers.yml

roles/
    common/
    webtier/
    monitoring/
    fooapp/
```

* **Inventory**: Where the ```hosts``` file will place
* **Role**: Where projects directories will place, for example, install
	Apache, install samba, etc.
	* **Task**: It's least minimum that ansible needs regarding work
		properly 
	* **vars** and **defaults**: Where all variables could declare. The
		variables with less changes placed in **defaults**, like version,
		and other variables with more changes, like username or password
		keep in **vars**.
	* **files** and **templates**: Where all files that needed during the
		run process keep in. Immutable files like ```.rpm```, ```.tar.gz```,
		and others placed in **files** and mutable file like config files
		placed in **template**. All files in **template** should have
		```.j2``` file extension.
	* **handlers**: Where placed actions like ```stop```, ```start```,
		```restart```, ```enable```, etc for service that installed or
		modified
	* **meta**: Where placed dependencies and preprocessing in it, for
		example install specific package, or disable ```selinux```

## Hot to run Ansible Playbook with parameters and error handling in playbooks 

```bash
ansible-playbook -i /PATH/TO/<inventory-file-name> <playbook-name>.yml --<switches>
```

**Switches**:
* ```-v```: Verbose for view details(```-v```,```-vv```,```-vvv```)
* ```--tags=<tag-name>```: Run only <tag-name> used into task/main.yml
* ```--tags "<tag-name-1>,<tag-name-2>,..."```: Select tags for RUN
* ```--tags-skip "<tag-name-1>,<tag-name-2>,..."```: Select tags for not RUN 
* ```--step```: (y/n/c) (yes/no/continue) ask to RUN each step
* ```--list-tasks```: List of tasks in yaml file without RUN
* ```--extra-vars <var-name>="<value>"```: Create variable
* ```--syntax-check```: Yaml file syntax check
* ```--check``` (Dry-run mode): Check yaml file without RUN and Download
  make sure it's runnable

2:08
