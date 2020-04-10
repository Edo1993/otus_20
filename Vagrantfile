# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :inetRouter => {
        :box_name => "centos/6",
        #:public => {:ip => '10.10.10.1', :adapter => 1},
        :net => [
                   {adapter: 2, virtualbox__intnet: "router-net"},
                   {adapter: 3, virtualbox__intnet: "router-net"},
                ]
  },
  :centralRouter => {
        :box_name => "centos/7",
        :net => [
                   {adapter: 2, virtualbox__intnet: "router-net"},
                   {adapter: 3, virtualbox__intnet: "router-net"},
                   {adapter: 4, virtualbox__intnet: "testLAN"},
                ]
  },
  
  :testClient1 => {
        :box_name => "centos/7",
        :net => [
                   {adapter: 2, virtualbox__intnet: "testLAN"},
                ]
  },
  :testClient2 => {
        :box_name => "centos/7",
        :net => [
                   {adapter: 2, virtualbox__intnet: "testLAN"},
                ]
  },
  :testServer1 => {
        :box_name => "centos/7",
        :net => [
                   {adapter: 2, virtualbox__intnet: "testLAN"},
                ]
  },
  :testServer2 => {
        :box_name => "centos/7",
        :net => [
                   {adapter: 2, virtualbox__intnet: "testLAN"},
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
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end
        
        case boxname.to_s
        when "inetRouter"
          box.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook.yml"
          end            
        when "centralRouter"
           box.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook.yml"
          end            
        when "testClient1"
          box.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook.yml"
          end            
        when "testClient2"
          box.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook.yml"
          end            
        when "testServer1"
          box.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook.yml"
          end            
        when "testServer2"
          box.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook.yml"
          end            
        end

      end

  end
end
