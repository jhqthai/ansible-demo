*This is the old configuration. This configuration was used when the author did not know how to access the oracle server and modify ssh keys directly as an unpriviledged user*

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

