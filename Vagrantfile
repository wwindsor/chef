# -*- mode: ruby -*-
# vi: set ft=ruby :
# 
Vagrant.configure("2") do |config|

  # Box1 CentOS
  config.vm.define "centos" do |centos|
    config.vm.box = "generic/centos9s"
    centos.vm.network "private_network", ip: "192.168.33.10"
    centos.vm.provider "virtualbox" do |vb|
      vb.name = "unir_had_t02_centos9"
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
      # Customize the amount of memory on the VM:
      vb.memory = "2048"
      # Customize the amount of vcpu on the VM:
      vb.cpus = "2"
    end
    centos.vm.provision "shell", inline: <<-SHELL
      sudo dnf update -y
      sudo dnf install zip unzip curl -y
      wget https://reflector.westga.edu/repos/chef/chef-workstation-24.12.1073-1.el8.x86_64.rpm
      sudo yum localinstall chef-workstation-24.12.1073-1.el8.x86_64.rpm -y
      #wget https://packages.chef.io/files/stable/chefdk/4.13.3/el/8/chefdk-4.13.3-1.el7.x86_64.rpm
      #sudo rpm -ivh chefdk-4.13.3-1.el7.x86_64.rpm
    SHELL
  end

  # Box2 Ubuntu 20.04
  config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.box = "bento/ubuntu-20.04"
    # ubuntu.vm.network "forwarded_port", guest: 80, host: 8080
    ubuntu.vm.network "private_network", ip: "192.168.33.20"
    ubuntu.vm.provider "virtualbox" do |vb|
      vb.name = "unir_had_t02_ubuntu20.04"
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
      # Customize the amount of memory on the VM:
      vb.memory = "2048"
      # Customize the amount of vcpu on the VM:
      vb.cpus = "2"
    end
    
    ubuntu.vm.provision "shell", inline: <<-SHELL
      apt-get update
      wget https://packages.chef.io/files/stable/chef-workstation/21.10.640/ubuntu/20.04/chef-workstation_21.10.640-1_amd64.deb
      sudo dpkg -i chef-workstation_21.10.640-1_amd64.deb
      sudo apt-get install zip unzip
    SHELL
  end

  config.vm.provision "chef_solo" do |chef|
    chef.arguments = "--chef-license accept"
    chef.cookbooks_path = 'cookbooks'
    chef.add_recipe "apache::default"
    chef.add_recipe "php::default"
    chef.add_recipe "mariadb::default"
    chef.add_recipe "wordpress::default"
  end
end
