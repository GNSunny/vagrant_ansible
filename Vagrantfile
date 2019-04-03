# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant.configure("2") do |config|

#   config.vm.define "control" do |control|
#     control.vm.hostname = "control"
#     control.vm.box = "centos/7"
#     control.vm.network "private_network", ip: "192.168.33.10"
#     #control.vm.network "forwarded_port", guest: 80, host: 8081
#     #control.vm.network "forwarded_port", guest: 80, host: 8081, host_ip: "127.0.0.1"
#     control.vm.synced_folder ".", "/vagrant", type: "nfs"
#     control.vm.provider "virtualbox" do |vb|
#        vb.customize ["modifyvm", :id, "--name", "control"]
#        vb.gui = false
#        vb.memory = "256"
#       end  
#    control.vm.provision "shell", inline: <<-SHELL
#      sudo yum update -y
#      sudo yum install gcc make kernel-devel -y
#      sudo yum install ansible -y 
#     SHELL
#   end
  


#   config.vm.define "web01" do |web01|
#     web01.vm.hostname = "web01"
#     web01.vm.box = "centos/7"
#     web01.vm.network "private_network", ip: "192.168.33.11"
#     web01.vm.synced_folder ".", "/vagrant", type: "nfs"
#     web01.vm.provider "virtualbox" do |vb|
#        vb.customize ["modifyvm", :id, "--name", "web01"]
#        vb.gui = false
#        vb.memory = "256"
#      end  
#     end
# end

#  create managment node
#  config.vm.define  



# Defines our Vagrant environment
#
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # create mgmt node
  config.vm.define :mgmt do |mgmt_config|
      mgmt_config.vm.box = "ubuntu/trusty64"
      mgmt_config.vm.hostname = "mgmt"
      mgmt_config.vm.network :private_network, ip: "10.0.15.10"
      #mgmt_config.vm.synced_folder ".", "/vagrant", type: "nfs"
      mgmt_config.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
      mgmt_config.vm.provision :shell, path: "bootstrap-mgmt.sh"
  end

  # create load balancer
  config.vm.define :lb do |lb_config|
      lb_config.vm.box = "ubuntu/trusty64"
      lb_config.vm.hostname = "lb"
      lb_config.vm.network :private_network, ip: "10.0.15.11"
      lb_config.vm.network "forwarded_port", guest: 80, host: 8080
      lb_config.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
  end

  # create some web servers
  # https://docs.vagrantup.com/v2/vagrantfile/tips.html
  (1..3).each do |i|
    config.vm.define "web#{i}" do |node|
        node.vm.box = "ubuntu/trusty64"
        node.vm.hostname = "web#{i}"
        node.vm.network :private_network, ip: "10.0.15.2#{i}"
        node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
        node.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
        end
    end
  end

end