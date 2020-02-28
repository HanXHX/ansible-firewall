# -*- mode: ruby -*-
# vi: set ft=ruby :
# vi: set tabstop=2 :
# vi: set shiftwidth=2 :

Vagrant.configure("2") do |config|

  vms = [
    [ "debian-wheezy", "debian/wheezy64" ],
    [ "debian-jessie", "debian/jessie64" ],
    [ "debian-stretch", "debian/stretch64" ],
    [ "debian-buster", "debian/buster64" ]
  ]

  config.vm.provider "virtualbox" do |v|
    v.cpus = 1
    v.memory = 256
  end

  vms.each do |vm|
    config.vm.define vm[0] do |m|
      m.vm.box = vm[1]
      m.vm.network "private_network", type: "dhcp"

      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/test.yml"
        ansible.verbose = 'vv'
        ansible.become = true
      end
    end
  end
end
