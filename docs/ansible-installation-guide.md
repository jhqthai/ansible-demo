# How to install Ansible on RHEL8
Source: https://adamtheautomator.com/install-ansible/

1. Install epel release to install ansible
`sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`

2. Install ansible
`sudo dnf install ansible`

Note: Try the command below if the above command does not work
`sudo dnf install  --enablerepo epel-playground ansible`

3. Configure ansible host by adding the host name or ip address to the file below
`sudo vi /etc/ansible/hosts`

4. Configure ansible.cfg to disable host_key_checking to not having to log in everytime on host [optional] - Avoid in production as insecure
`sudo vi /etc/ansible/ansible.cfg`
4.1 Set the following line in the file as below.
`host_key_checking = False`

5. Set up key in both servers 
5.1 Single ssh key
Source: https://medium.com/openinfo/ansible-ssh-private-public-keys-and-agent-setup-19c50b69c8c
5.2 Multiple ssh key setup
Source: https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client

6. Test - Run the following command in cmd
`ansible clientName -m ping`

---------------------
## Extra Resources

https://chartio.com/resources/tutorials/how-to-create-a-user-and-grant-permissions-in-oracle/
CONNECT username/password@[//]host[:port][/service_name]

Guide:
https://www.redhat.com/sysadmin/ansible-quick-start
https://docs.ansible.com/ansible/latest/user_guide/connection_details.html#setting-up-ssh-keys


