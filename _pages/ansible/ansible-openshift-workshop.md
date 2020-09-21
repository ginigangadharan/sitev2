tb/ansible
- add intro with pre-req; minimum linux experience for managing systems and commands

Playbook
#update part 1 based on pdf
Consider ssh hooks load for controlnodes

Add pre-req and ssh-key thing as 2.1
=====================
# update with user creation
useradd devops
To setup an easy password :
echo abc |passwd --stdin devops
add more details of remote user creation and 

echo "suser ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/suser


inventory
==========
add how to mention inventory in ansible.cfg. mention the same in deployment chapter as well.

ad-hoc
===================
add basic ansible command pattern with details from PDF


playbook
========
correct the "Start with — " thing as proper ---
give a dummy playbook strucure

play1
  task
    -args
  task
    -args    
  task
    -args
    
play1
  task
    -args
  task
    -args    
  task
    -args
    
Installing a module :
===================================
https://blog.toast38coza.me/custom-ansible-module-hello-world/
An excellent general guide to writing modules (I've no connection to the author) can be found here: http://blog.toast38coza.me/custom-ansible-module-hello-world/

The quickest way is to simply have a folder called library/ in the same folder as your playbook. Inside this folder, place the python script for the Ansible Module. You should now have a corresponding task available to your playbook.

If you want to share your module across multiple projects, then you can add an entry to /etc/ansible/ansible.cfg pointing to a shared library location, eg:

library        = /usr/share/ansible/library



openshift
======================
