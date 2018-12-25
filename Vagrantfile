# -*- mode: ruby -*-
# vi: set ft=ruby :

$NIXOS_IP = "172.16.137.2"

ANSIBLE_RAW_SSH_ARGS = []

ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/nixos/virtualbox/private_key "

Vagrant.configure("2") do |config|
  config.vm.define "nixos" do |nixos|
    nixos.vm.box = "ubuntu/bionic64"
    nixos.vm.hostname = "nixos"
    nixos.vm.network "private_network", ip: $NIXOS_IP

    nixos.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    nixos.vm.provision "shell", inline: "add-apt-repository universe"
    nixos.vm.provision "shell", inline: "apt-get update"
    nixos.vm.provision "shell", inline: "apt-get install -y python expect"
    nixos.vm.provision "shell", inline: "if [ ! -d /root/nixos-in-place ]; then git clone https://github.com/jeaye/nixos-in-place.git /root/nixos-in-place; fi"
    nixos.vm.provision "shell", inline: "whoami"
    nixos.vm.provision "shell", inline: "cd /root/nixos-in-place/ && expect -c 'set timeout -1; spawn bash /root/nixos-in-place/install; expect \"Continue?\"; send \"y\\r\"; expect eof'"
  end
end
