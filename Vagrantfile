Vagrant.configure("2") do |config|
  config.vm.boot_timeout = 600
  config.vm.provision "shell", inline: <<-SHELL
	  apt update 
	  apt upgrade -y
	  echo "10.0.100.21 master-control-plane" >> /etc/hosts
	  echo "10.0.100.22 worker-node01" >> /etc/host
	  echo "10.0.100.23 worker-node02" >> /etc/host
  SHELL

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.define "master" do |master|
	master.vm.box = "ubuntu/trusty64"
	master.vm.box_check_update = false
	master.vm.hostname = "master-control-plane"
	master.vm.network "private_network", ip: "10.0.100.20"
	master.vm.provider "virtualbox" do |vb|
		vb.memory = 4096
		vb.cpus = 2
	end
  end
  
  (1..2).each do |i|
  
  config.vm.define "node0#{i}" do |node|
	node.vm.box = "ubuntu/trusty64"
	node.vm.box_check_update = false
	node.vm.hostname = "worker-node0#{i}"
	node.vm.network "private_network", ip: "10.0.100.2#{i}"
	node.vm.provider "virtualbox" do |vb|
		vb.memory = 2048
		vb.cpus = 1
	end
  end
  
  end	
end
