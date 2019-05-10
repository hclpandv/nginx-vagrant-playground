# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  #------------------------------------------------------------
  #------ Creating VM: a loadbalancer with haproxy installed
  #------------------------------------------------------------
  config.vm.define "nginx" do |lb|
    lb.vm.box = "bento/ubuntu-18.04"
    lb.vm.hostname = 'nginx.local'
    # Assigning IP:
    lb.vm.network "private_network", ip: "10.10.10.20"
    #------ Using Virtualbox provider for the exercise
    lb.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.customize ["modifyvm", :id, "--name", "nginx"]  
      # Customize the amount of memory on the VM:
      vb.memory = "512"      
    end
    #------ Provisioning through Ansible as per guidelines in exercise
    lb.vm.provision "ansible_local" do |ansible|
      ansible.install = true
      ansible.playbook = "nginx/deploy.yml"
      ansible.verbose = 'v'
    end
  end
  #-----------------------------------------------------------
  #------ Creating VM: a webserver Web1
  #-----------------------------------------------------------
  config.vm.define "web1" do |web1|
    web1.vm.box = "bento/ubuntu-18.04"
    web1.vm.hostname = 'web1.local'
    # Assigning IP:
    web1.vm.network "private_network", ip: "10.10.10.21"
    #------ Using Virtualbox provider for the exercise
    web1.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.customize ["modifyvm", :id, "--name", "web1"]
      # Customize the amount of memory on the VM:
      vb.memory = "512"
    end
    #------ Provisioning via shell. apache2 setup
    web1.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      sudo mv /var/www/html /var/www/_html
      sudo ln -s /vagrant/web1 /var/www/html
    SHELL
  end
  #-----------------------------------------------------------
  #------ Creating VM: a webserver Web2
  #-----------------------------------------------------------
  config.vm.define "web2.local" do |web2|
    web2.vm.box = "bento/ubuntu-18.04"
    web2.vm.hostname = 'web2'
    # Assigning IP:
    web2.vm.network "private_network", ip: "10.10.10.22"
    #------ Using Virtualbox provider for the exercise
    web2.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.customize ["modifyvm", :id, "--name", "web2"]
      # Customize the amount of memory on the VM:
      vb.memory = "512"
    end
    #------ Provisioning through Ansible as per guidelines in exercise
    web2.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      sudo mv /var/www/html /var/www/_html
      sudo ln -s /vagrant/web2 /var/www/html
    SHELL
  end

end

