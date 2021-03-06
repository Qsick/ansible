# -*- mode: ruby -*-
# vi: set ft=ruby :

# README
#
# Getting Started:
# 1. vagrant plugin install vagrant-hostmanager
# 2. vagrant up
# 3. vagrant ssh
#
# This should put you at the control host
#  with access, by name, to other vms
Vagrant.configure(2) do |config|
  config.hostmanager.enabled = true

  config.vm.box = "ubuntu/trusty64"

  config.vm.define "control", primary: true do |h|
    config.vm.host_name = "control"
    config.vm.synced_folder "~/code/ansible", "/home/vagrant/ansible"
    h.vm.network "private_network", ip: "192.168.135.10"
    h.vm.provision :shell, :inline => <<'EOF'
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
fi
if [ ! -d /home/vagrant/ansible/.vagrant ]; then
    mkdir -p /home/vagrant/ansible/.vagrant
fi
cp /home/vagrant/.ssh/id_rsa.pub /home/vagrant/ansible/.vagrant/control.pub

cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

chown -R vagrant:vagrant /home/vagrant/.ssh/
EOF
  end

  config.vm.define "lb01" do |h|
    config.vm.host_name = "lb01"
    config.vm.synced_folder "~/code/ansible", "/home/vagrant/ansible"
    h.vm.network "private_network", ip: "192.168.135.101"
    h.vm.provision :shell, inline: 'cat /home/vagrant/ansible/.vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "app01" do |h|
    config.vm.host_name = "app01"
    config.vm.synced_folder "~/code/ansible", "/home/vagrant/ansible"
    h.vm.network "private_network", ip: "192.168.135.111"
    h.vm.provision :shell, inline: 'cat /home/vagrant/ansible/.vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "app02" do |h|
    config.vm.host_name = "app02"
    config.vm.synced_folder "~/code/ansible", "/home/vagrant/ansible"
    h.vm.network "private_network", ip: "192.168.135.112"
    h.vm.provision :shell, inline: 'cat /home/vagrant/ansible/.vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "db01" do |h|
    config.vm.host_name = "db01"
    config.vm.synced_folder "~/code/ansible", "/home/vagrant/ansible"
    h.vm.network "private_network", ip: "192.168.135.121"
    h.vm.provision :shell, inline: 'cat /home/vagrant/ansible/.vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end
end
