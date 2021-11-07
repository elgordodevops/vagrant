# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
$script_master1 = <<-SCRIPT
		cat /etc/redhat-release
		cat /etc/system-release
		yum update -y
		yum install -y yum-utils
		yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
		yum install -y docker-ce docker-ce-cli containerd.io
		systemctl enable docker
		systemctl start docker
		yum install -y epel-release
		yum install -y ansible 
SCRIPT

$script = <<-SCRIPT
		cat /etc/redhat-release
		cat /etc/system-release
		yum update -y
		yum install -y yum-utils
		yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
		yum install -y docker-ce docker-ce-cli containerd.io
		systemctl enable docker
		systemctl start docker
SCRIPT

Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "CentOS7.box"

  config.vm.provider "hyperv" do |vms_config|
    vms_config.enable_virtualization_extensions = true
    vms_config.linked_clone = true
	vms_config.memory = 1024
	vms_config.cpus = 2
  end

  config.vm.define :master1 do |master1|
    master1.vm.provision "shell", inline: $script_master1
    master1.vm.hostname = "master1.local"
	master1.vm.network "private_network", bridge: "Default Switch"
  end
  
  config.vm.define :node1 do |node1|
    node1.vm.provision "shell", inline: $script
    node1.vm.hostname = "node1.local"
	node1.vm.network "private_network", bridge: "Default Switch"
  end
  
  config.vm.define :node2 do |node2|
    node2.vm.provision "shell", inline: $script
	node2.vm.hostname = "node2.local"
	node2.vm.network "private_network", bridge: "Default Switch"
  end

  
end

Vagrant.configure("2") do |win_config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  win_config.vm.box = "winserver.box"

  win_config.vm.provider "hyperv" do |win_config|
    win_config.enable_virtualization_extensions = true
    win_config.linked_clone = true
	win_config.memory = 3000
	win_config.cpus = 2
  end

  win_config.vm.define :winserver1 do |winserver1|

	winserver1.vm.hostname = 'winserver1'
	winserver1.vm.network "public_network", bridge: "Default Switch"
	winserver1.vm.synced_folder ".", "/vagrant", disabled: true
  end

  win_config.vm.define :winserver2 do |winserver2|

    winserver2.vm.hostname = 'winserver2'
	winserver2.vm.network "public_network", bridge: "Default Switch"
	winserver2.vm.synced_folder ".", "/vagrant", disabled: true
  end 
  
end 