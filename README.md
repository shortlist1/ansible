# Instructor
Mitesh
msharma@redhat.com

# Accounts
student: student
root: redhat

# ansible
Automation with ansible D0407
Day 1

# List of help commands
rht-vmctl help

# Start workstation
rht-vmctl start workstation

# Terminology
Control node/ server
Managed host/ ansible clients/ remote host

# Ansible requirements
ssh install
python 2.6 or 7
python-simplejson
winrm (windows hosts)

# Defining ranges of hosts
100.0.1.[1:5]
server[1:5].example.com

# Host gruops of groups
[apache]
192.168.1.1

[ftp]
192.168.1.2

[internet:children]
apache
ftp

# List hosts not in any group
ansible ungrouped -i inventory --list-hosts

# Options
-m = module (ping for example ansible all -m ping)
-a = argument (get date of all servers ansible all -m command -a "date")

# Module information/ help
andible-doc (with an option: -a --all, -l --list, actual module name like ping for example)

#### See if its possible to identify LB health monitors (checkout module uri for seeing if the response code is 200)
ansible netscaler -m get-url -a 'url=http://service-environement-server'

# Information on writing dynamic inventory scripts
http://docs.ansible.com/ansible/intro_dynamic_inventory.html
https://github.com/ansible/

# Configure vim to edit in indentations
edit ~/.vimrc

autcmd FileType yaml setlocal ai ts=2 sw=2 cc=3,5,7,9,11 et


### Day 2 ###

# order of preference
/etc/ansible/

# variables in bash
VAR=something
echo $VAR = something

# Naming variables
Has to start with V and there can be no spaces or dots. If you need a space you can use an underscore

# Variable scope
1. Global scope (can be defined from the command line)
2. Play scope (can be defined within playbook)
3. Host scope (can be defined in the inventory file)

# Host file example 
In hostfile
servera.lab.example.com username=bob

in inventory file

in playbook
---
- name: variable example
  hosts: all
  tasks:
     - name: print variable
       msg: "{{ username }}" is user name
       
...
# or also in playbook
---
- name: variable example
  hosts: all
  vars:
    susan:
      name: susan
      password: password1
    username1: bob
    username2: susan
    password1: something
  tasks:
     - name: copy content
       copy:
         content: User name {{ susan['name'] }}
         content: Password {{ susan['password'] }}
         dest: /tmp/user.txt
         
# Playbook to gather facts for DNS
---
- name: Play to gather IP address for DNS A-record
  hosts: all
  tasks:
    - name: copy content
      copy:
        content: "{{ ansible_fqdn }}({{ ansible_default_ipv4.address }})
        dest: /tmp/hostsandip.txt
        
        
### Day 3 ###

# Make role directory tree
sudo mkdir rolename/{defaults,tasks,files,templates} 


########################
tasks
########################
# Prob: when creating vm's with terraform ansible is unable to connect until a ping command has been issed from the ansible server to the new host and you have agreed to add the new server to the know hosts file
Solution:On ansible server add all servers in subnet to /home/administrator/.ssh/known_hosts

# Prob: when running playbooks via jenkins that use vault you get prompted for the vault password and the pipeline fails.
Solution: add vault password as a jenkins variable - https://dantehranian.wordpress.com/2015/07/24/managing-secrets-with-ansible-vault-the-missing-guide-part-2-of-2/

# Prob: Hosts file is getting large and increasingly difficult to maintain. Individual IP's are being added and its not sustainable
soultion: add subnet ranges (e.g 10.0.1.0:255) then refine with groups where required

# Prob: Once a vm has been created and playbooks run certain attributes are required for additional tasks
solution: create past_tasks to output certain variables like fqdn, IP etc

# Prob: Having to add a sleep command to wait for vmware customisation to complete before executing playbooks in jenkins pipeline. This process doesn't always take the same amount of time therefore i have to extent the sleep period to allow for this resulting in jobs taking far longer than they have to.
solution: Look at vsphere guest module http://docs.ansible.com/ansible/latest/vsphere_guest_module.html and also vmware esxi api to see if there is a command that lets you know once customization is complete

# Task: Add modules to galaxy

## TIP command to quickly crete a role skeleton folder structure
ansible-galaxy init --offline -f -p rolename /etc/ansible/roles/test1
