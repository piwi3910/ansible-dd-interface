# Ansible-dd-interface

This ansible playbook allows for a quick way to enable and disable ports on a datadomain.
As datadomain is not a full usable Linux system, we utilize the ansible raw module to pass direect commands over ssh to the datadomain

## Requirement
The following items need to be installed when using passwords (not recommended) to connect to the datadomain -> use ssh keys is better

* sshpass

REMARK: 
```
1) login once with ssh directly before running ansible playbooks
2) accept the key to the known hosts
3) say no to create a security officer, so this popup goes away, else it will break the ansible connection
```

## Function

Define one or more datadomains in the inventoryfile.
You may call them whatever you like
make sure there is a file named dd1.yml that matches what you name the dd in the inventory file.
So if you name it mydd create a file named mydd.yml under the host_vars

In this file specify the ip, ssh user and password (not recommend) -> use ssh keys is better

you can specify multiple ports to enable/disable at once
example:

```
dd_ports:
  - "ethV1"
  - "ethV2"
  - "ethV3"
```

all these ports will be enabled or disabled in one single run