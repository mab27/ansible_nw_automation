

# Title 1:
## Title A1:

- check version:
```
mab@mab-infra:~/automation/ansible$ ansible --version
ansible 2.2.0.0
  config file = /home/mab/automation/ansible/ansible.cfg
  configured module search path = Default w/o overrides
```

- Install dependcy:
```
pip install junos-eznc
```


- First run:
```
mab@mab-infra:~/automation/ansible-training-for-junos-automation/junos_template$ sudo ansible-playbook pb.bgp.2.yml 
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



- Second run (idempotency):
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
