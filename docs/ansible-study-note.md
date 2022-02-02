## Ansible Fact
Use the following ad-hoc command to check ansible fact
` ansible clientName -m setup `

Can also do it in playbook. Google ansible fact.


## Ansible Construct
https://rhtapps.redhat.com/promo/course/do007?segment=6
Use conditions such as below in playbook. Similar to if conditional statement
* when
* include

Example1: We can combine register: with when: statement
Example2: We can use notfiy: with handlers:

## Ansible Template
Good quick intro to templating
https://rhtapps.redhat.com/promo/course/do007?segment=7

Template uses variables to replace what exists in it.

## Ansible Roles
A Role --> An aggregated playbook
We can call a role to do something for us.

## Automation Controller - Replaced Ansbile Tower
Ansible Automation Controller is just one product of Ansible Automation Platform.



