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

# Configure oracle user on Oracle Server for Ansible
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
# Example: Ansible server example playbook
```
- hosts: 158.176.141.190
  tasks:
    - name: check user
      remote_user: oracle
      command: whoami
      register: command_output

    - debug:
        var: command_output.stdout_lines