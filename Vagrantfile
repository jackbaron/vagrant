# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.network :private_network, ip: "192.168.33.50"
  config.vm.synced_folder "C:/ws_php", "/var/www/html", owner: "vagrant", group: "vagrant", :mount_options => ["dmode=777", "fmode=777"]
end