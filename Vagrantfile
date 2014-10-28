# -*- mode: ruby -*-
# vi: set ft=ruby :
#John R. Ray
#Provide a Vagrant ENV for the puppet Quest Guide
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
config.vm.box = "centos-65-x64-nocm"
config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/centos-65-x64-virtualbox-nocm.box"

pe_version = '3.3.0'
config.pe_build.version       = pe_version
config.pe_build.download_root = "https://s3.amazonaws.com/pe-builds/released/#{pe_version}"

## Master
config.vm.define :master do |master|

master.vm.provider :virtualbox do |v|
v.memory  = 1024
v.cpus = 2
end

master.vm.network :private_network,  ip: "10.10.100.100"
master.vm.network "forwarded_port", guest: 443, host: 8443
master.vm.hostname = 'master.puppetlabs.vm'
master.vm.provision :hosts

master.vm.provision :pe_bootstrap do |pe|
pe.role = :master
end

config.vm.provision "shell", 
	inline: "service iptables stop"
	end

## Client 
	config.vm.define :client do |client|

	client.vm.provider :virtualbox
	client.vm.network :private_network,  ip: "10.10.100.111"

	client.vm.hostname = 'production.puppetlabs.vm'
	client.vm.provision :hosts

	client.vm.provision :pe_bootstrap do |pe|
	pe.role   =  :agent
	pe.master = 'master.puppetlabs.vm'
	end
	end
end
