# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'rubygems'
require 'json'

Vagrant.configure("2") do |config|
  config.vm.provider 'virtualbox' do |vb|
    vb.customize ["modifyvm", :id, "--memory", 1024]
  end
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise32"

  config.ssh.forward_agent = true

  # Forward a port from the guest to the host, which allows for outside
  # computers to access the VM, whereas host only networking does not.

  config.vm.network :forwarded_port, host: 9000,  guest: 80
  config.vm.network :forwarded_port, host: 50070, guest: 50070
  config.vm.network :forwarded_port, host: 50060, guest: 50060
  config.vm.network :forwarded_port, host: 50030, guest: 50030
  config.vm.network :forwarded_port, host: 50075, guest: 50075
  config.vm.network :forwarded_port, host: 10000, guest: 10000
  config.vm.network :forwarded_port, host: 50020, guest: 50020
  config.vm.network :forwarded_port, host: 50090, guest: 50090
  config.vm.network :forwarded_port, host: 50010, guest: 50010

  VAGRANT_JSON = JSON.parse(Pathname(__FILE__).dirname.join('nodes', 'vagrant.json').read)

  config.vm.provision :shell, :inline => "apt-get -y update && apt-get -y install libmysqlclient-dev && apt-get -y install vim"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["site-cookbooks", "cookbooks"]
    chef.roles_path = "roles"
    chef.data_bags_path = "data_bags"
    chef.provisioning_path = "/tmp/vagrant-chef"

    # You may also specify custom JSON attributes:
    chef.json = VAGRANT_JSON
    VAGRANT_JSON['run_list'].each do |recipe|
      chef.add_recipe(recipe)
    end if VAGRANT_JSON['run_list']
  end
end
