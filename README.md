# ansible-role-redmine

[![Build Status](https://travis-ci.org/CoffeeITWorks/ansible-role-redmine.svg?branch=master)](https://travis-ci.org/CoffeeITWorks/ansible-role-redmine)

This role performs basic redmine 3.3.0 installation with apache and passenger. You have to add https support on your own, feel free to install redmine to lxc container managed by [this role](https://github.com/gitinsky/ansible-role-lxc)

Get with ```git clone https://github.com/gitinsky/ansible-role-redmine.git roles/redmine```.

Tested on ubuntu 14.04.

Ubuntu 12.04 support is currently broken due to sudoers issues.

Applied with ansible 1.8.4.

Repository support is not implemented yet.

Role removes default apache config file.

## Dependency

This role depends on ruby-rvm role, you must create `requirements.yml` ans use `ansible-galaxy install -r requirements.yml`

Example file:

```yaml
- src: https://github.com/CoffeeITWorks/ansible-role-ruby-rvm.git
  name: ruby-rvm
```

## Vars

Check them out on [defaults/main.yml](defaults/main.yml)

You can change the redmine version with:

    redmine_svn_version: 3.3

## Oficial documentation

http://www.redmine.org/projects/redmine/wiki/HowToInstallRedmineOnUbuntuServer

## Example playbook

```yaml
- hosts: redmine_servers
  become: yes
  become_method: sudo
  vars:
    ruby_version: '2.3.6'  # Depends on your redmine version
  environment: "{{ proxy_env }}"
  roles:

    - role: ansible_redmine
      tags: [ "redmine_servers" ]
      # This role requires ruby-rvm

    - role: ansible_redmine_plugins
      tags: [ "redmine_servers", "redmine_servers_plugins"]

    - role: ansible_redmine_git_sync
      tags: [ "redmine_servers", "redmine_servers_git_sync"]

    - role: ansible_redmine_emails
      tags: [ "redmine_servers", "redmine_servers_emails"]

    #- role: ansible_redmine_backup
    #  tags: [ "redmine_servers", "redmine_servers_backup"]

    - role: postfix_client
      tags: [ "postfix_clients", "redmine_servers_all" ]
```
