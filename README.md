# ansible-role-redmine

[![Build Status](https://travis-ci.org/CoffeeITWorks/ansible-role-redmine.svg?branch=master)](https://travis-ci.org/CoffeeITWorks/ansible-role-redmine)

This role performs basic redmine 3.3 /3.4 / 4.x installation with apache and passenger. You have to add https support on your own, feel free to install redmine to lxc container managed by [this role](https://github.com/gitinsky/ansible-role-lxc)

Get with ```git clone https://github.com/gitinsky/ansible-role-redmine.git roles/redmine```.

Tested on ubuntu 16.04.

Applied with ansible 2.6+

Role removes default apache config file.

## Dependency

This role depends on ruby-rvm role, you must create `requirements.yml` ans use `ansible-galaxy install -r requirements.yml`

Example file:

```yaml
- src: https://github.com/CoffeeITWorks/ansible-role-ruby-rvm.git
  name: coffeeitworks.ansible_role_ruby_rvm
```

## Vars

Check them out on [defaults/main.yml](defaults/main.yml)

You can change the redmine version with:

    redmine_svn_version: 3.4
    # This variable is used by role ruby-rvm and requires to be compatible with current redmine version
    ruby_version: '2.4.5'
    # To avoid errors like this we need to specify the bundler version:
    # rails (= 4.2.11) was resolved to 4.2.11, which depends on\n
    # bundler (< 2.0, >= 1.3.0)\n\n  Current Bundler version:\n
    # bundler (2.0.1)\nThis Gemfile requires a different version of Bundler
    # https://github.com/bundler/bundler/blob/1-17-stable/CHANGELOG.md
    # https://bundler.io/guides/bundler_2_upgrade.html#what-happens-if-my-application-needs-bundler-1-but-i-only-have-bundler-2-installed
    redmine_bundle_version: 1.17.3
    redmine_bundler_gem: 'bundler -v "{{ redmine_bundle_version }}"'

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

## Other external links

https://www.vultr.com/docs/how-to-install-redmine-on-ubuntu-16-04

## Notes about upgrading from previous redmine versions

The best option is to install a new machine with this role and move the database and data files to it, then run this role again to ensure all steps are updated for every plugin and gemfiles requirements.

But also in-place upgrade could be performed, but with a little of hand performed tasks.

For example I had to run as root:

```shell
 gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
 rm -rf /usr/local/rvm/
 rm /etc/apache2/sites-enabled/redmine.conf
 rm -rf /home/redmine/.rvm/
 rm /etc/apache2/conf-enabled/passenger.conf
 ```

 To ensure everything was clean before upgrading.

See also open issues at: https://github.com/CoffeeITWorks/ansible-role-redmine/issues
