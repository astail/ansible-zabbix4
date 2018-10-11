# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.5"

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |v|
    v.memory = 512
    v.cpus = 1
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.provision "shell", inline: "systemctl restart network.service"

  config.vm.define "zabbix401" do |zabbix4|
    zabbix4.vm.hostname = "zabbix4.vagrant.com"
    zabbix4.vm.network "private_network", ip: "192.168.33.34"
    zabbix4.vm.network :forwarded_port, host: 5111, guest: 5111

    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "site.yml"
      ansible.inventory_path = "inventory/hosts"
      ansible.limit = 'vagrant'
#      ansible.verbose = "v"
    end
  end

end
