# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.box_check_update = false
  config.vm.hostname = "vm1-otus"
  config.vm.define "vm1-otus"
  # config.ssh.password = "vagrant"
  # config.ssh.insert_key = true
  # config.ssh.private_key_path = ['~/.ssh/id_rsa.pub']
  # config.vm.network "forwarded_port", guest: 22, host: 2222, host_ip: "127.0.0.1"

  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "public_network", ip: "192.168.190.22"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true

     vb.name = "vm1-otus"
     vb.cpus = "2"
     vb.memory = "2048"
   end

  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "ansible" do |ansible|
  # ansible = vm1-otus.yml
  
	config.vm.provision "shell", inline: <<-SHELL
   
  mkdir -p ~root/.ssh
  cp ~vagrant/.ssh/auth* ~root/.ssh
  sudo sed -i 's/\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
  systemctl restart sshd

  # useradd -m -s /bin/bash -U ivan	
  #  apt-get update
  #  apt-get install -y nginx
    
  SHELL

end