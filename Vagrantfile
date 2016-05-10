# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# vagrant-proxyconf: https://tmatilai.github.io/vagrant-proxyconf
# if necessary set environment variable PROXY|HTTP_PROXY|HTTPS_PROXY="http://proxy:port"
if ENV.key?('PROXY')
  HTTP_PROXY=ENV['PROXY']
elsif ENV.key?('proxy')
  HTTP_PROXY=ENV['proxy']
elsif ENV.key?('HTTP_PROXY')
  HTTP_PROXY = ENV['HTTP_PROXY']
elsif ENV.key?('http_proxy')
  HTTP_PROXY = ENV['http_proxy']
elsif ENV.key?('HTTPS_PROXY')
  HTTP_PROXY = ENV['HTTPS_PROXY']
elsif ENV.key?('https_proxy')
  HTTP_PROXY = ENV['https_proxy']
else
  print "WARN: you installed vagrant-proxyconf plugin, but proxy environment variable (PROXY|HTTP_PROXY|HTTPS_PROXY) not set yet!\n"
  HTTP_PROXY="http://proxy_not_set:3128"
end
HTTPS_PROXY=HTTP_PROXY

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # configura proxy se necessário e o plugin estiver instalado
  if Vagrant.has_plugin?("vagrant-proxyconf")
    #vagrant plugin install vagrant-proxyconf (check https://tmatilai.github.io/vagrant-proxyconf/)
    config.proxy.http     = HTTP_PROXY
    config.proxy.https    = HTTPS_PROXY
    config.proxy.no_proxy = "localhost, 127.0.0.1, .hacklab"
  end

  # box para todas as VM
  config.vm.box = "gutocarvalho/centos7x64"

  # puppet server + puppet agent
  config.vm.define "puppetserver" do |puppetserver|
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
