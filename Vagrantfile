# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # configura proxy se necessário e o plugin estiver instalado
  if Vagrant.has_plugin?("vagrant-proxyconf")
    #vagrant plugin install vagrant-proxyconf (check https://tmatilai.github.io/vagrant-proxyconf/)
    config.proxy.http     = "http://10.122.19.54:5865"
    config.proxy.https    = "http://10.122.19.54:5865"
    config.proxy.no_proxy = "localhost, 127.0.0.1, .hacklab"
  end

  # box para todas as VM
  config.vm.box = "puppetlabs/centos-7.2-64-puppet"
  #config.vm.box_check_update = false

  # puppet server + puppet agent
  config.vm.define "puppetserver-pcpm" do |puppetserver|
    puppetserver.vm.hostname = "puppet-pcpm.hacklab"
    puppetserver.vm.network :private_network, ip: "192.168.251.20"
    puppetserver.vm.provision "shell", path: "install.sh"
    puppetserver.vm.provision :hosts do |provisioner|
      provisioner.autoconfigure = true
      provisioner.sync_hosts = true
    end
    puppetserver.vm.provider "virtualbox" do |v|
      v.customize [ "modifyvm", :id, "--cpus", "2" ]
      v.customize [ "modifyvm", :id, "--memory", "2048" ]
    end
  end
end
