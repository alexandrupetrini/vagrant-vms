# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.

  config.vm.define "server" do |server|
    server.vm.box = "wg-vpn"
    server.vm.box = "generic/ubuntu2004"
    server.vm.network "public_network"
    server.vm.box_check_update = false

    server.vm.synced_folder "./", "/vagrant_data", disabled: true
    ENV['VAGRANT_DEFAULT_PROVIDER'] = 'hyperv'

    server.vm.provider :hyperv do |sv, override|
      sv.linked_clone = false
      sv.enable_virtualization_extensions = true
      sv.maxmemory = 1024
      sv.memory = 1024
      sv.cpus = 1
    end
  end

  config.vm.define "client" do |client|
    client.vm.box = "nginx"
    client.vm.box = "generic/ubuntu2004"
    client.vm.network "public_network"
    client.vm.box_check_update = false

    client.vm.synced_folder "./", "/vagrant_data", disabled: true
    ENV['VAGRANT_DEFAULT_PROVIDER'] = 'hyperv'

    client.vm.provider :hyperv do |cl, override|
      cl.linked_clone = false
      cl.enable_virtualization_extensions = true
      cl.maxmemory = 1024
      cl.memory = 1024
      cl.cpus = 1
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    yes | apt-get upgrade
    yes | apt-get install git isc-dhcp-server wpasupplicant
    yes | apt-get install linux-headers-$(uname --kernel-release)
    yes | apt-get install wireguard wireguard-tools wireguard-dkms
    update-alternatives --set editor /usr/bin/vim.basic
  SHELL
end
