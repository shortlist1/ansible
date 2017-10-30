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
edit ~/.vomrc

autcmd FileType yaml setlocal ai ts=2 sw=2 cc=3,5,7,9,11 et

