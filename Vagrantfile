# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

# Rreading of guestVM configuration for the YML format file.
setting = YAML.load_file('conf-vbox-guestvm.yml')

# Set the hash variable
arr_guestvm_h = setting['guestvm']

# Define Vagrant ver.
VAGRANTFILE_API_VERSION = '2'

# Define Vagrantfile
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Use host's(macbook) proxy and dns   
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  # Define the settings for each guest OS and VM
  arr_guestvm_h.each do |guestvm|

    config.vm.define guestvm['name'] do |server|

      # Define guest OS setting
      server.vm.hostname              = guestvm['name']

      # Define box for vsphere
      server.vm.box                   = guestvm['box']

      if guestvm.has_key?('box_url') && !guestvm['box_url'].nil?
        server.vm.box_url             = guestvm['box_url']
      end

      if guestvm.has_key?('ipaddress') && !guestvm['ipaddress'].nil?
        server.vm.network               'private_network', ip: guestvm['ipaddress']
      end

      if guestvm.has_key?('os_type') && guestvm['os_type'] == 'linux'
#        server.vm.communicator        = :ssh
#        server.ssh.insert_key         = false
#        server.ssh.private_key_path   = '~/.ssh/id_rsa'
        if guestvm.has_key?('os_username') && !guestvm['os_username'] .nil?
          server.ssh.username         = guestvm['os_username']
          if guestvm.has_key?('os_password') && !guestvm['os_password']
            server.ssh.password       = guestvm['os_password']
          end
        end
      end
      if guestvm.has_key?('os_type') && guestvm['os_type'] == 'windows'
        server.vm.communicator        = :winrm
        server.vm.guest               = :windows
        if guestvm.has_key?('os_username') && !guestvm['os_username'].nil? 
          server.winrm.username       = guestvm['os_username']
          if guestvm.has_key?('os_password') && !guestvm['os_password'].nil?
            server.winrm.password     = guestvm['os_password']
          else
            puts "Input  #{guestvm['name']} #{guestvm['os_username']} ospassword:\n"
            server.winrm.password     = STDIN.noecho(&:gets).chop
          end
        end
      end

      # provisioning guest vm with ansible
      if guestvm.has_key?('ansible_playbook') && !guestvm['ansible_playbook'].nil?
        server.vm.provision "ansible" do |ansible|
          ansible.playbook            = guestvm['ansible_playbook']
          if guestvm.has_key?('ansible_inventory_path')  && !guestvm['ansible_inventory_path'].nil?
            ansible.inventory_path    = guestvm['ansible_inventory_path']
            ansible.limit             = guestvm['name']
          end
        end
      end
    end
  end
end
