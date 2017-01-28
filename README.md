# ansible-role-redmine

This role performs basic redmine 3.3.0 installation with apache and passenger. You have to add https support on your own, feel free to install redmine to lxc container managed by [this role](https://github.com/gitinsky/ansible-role-lxc)

Get with ```git clone https://github.com/gitinsky/ansible-role-redmine.git roles/redmine```.

Tested on ubuntu 14.04.

Ubuntu 12.04 support is currently broken due to sudoers issues.

Applied with ansible 1.8.4.

Reposytory support is not implemented yet.

Role removes default apache config file.

# Oficial documentation: 

http://www.redmine.org/projects/redmine/wiki/HowToInstallRedmineOnUbuntuServer
