--------
title: Ansible : Variables 

--------

# Ansible: Variables

*by Mark Nielsen*  
*Copyright October 2023*

The purpose of this document is to:

- Explain variables
- Special Variables: Fact, connection, magic


---

* [Links](#links)
* [Variables Overview](#var)
* [Special Variables](#special)
    * [Facts](#facts)
    * [Connection](#connection)
    * [Magic](#magic)
* [Defining Variables](#define)
    * [Playbook](#playbook)
        * [Playbook Definitions](#pbdef)
        * [Playbook Output Defintions](#pbdefoutput) 
    * [Inventory](#iv)
        * [Host](#host)
        * [Group](#group)

    * Role
    * Runtime
    

* * *

<a name=links></a>Links
-----
* [Using Variables](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html)

* [How to Use Different Types of Ansible Variables](https://spacelift.io/blog/ansible-variables)
* [Ansible Roles and Variables](https://www.dasblinkenlichten.com/ansible-roles-and-variables/)
* [Sample Ansible setup](https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html)



* * *

<a name=var></a>Variables
-----

First thing to do is [understand the precedence of variables](https://docs.ansible.com/ansible/latest/reference_appendices/general_precedence.html#general-precedence-rules).

For variables defined by user... from [Ansible Roles and Variables](https://www.dasblinkenlichten.com/ansible-roles-and-variables/)
    * Variables defined in role ‘defaults’
    * Variables defined as group_vars
    * Variables defined as host_vars
    * Variables defined in plays
    * Variables defined in role ‘vars’
    * Variables defined at runtime with –e switch

Second thing, understand the different type of variables. 

* [Special variables](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html) are defined by ansible. Ansible will override any attempt by the user to set them.
   * [Facts](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html#ansible-facts) : variables ansible gets from each host it connects to before it does anything. Facts can be turned off.
   * Connection variables : Variables which determine how to execute commands on a host.
       * Examples
           * "become" variable which will execute commands as root or superuser.
           * "ansible_user" which is the user ansible logs as into the server. 
   * [Magic Variables](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html#information-about-ansible-magic-variables)
       * These are variables which get set when ansible runs. Many come from how the config files are set.
           * Examples  
               * groups : A dictionary of all groups in host file or inventory.
               * hostvars : variables assigned to each host.
               * inventory_hostname : The current "host" being worked on from the inventory in the host file. Note: "inventory_hostname" may not be the same as the name the host believes it is which is recorded as ansible_hostname from "facts" information. 
* Defining variables : In any YAML file you can define variables.
    * Playbook variables
        * Variables can be defined
	* Variables can be regisered from output of commands or scripts
        * Registering variables
            * You execute a command and then register (record it output)
    * Inventory variables
        * Host variables : Variables for host can be defined in the host or inventory file. 
        * Group variables
    * Role variable : Variables can be defined in the role files. 
    * At runtime


* * *

<a name=set></a>Set Variables
-----
Set a role default roles and role vars. 

```shell
mkdir -p /etc/ansible/roles/role1/defaults
mkdir -p /etc/ansible/roles/role1/vars

echo "---" > /etc/ansible/roles/role1/defaults/main.yml
echo "role_default1 : ' role default value'" >> /etc/ansible/roles/role1/defaults/main.yml
echo "order_var1 : ' role default value'" >> /etc/ansible/roles/role1/defaults/main.yml
echo "order_var2 : ' role default value'" >> /etc/ansible/roles/role1/defaults/main.yml
echo "order_var3 : ' role default value'" >> /etc/ansible/roles/role1/defaults/main.yml
echo "order_var4 : ' role default value'" >> /etc/ansible/roles/role1/defaults/main.yml
echo "order_var5 : ' role default value'" >> /etc/ansible/roles/role1/defaults/main.yml
echo "order_var6 : ' role default value'" >> /etc/ansible/roles/role1/defaults/main.yml


echo "---" > /etc/ansible/roles/role1/vars/main.yml
echo "role_var1 : ' role var 1 value'" >> /etc/ansible/roles/role1/vars/main.yml
echo "order_var6 : ' role var 1 value'" >> /etc/ansible/roles/role1/vars/main.yml
```

Set a Host and Group variable. Note, change this host to your server. In /etc/ansible/host

```shell
# NOTE: Change the ip address to the ip address of your target server or its hostname.

echo "[testservers]
 192.168.1.7 host_var1=' host var1 value', order_var2='host var1 value'" >> /etc/ansible/hosts

echo ""  >> /etc/ansible/hosts

echo "[testservers:vars]
group_var1 = 'group var 1 value'
order_var3 = 'group var 1 value'
order_var4 = 'group var 1 value'
order_var7 = 'group var 1 value'" >> /etc/ansible/hosts


mkdir -p /etc/ansible/host_vars
mkdir -p /etc/ansible/group_vars

echo "---
host_var2='host var2 value'
order_var3='host var2 value'
order_var4 = 'host var 2 value'
order_var7='host var2 value'" >> /etc/ansible/host_vars/192.168.1.7.yml

echo "---
group_var2='group var 2 value'
order_var3='group var 2 value'
order_var4 = 'group var 2 value'
order_var5='group 2 value'
order_var7='group 2 value'" >> /etc/ansible/group_vars/testservers.yml

```

Set a Playbook variable. Create a file /etc/anisble/mongo.yml

```shell

echo "
---
  - name : Sample echo playbook
    roles :
      - role1
    hosts: testservers
    vars:
      playbook_var1: 'a value'
      order_var6: 'playbook 1 var'
      order_var7: 'playbook 1 var'

    tasks:
      - name : print vars
        debug:
          msg:
            - role default  {{ role_default1 }}
            - role var1     {{ role_var1 }}
            - host_var1     {{ host_var1 }}
            - host_var2     {{ host_var2 }}
            - group_var1    {{ group_var1 }}
            - group_var2    {{ group_var2 }}
            - playbook_var1 {{ playbook_var1 }}
            - order_var1    {{ order_var1 }}


" > /etc/ansible/echo.yml

```

Now execute
```shell
ansible-playbook echo.yml
```

and you should get something like

```

PLAY [Sample echo playbook] **************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [192.168.1.7]

TASK [print vars] ************************************************************************************************************
ok: [192.168.1.7] => {
    "msg": [
        "role default   role default value",
        "role var1      role vars value",
        "host_var1     a value",
        "host_var2     host var2 value",
        "group_var1    host 1 value",
        "group_var2    group 2 value",
        "playbook_var1 a value",
        "order_var1     role vars value"
    ]
}

PLAY RECAP ********************************************************************************************************************
192.168.1.7                : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0


```
