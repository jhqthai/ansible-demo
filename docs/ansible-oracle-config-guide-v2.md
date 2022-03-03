*This configuration allow direct access to oracle user.*

# PART1: Configuration for oracle user
1. Config oracle user to run sudo command password free
* As root user, create a file (under /etc/sudoers.d)
` vi /etc/sudoers.d/99-oracle `
* Add the following in file
` oracle  ALL=(ALL)  NOPASSWD:ALL `

2. As oracle user, copy ssh public key into oracle authorized_keys (assuming there's a keypair)

```
 sudo vi ~/.ssh/authorized_keys 
 copy public key in file

```
# PART2: Ansible server example playbook
```
- hosts: 158.176.141.190
  tasks:
    - name: check user
      remote_user: oracle
      command: whoami
      register: command_output

    - debug:
        var: command_output.stdout_lines
```
