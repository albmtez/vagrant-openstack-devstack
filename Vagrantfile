# -*- mode: ruby -*-
# vi: set ft=ruby :

################################################################################
# Openstack
#
# Installed in Centos 7 using Devstack.
#
# Install guide: https://docs.openstack.org/devstack/latest/
################################################################################

# VM memory.
vm_memory = 4096
# VM number of cpus.
vm_cpus = 2

$provisioning_script = <<SCRIPT
yum update -y
yum install git bridge-utils -y
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
su - stack <<EOF
git clone https://opendev.org/openstack/devstack
cd devstack
cp samples/local.conf .
./stack.sh
EOF
SCRIPT

Vagrant.configure("2") do |config|

	config.vm.define "OpenstackDevstack" do |openstack|
		openstack.vm.box = "centos/7"
		openstack.vm.hostname = "openstack-devstack"
		openstack.vm.network "private_network", ip: "10.0.0.100"
		openstack.vm.network "forwarded_port", guest:80, host: 8088
		openstack.ssh.insert_key = false

		openstack.vm.provision "shell", inline: $provisioning_script

		openstack.vm.provider :virtualbox do |vb|
			vb.gui = false
			vb.name = "OpenstackDevstack"
			vb.memory = "#{vm_memory}"
			vb.cpus = "#{vm_cpus}"
		end

		openstack.vm.provider :libvirt do |libvirt|
			libvirt.default_prefix = "vagrant"
			libvirt.memory = "#{vm_memory}"
			libvirt.cpus = "#{vm_cpus}"
		end
	end
end
