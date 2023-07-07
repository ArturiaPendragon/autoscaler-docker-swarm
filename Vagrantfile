# -*- mode: ruby -*-
# vi: set ft=ruby :

NODES = 3

Vagrant.configure("2") do |config|
  config.vm.box = "generic/debian11"

  (1..NODES).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.hostname = "DOC-SWM-0#{i}"
      node.vm.network "public_network", bridge: 'External'

      node.vm.provider "hyperv" do |h|
        h.linked_clone = true
        h.enable_virtualization_extensions = true
        h.vmname = "DOC-SWM-0#{i}"
        h.cpus = 1
        h.memory = 2048
        h.maxmemory = 2048
        h.vm_integration_services = {
          time_synchronization: true
        }
      end

      node.vm.provision "shell", inline: <<-SHELL
        apt-get update -qq
        apt-get install -y -qq git vim htop passwd tree curl wget nano
        curl -fsSL https://get.docker.com -o get-docker.sh
        sudo CHANNEL=stable VERSION=20.10.8 sh get-docker.sh
      SHELL

      if i == 1
        node.vm.provision "shell", inline: <<-SHELL
          IP_ADDR=$(hostname -I | awk '{print $1}')
          echo $IP_ADDR > /home/vagrant/manager-ip
          docker swarm init --advertise-addr $IP_ADDR
          docker swarm join-token -q worker
          docker swarm join-token -q manager
        SHELL
      end
    end
  end
end
