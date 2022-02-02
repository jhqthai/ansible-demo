# How to install Ansible on RHEL8
Source: https://adamtheautomator.com/install-ansible/

1. Install epel release to install ansible
> `sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`

2. Install ansible
> `sudo dnf install ansible`

> **Note:** Try the command below if the above command does not work
>> `sudo dnf install  --enablerepo epel-playground ansible`

3. Configure ansible host by adding the host name or ip address to the file below
> `sudo vi /etc/ansible/hosts`

4. Configure ansible.cfg to disable host_key_checking to not having to log in everytime on host [optional] - Avoid in production as insecure
> `sudo vi /etc/ansible/ansible.cfg`

> Then set the following line in the file as follow.
>> `host_key_checking = False`

5. Set up key in both servers 
> 5.1 Single ssh key
>> Source: https://medium.com/openinfo/ansible-ssh-private-public-keys-and-agent-setup-19c50b69c8c

> 5.2 Multiple ssh key setup
>> Source: https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client

6. Test - Run the following command in cmd
> `ansible clientName -m ping`

# How to configure Ansible for Oracle
## How to setup Ansible to become another unprivileged user
**Change the parameters below in the ansible.cfg file to allow unpriviledge escalation**
```
allow_world_readable_tmpfiles = True
pipelining = True
```

## Playbook Setup
**Set playbook as follow to utilises the unpriviledge user account**
```
- hosts: orclients
  become: true
  become_method: sudo
  become_user: oracle
  vars:
    allow_world_readable_tmpfiles: True
```

**Set up environment path for oracle managed nodes to use oracle db functionality**

*Method 1: Set environment individually to each task*
```
- name: run shell script in oracle servers
    shell: "/home/oracle/demo_shell.sh"
    environment:
    ORACLE_HOME: "/u01/app/product/19c"
    ORACLE_SID: "db19000"
    PATH: "$PATH:/u01/app/product/19c/bin"]
```

*Method 2: Set for the whole block*
```
  tasks:
    - name: oracle clients task
      block:
        - name: copy shell script to oracle servers
          copy:
            src: /home/cecuser/demo-files/demo_shell.sh
            dest: /home/oracle/demo_shell.sh
        ....
        - name: run shell script in oracle servers
          shell: "/home/oracle/demo_shell.sh"

      become: true
      become_method: sudo
      become_user: oracle
      vars:
        allow_world_readable_tmpfiles: True
      
      environment:
        ORACLE_HOME: "/u01/app/product/19c"
        ORACLE_SID: "db19000"
        PATH: "$PATH:/u01/app/product/19c/bin"
```

