# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "generic/centos7"

  config.vm.define "db-server-1" do |dbserver|
    dbserver.vm.hostname = "db-server-1.example.com"
    dbserver.vm.provision "ansible" do |ansible|
      ansible.playbook = "v2/site.yml"
      ansible.config_file = "v2/ansible.cfg"
      ansible.compatibility_mode = '2.0'
      ansible.extra_vars = { 'gateway_ip' => '1.2.3.4',
                             'gateway_pass' => 'Webco123' }
    end
  end

  config.vm.define "db-server-2", autostart: false do |dbserver|
    dbserver.vm.hostname = "db-server-2.example.com"
    dbserver.vm.provision "ansible" do |ansible|
      ansible.playbook = "v2/site.yml"
      ansible.config_file = "v2/ansible.cfg"
      ansible.compatibility_mode = '2.0'
      ansible.extra_vars = { 'gateway_ip' => '1.2.3.4',
                             'gateway_pass' => 'Webco123',
                             'install' => true }
    end
  end
end
