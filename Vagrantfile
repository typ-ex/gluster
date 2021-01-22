# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "geerlingguy/ubuntu2004"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.cpus = 1
  end
  
  # Define two VMs with static private IP addresses
  boxes = [
    { :name => "gluster1", :ip => "10.108.1.92" },
    { :name => "gluster2", :ip => "10.108.1.93" },
  ]

# run ansible script after last machine is made
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]

      if opts[:name] == "gluster2"
        config.vm.provision "ansible" do |ansible|
          ansible.playbook = "playbooks/provision.yml"
          ansible.inventory_path = "inventory"
          ansible.limit = "all"
        end
      end
    end
  end

end
