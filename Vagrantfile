# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "controller" do |controller_config|
    controller_config.vm.box = "bento/ubuntu-24.04"
    controller_config.vm.hostname = "controller"
    controller_config.vm.network "private_network", ip: "192.168.56.10"
    controller_config.vm.synced_folder "./ansible", "/home/vagrant/ansible"
    controller_config.vm.provider "vmware_desktop" do |v|
      v.gui = false
      v.memory = 2048
      v.cpus = 2
    end
    controller_config.vm.provision "shell" do |provision|
      provision.path = "provision_env.sh"
    end
    controller_config.vm.provision "shell" do |provision|
      provision.path = "provision_ansible.sh"
    end
  end

  (1..3).each do |i|
    config.vm.define "node0#{i}" do |machine|
      machine.vm.box = "bento/ubuntu-22.04-arm64"
      machine.vm.hostname = "node0#{i}"
      machine.vm.network "private_network", ip: "192.168.56.1#{i}"
      machine.vm.provider "vmware_desktop" do |v|
        v.gui = false
        v.memory = 2048
        v.cpus = 2
      end
      machine.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
    end
  end
end
