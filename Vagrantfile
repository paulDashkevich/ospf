# -*- mode: ruby -*-
# vim: set ft=ruby :
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:r1 => {
        :box_name => "centos/7",
        :net => [
		   {ip: '11.10.0.1', adapter: 4, netmask: "255.255.255.0", virtualbox__intnet: "area1"},
                   {ip: '12.0.0.1',  adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "vlan12"},
		   {ip: '14.0.0.2',  adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "vlan14"},
                ]
  },

:r2 => {
        :box_name => "centos/7",
        :net => [
		   {ip: '11.20.0.1', adapter: 4, netmask: "255.255.255.0", virtualbox__intnet: "area2"},
                   {ip: '12.0.0.2',  adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "vlan12"},
	           {ip: '13.0.0.1',  adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "vlan13"}, 
               ]
  },
  
:r3 => {
        :box_name => "centos/7",
        :net => [
		   {ip: '11.30.0.1', adapter: 4, netmask: "255.255.255.0", virtualbox__intnet: "area3"},
                   {ip: '13.0.0.2',  adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "vlan13"},
	 	   {ip: '14.0.0.1',  adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "vlan14"},
                ]
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|
        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s
        boxconfig[:net].each do |ipconf|
        box.vm.network "private_network", ipconf
        end
        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
          cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
        
        case boxname.to_s
        when "r1"
	box.vm.provision "ansible" do |ansible|
	ansible.playbook = "r1.yml"
	end
        when "r2"
	box.vm.provision "ansible" do |ansible|
	ansible.playbook = "r2.yml"
	end
	when "r3"
	box.vm.provision "ansible" do |ansible|
        ansible.playbook = "r3.yml"
	end
	end
      end
   end
end
