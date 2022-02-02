PART2: How to setup Ansible to become another unprivileged user
1. Configure ansible.cfg file by changing the following parameters to below.
# Find the following paramters in the file and set it as follow.
allow_world_readable_tmpfiles = True
pipelining = True

2. Set playbook as follow
- hosts: orclients
  become: true
  become_method: sudo
  become_user: oracle
  vars:
    allow_world_readable_tmpfiles: True

-------------------
PART3: Set up environment path for target managed nodes
# Set individually to each task
- name: run shell script in oracle servers
    shell: "/home/oracle/demo_shell.sh"
    environment:
    ORACLE_HOME: "/u01/app/product/19c"
    ORACLE_SID: "db19000"
    PATH: "$PATH:/u01/app/product/19c/bin"

# Set for the whole block
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

--------------
