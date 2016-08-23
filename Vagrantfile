# Defines our Vagrant environment
#
# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.require_version ">= 1.7.0"

Vagrant.configure("2") do |config|

  # create registry node
  config.vm.define :registry do |registry|
      registry.vm.box = "centos/7"
      registry.vm.hostname = "registry"
      registry.vm.box_url = "http://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-1605_01.VirtualBox.box"
      registry.vm.network :private_network, ip: "10.0.15.10"
      registry.vm.network "forwarded_port", guest: 8081, host: 1234, protocol: "tcp"
      registry.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
      registry.vm.synced_folder ".", "/vagrant"

      # registry.vm.provision :ansible do |ansible|
      #   ansible.verbose = "v"
      #   ansible.playbook = "site.yml"
      #   ansible.groups = {
      #     "registry" => ["registries"],
      #     "registries:vars" => {
      #       "ansible_host" => "127.0.0.1",
      #       "ansible_ssh_user" => "vagrant",
      #       "ansible_become" => "yes",
      #       "ansible_ssh_private_key_file" => ".vagrant/machines/desktop_prod/virtualbox/private_key"
      #     }
      #   }
      # end
      registry.vm.provision :ansible do |ansible|
        ansible.verbose = "v"
        ansible.playbook = "site.yml"
        ansible.groups = {
          "registry" => ["registries"],
        }
      end
  end

  # create some clients docker
  # https://docs.vagrantup.com/v2/vagrantfile/tips.html
  (1..2).each do |i|
    config.vm.define "node#{i}" do |node|
        node.vm.box = "centos/7"
        node.vm.hostname = "node#{i}"
        node.vm.box_url = "http://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-1605_01.VirtualBox.box"
        node.vm.network :private_network, ip: "10.0.15.#{20+i}"
        node.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
        end
        node.vm.synced_folder ".", "/vagrant"

        # node.vm.provision :ansible do |ansible|
        #   ansible.verbose = "v"
        #   ansible.playbook = "site.yml"
        #   ansible.groups = {
        #     "node#{i}" => ["nodes"],
        #     "nodes:vars" => {
        #       "ansible_host" => "127.0.0.1",
        #       "ansible_ssh_user" => "vagrant",
        #       "ansible_become" => "yes",
        #       "ansible_ssh_private_key_file" => ".vagrant/machines/desktop_prod/virtualbox/private_key"
        #     }
        #   }
        # end
        node.vm.provision :ansible do |ansible|
          ansible.verbose = "v"
          ansible.playbook = "site.yml"
          ansible.groups = {
            "node#{i}" => ["nodes"],
          }
        end
    end
  end
end
