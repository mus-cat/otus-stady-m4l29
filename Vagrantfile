# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter1 => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "r1rC"}
                ],
	:config => "inetRouter1.yml"
  },
:inetRouter2 => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.1.6', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "r2rC"},
                   {ip: '192.168.56.33', adapter: 3, netmask: "255.255.255.0", name: "vboxnet0"}
                ],
	:config => "inetRouter2.yml"
  },
  :centralRouter => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.1.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "r1rC"},
                   {ip: '192.168.1.5', adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "r2rC"},
                   {ip: '192.168.0.1', adapter: 4, netmask: "255.255.255.0", virtualbox__intnet: "lan"}
                ],
	:config => "centralRouter.yml"
  },
  :centralServer => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "lan"}
                ],
	:config => "server.yml"
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s
	
	box.vm.provider "virtualbox" do |v|
        	# Set VM RAM size and CPU count
        	v.memory = "512"
        	v.cpus = "1"
     	 end

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end

	if boxconfig[:config].length > 0
      		box.vm.provision "ansible" do |ansible|
	          ansible.playbook = boxconfig[:config]
       		  ansible.verbose = "v"
		  #if boxname.to_s.include? "Server"
		  #  	  ansible.extra_vars = { work_ip: boxconfig[:net][0][:ip]}
		  #end
		end
	end
      end
  end
end

