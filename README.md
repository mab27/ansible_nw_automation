# Network Automation with Ansible:

## Overview of Ansible:

- An IT automation tool designed for performing actions (tasks) on multiple servers/devices. Ansible is extensivly used for automating configurations build, and has become one of the de-facto tools in the industry for autmating IT.
- Agent-less: Ansible use SSH (quite native). There is no agent to install on the remote hosts, which is easisier, more secured because there are no additional ports to open.
- Push-based approach: As soon as you run the ansible-playbook command (to execute a playbook), Ansible connects to the remote servers/devices and does its thing. Different from the pull-based approch often assicated with the tools relying on agents.
- Some level of abstraction: The tool follows a declarative approach. You describe the desirated state, indicating "what", not "how“.

### Ansible components:
- Inventory file
- Variables		(YAML)
- Tasks / modules	(Python)
- Plays and Playbooks	(YAML)
	- A playbook is a list of plays (from a code perspective it is a list of dictionaries).
	- Playbook runs top to bottom.
	- A play maps a selection of hosts to tasks. Each task is a call to an Ansible module, a small piece of code (written in python) for doing a specific task. Each task is associated with exactly one module.
	- Tasks are executed in order, one at a time, against the selection of hosts, before moving on to the next task.
	- A play associates an unordered set of hosts with an ordered list of task. 

### Network modules:
- http://docs.ansible.com/ansible/list_of_network_modules.htm


## Getting Started with Junos modules:

### Pre-requisistes:
- Those examples are performed with Ansible version 2.2.0.0
- Install dependency (Vendor python library):
```
pip install junos-eznc
```
- Configure Netconf on the network devices.

### Example 1: configure BGP between two routers.

- Populate the addresses of the Juniper routers:
	- ```hosts``` file, located at the root of this repository.
- Provide the credentials information to reach the devices:
	- ```credentials.yml``` file, located at **\group_vars\vmx**
- Prepare the Jinja2 files for the template module to use them:
	- ```bgp.j2``` file, located in the plabook directorty: **\junos_template**
- Provide the variables information, to be used in the Jinja2 tempalting to render the configurations:
	- ```bgp.yaml``` file, located in **\host_vars\vmx1**
	- ```bgp.yaml``` file, located in **\host_vars\vmx2**

- Write the playbook:
	- ```pb.bgp.2.yml``` file, located at \junos_template\
	- This playbook has 3 plays:
		- **create BGP junos configuration**, composed of 2 tasks.
		- **wait for peers to establish connections**, composed of 1 task.
		- **check bgp states**, composed of 1 task.

- Run the playbook (First run):
```
mab@mab-infra:~/automation/ansible/junos_template$ sudo ansible-playbook pb.bgp.2.yml 
[DEPRECATION WARNING]: junos_template is kept for backwards compatibility but usage is discouraged. The module documentation details 
page may explain more about this rationale..
This feature will be removed in a future release. Deprecation warnings can be disabled 
by setting deprecation_warnings=False in ansible.cfg.

PLAY [create BGP junos configuration] ******************************************

TASK [Render BGP configuration for junos devices] ******************************
ok: [vmx1]
ok: [vmx2]

TASK [push bgp configuration on devices] ***************************************
changed: [vmx1]
changed: [vmx2]

PLAY [wait for peers to establish connections] *********************************

TASK [pause] *******************************************************************
Pausing for 25 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [localhost]

PLAY [check bgp states] ********************************************************

TASK [check bgp peers states] **************************************************
ok: [vmx2] => (item={u'interface': u'ge-0/0/0', u'local_ip': u'172.16.0.40', u'asn': 101, u'name': u'to vmx1', u'peer_ip': u'172.16.0.30'})
ok: [vmx1] => (item={u'interface': u'ge-0/0/0', u'local_ip': u'172.16.0.30', u'asn': 102, u'name': u'to vmx2', u'peer_ip': u'172.16.0.40'})

PLAY RECAP *********************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0   
vmx1                       : ok=3    changed=1    unreachable=0    failed=0   
vmx2                       : ok=3    changed=1    unreachable=0    failed=0   
```

- Run the playbook again (second run, to check idempotency):
```
mab@mab-infra:~/automation/ansible/junos_template$ sudo ansible-playbook pb.bgp.2.yml 
[DEPRECATION WARNING]: junos_template is kept for backwards compatibility but usage is discouraged. The module documentation details 
page may explain more about this rationale..
This feature will be removed in a future release. Deprecation warnings can be disabled 
by setting deprecation_warnings=False in ansible.cfg.

PLAY [create BGP junos configuration] ******************************************

TASK [Render BGP configuration for junos devices] ******************************
ok: [vmx1]
ok: [vmx2]

TASK [push bgp configuration on devices] ***************************************
ok: [vmx2]
ok: [vmx1]

PLAY [wait for peers to establish connections] *********************************

TASK [pause] *******************************************************************
Pausing for 25 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [localhost]

PLAY [check bgp states] ********************************************************

TASK [check bgp peers states] **************************************************
ok: [vmx2] => (item={u'interface': u'ge-0/0/0', u'local_ip': u'172.16.0.40', u'asn': 101, u'name': u'to vmx1', u'peer_ip': u'172.16.0.30'})
ok: [vmx1] => (item={u'interface': u'ge-0/0/0', u'local_ip': u'172.16.0.30', u'asn': 102, u'name': u'to vmx2', u'peer_ip': u'172.16.0.40'})

PLAY RECAP *********************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0   
vmx1                       : ok=3    changed=0    unreachable=0    failed=0   
vmx2                       : ok=3    changed=0    unreachable=0    failed=0   
``` 


### Example 2: 



## Getting Started with Arista modules:

### Example 1: configure BGP between two routers.

### Example 2: 