# -*- mode: ruby -*-
# vi: set ft=ruby :

# This script to install Kubernetes will get executed after we have provisioned the box
$script = <<-SCRIPT

# kubelet requires swap off
swapoff -a

systemctl restart docker
systemctl restart kubelet

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "k8s-vagrant"
  config.vm.hostname = "mini-k8s"
  config.vm.network "private_network", ip: "172.28.128.3"
  config.vm.provision "shell", inline: $script
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "4096"
     vb.cpus = "2"
   end
end

#kubectl create deployment --image nginx my-nginx
#kubectl expose deployment my-nginx --port=80 --type=NodePort
