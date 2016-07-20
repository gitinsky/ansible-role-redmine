# -*- mode: ruby -*-
# vi: set ft=ruby :

# defaultbox = "precise64"
defaultbox = "ubuntu/trusty64"
box = ENV['BOX'] || defaultbox
ENV['ANSIBLE_ROLES_PATH'] = "../../roles"
ENV['ANSIBLE_SSH_PIPELINING'] = "1"

Vagrant.configure(2) do |config|

  config.vm.box = box
  if box == defaultbox
  end
  
  config.vm.define "redmine14" do |redmine_cfg|
    redmine_cfg.vm.network :forwarded_port, host: 8014, guest:  8048
    redmine_cfg.vm.network "private_network", type: "dhcp"
    redmine_cfg.vm.provider :virtualbox do |v|
      v.name = "redmine-14.04"
      v.memory = 2048
      # v.gui = true
    end
  end
  
  config.vm.define "redmine12" do |redmine12_cfg|
    redmine12_cfg.vm.box='ubuntu/precise64'
    redmine12_cfg.vm.network :forwarded_port, host: 8012, guest: 8048
    redmine12_cfg.vm.provider :virtualbox do |v|
      v.name = "redmine-12.04"
      v.memory = 2048
      # v.gui = true
    end
  end
  
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "example/redmine.yml"
    # ansible.verbose = "vvv"
    ansible.sudo = true
    # ansible.tags = ['debug']
    ansible.groups = {
      "redmine" => ["redmine14"],
    }
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "example/redmine.yml"
    ansible.verbose = "vvv"
    ansible.sudo = true
    # ansible.tags = ['debug']
    ansible.groups = {
      "redmine" => ["redmine12"],
    }
  end

end
