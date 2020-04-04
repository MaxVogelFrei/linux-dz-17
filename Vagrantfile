# -*- mode: ruby -*-
# vim: set ft=ruby :
MACHINES = {
  :inetRouter => {
        :box_name => "centos/7",
        :forward => {:guest => 22, :host => 20022},		
        :net => [
                   {adapter: 2, virtualbox__intnet: "irouter1"},
                   {adapter: 3, virtualbox__intnet: "irouter2"}
				]
  },
  :centralRouter => {
        :box_name => "centos/7",
        :forward => {:guest => 22, :host => 21022},	
        :net => [
                   {adapter: 2, virtualbox__intnet: "irouter1"},
                   {adapter: 3, virtualbox__intnet: "irouter2"},
                   {adapter: 4, virtualbox__intnet: "router1"}
                ]
  },
  :Office1Router => {
        :box_name => "centos/7",
        :forward => {:guest => 22, :host => 22022},	
        :net => [
                   {adapter: 2, virtualbox__intnet: "router1"},
                   {adapter: 3, virtualbox__intnet: "vlan"}
                ]
  },
  :testServer1 => {
        :box_name => "centos/7",
        :forward => {:guest => 22, :host => 23022},	
        :net => [
                   {adapter: 2, virtualbox__intnet: "vlan"}
                ]
  },
  :testClient1 => {
        :box_name => "centos/7",
        :forward => {:guest => 22, :host => 24022},	        
	    :net => [
                   {adapter: 2, virtualbox__intnet: "vlan"}
                ]
  },
  :testServer2 => {
        :box_name => "centos/7",
        :forward => {:guest => 22, :host => 25022},	
        :net => [
                   {adapter: 2, virtualbox__intnet: "vlan"}
                ]
  },
  :testClient2 => {
        :box_name => "centos/7",
        :forward => {:guest => 22, :host => 26022},	
        :net => [
                   {adapter: 2, virtualbox__intnet: "vlan"}
                ]
  }
}

Vagrant.configure("2") do |config|

    MACHINES.each do |boxname, boxconfig|

        config.vm.define boxname do |box|

            box.vm.box = boxconfig[:box_name]
            box.vm.host_name = boxname.to_s
            box.vm.network "forwarded_port", boxconfig[:forward]

            boxconfig[:net].each do |ipconf|
                box.vm.network "private_network", ipconf
            end
            
            box.vm.provider :virtualbox do |vb|
                vb.customize ["modifyvm", :id, "--memory", "256"]
            end

            box.vm.provision "shell", inline: <<-SHELL
              mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
              sed -i '65s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
              systemctl restart sshd
            SHELL
        end
    end
end
