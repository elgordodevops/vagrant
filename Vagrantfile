# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

#----VMWARE_WORSTATION_CONFIG
#vms_config.vm.provider "vmware_desktop" do |vms_config|
#	vms_config.vmx["memsize"] = "2000"
#	vms_config.vmx["numvcpus"] = "4"
#master1.vm.network "public_network", ip: "192.168.0.100", bridge: "Realtek PCIe 2.5GbE Family Controller", :adapter => 2


#----VIRTUALBOX_CONFIG
#vms_config.vm.provider "virtualbox" do |vms_config|
#	vms_config.memory= "2000"
#	vms_config.cpus= "4"
#master1.vm.network "public_network", ip: "192.168.0.100", bridge: "Realtek PCIe 2.5GbE Family Controller", :adapter => 2


#---HYPERV_CONFIG
#vms_config.vm.provider "hyperv" do |vms_config|
#	vms_config.memory= "2000"
#	vms_config.cpus= "4"
#   vms_config.enable_virtualization_extensions = true

#master1.vm.network "public_network", ip: "192.168.0.100", bridge: "VirtualSwitchName"
#master1.vmname = "nombre"
#master1.vm.network "public_network", ip: "192.168.0.100", bridge: "Realtek PCIe 2.5GbE Family Controller", :adapter => 2



##################################################################################
$ubuntu = <<-SCRIPT
		lsb-release
		apt update -y
		apt install -y apt-utils
		apt-get install	ca-certificates
		apt-get install curl
		apt-get install gnupg
		curl https://releases.rancher.com/install-docker/20.10.sh | sh
		systemctl start docker
		systemctl enable docker
		sed -i "/^[^#]*PasswordAuthentication[[:space:]]no/c\PasswordAuthentication yes" /etc/ssh/sshd_config
		systemctl restart sshd
		timedatectl set-timezone America/Argentina/Buenos_Aires
		systemctl stop firewalld
		systemctl disable firewalld
		systemctl mask --now firewalld
		sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/config
		ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
		hostname -I | cut -d' ' -f1
		reboot
SCRIPT

$centos7 = <<-SCRIPT
		cat /etc/redhat-release
		cat /etc/system-release
		yum update -y
		yum install -y yum-utils
		yum install -y nano
		yum install -y htop
		curl https://releases.rancher.com/install-docker/20.10.sh | sh
		systemctl start docker
		systemctl enable docker
		sed -i "/^[^#]*PasswordAuthentication[[:space:]]no/c\PasswordAuthentication yes" /etc/ssh/sshd_config
		systemctl restart sshd
		timedatectl set-timezone America/Argentina/Buenos_Aires
		systemctl stop firewalld
		systemctl disable firewalld
		systemctl mask --now firewalld
		sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/config
		ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
		hostname -I | cut -d' ' -f1
		sysctl net.bridge.bridge-nf-call-iptables=1
		reboot
SCRIPT

Vagrant.configure("2") do |vms_config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  vms_config.vm.box = "hypervUbuntu2004.box"

  vms_config.vm.provider "hyperv" do |vms_config|
	vms_config.memory= 2000
	vms_config.maxmemory = 4000
	vms_config.cpus= 4
    vms_config.enable_virtualization_extensions = true
  end

  vms_config.vm.define :master1 do |master1|
    master1.vm.provision "shell", inline: $ubuntu
	#master1.vmname = "master1"
	master1.vm.hostname = "master1.local"
	master1.vm.network "private_network", ip: "192.168.0.100", bridge: "BridgeExternal"	
  end
  
  vms_config.vm.define :node1 do |node1|
	node1.vm.provision "shell", inline: $centos7
	#node1.vmname = "node1"
	node1.vm.hostname = "node1.local"
	node1.vm.network "public_network", ip: "192.168.0.185", bridge: "BridgeExternal"
  end
  
  vms_config.vm.define :node2 do |node2|
	node2.vm.provision "shell", inline: $centos7
	#node2.vmname = "node2"
	node2.vm.hostname = "node2.local"
	node2.vm.network "public_network", ip: "192.168.0.186", bridge: "BridgeExternal"
  end

  vms_config.vm.define :node3 do |node3|
	node3.vm.provision "shell", inline: $centos7
	#node3.vmname = "node3"
	node3.vm.hostname = "node3.local"
	node3.vm.network "public_network", ip: "192.168.0.187", bridge: "BridgeExternal"
  end
 
end



#Vagrant.configure("2") do |win_config|
#  # All Vagrant configuration is done here. The most common configuration
#  # options are documented and commented below. For a complete reference,
#  # please see the online documentation at vagrantup.com.
#
#  # Every Vagrant virtual environment requires a box to build off of.
#  win_config.vm.box = "winserver16_vmware_desktop.box"
#
#  win_config.vm.provider "vmware_desktop" do |win_config|
#  win_config.vmx["memsize"] = "2000"
#  win_config.vmx["numvcpus"] = "2"
#  end
#
#  win_config.vm.define :winserver1 do |winserver1|
#	winserver1.vm.hostname = 'winserver1'
#	winserver1.vm.network "private_network", bridge: "Default Switch"
#	winserver1.vm.synced_folder ".", "/vagrant", disabled: true
#  end
#
#  win_config.vm.define :winserver2 do |winserver2|
#
#    winserver2.vm.hostname = 'winserver2'
#	winserver2.vm.network "private_network", bridge: "Default Switch"
#	winserver2.vm.synced_folder ".", "/vagrant", disabled: true
#  end 
#  
#end
