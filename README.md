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

## how to use

1) set all switches with a name and ip in the inventory file
2) create a host var file per switch with the same name you used in the inventory file.
3) Create a vault file to keep the switch credentials safe

   ```ansible-vault create passwd.yaml```

 **It will ask for a master passwords, Remember this we will need it later.**

 This will open up an editor
 provide user and passwords like:

 ```
 dd_01_user: admin
 dd_01_password: mypassword
 dd_02_user: admin
 dd_02_password: mypassword
 ```
 and so on, for as many datadomain there are your created earlier

 4) in the host var files created earlier, make sure you define all the ports you want to enable or disable. This is per datadomain so that you can have different ports per datadomain if needed

 ## Executing

 To execute this we need to provide the vault password, as now all the Datadomain credentials are encrypted. Depending on what is possible there are 3 main ways of doing this.

 There are 2 playbooks:
 * enable-dd-interface.yaml -> this enables the ports
 * disable-dd-interface.yaml -> this disables the ports

- - -
 ### manually

 ``` 
ansible-playbook playbooks/enable-dd-interface.yaml --extra-vars '@passwd.yaml' --ask-vault-pass
 ```
 This will request the vault password before running the enable playbook. Change to disable if needed.
 
- - -

 ### automated via a vault password file.
 
 Create a file named **vault_pass.txt**
 In this file on the first line type in your vault password
 ```
chmod +600 vault_pass.txt
 ```

 now run 

 ```
ansible-playbook enable-dd-interface.yaml --extra-vars '@passwd.yaml' --vault-password-file vault_pass.txt
 ```

 - - - 

 ### Automated via a vault environment variable

 If you automation solution that can set an environment variable on run time, this would be the msot secure way
 There is a special script that will read this variable and pipe it into the vault pass file flag.

 set the following variable on runtime.

 ```MY_VAULT_PASSWORD```

 also make sure to

```
 chmod +x vault_password.sh
 ```

 now run it as

 ```
ansible-playbook enable-dd-interface.yaml --extra-vars '@passwd.yaml' --vault-password-file vault_password.sh
 ```